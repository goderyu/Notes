# error C2039

我当时出现了

```
"GetObjectW": 不是"xxx"的成员
```

在VS2015下查看输出信息和错误列表，指向的错误根源都是UE的头文件什么的，经分析应该是我的代码头文件引用顺序不对。我的代码中使用到了第三方库，而第三方库会使用一些`WindowsAPI`，因此尝试着在包含该头文件的语句前后加上一下语句，例如：

```c++
#include "AllowWindowsPlatformTypes.h"
#include "windows.h"

// 这句是我使用的第三方库头文件
#include "ThirdParty/Lib.h"

#include "HideWindowsPlatformTypes.h"
```

重新生成，报错消失...听大佬分析意思是对`WindowsAPI`有依赖，而在UE4中使用这种格式可以消除一些平台限制。也就是先屏蔽系统库，再解除屏蔽。

# 错误列表指向集中在某一个头文件

例如，我在生成解决方案时，报错有115处，所有均来自UE4的`XmlFIle.h`头文件，转到该头文件，看到它的路径为`C:\Program Files\Epic Games\UE_4.18\Engine\Source\Runtime\XmlParser\Public`，看到它是在`XmlParser`目录下的，因此怀疑是缺少添加模块依赖，在`*.Build.cs`文件中找到并添加如下代码：

```csharp
PublicDependencyModuleNames.AddRange(
    new string[]
    {
        "Core",
        "CoreUObject",
        // 添加这个模块
        "XmlParser",
        // ... add other public dependencies that you statically link with here ...
    }
);

```

再次生成，问题解决。

# UE4 Plugin "xxx" failed to load...

我的情况是这样的：开发了三款插件，假设名为`PluginA`，`PluginB`，`PluginC`。它们三者的关系是：`PluginA`依赖`PluginB`，`PluginB`依赖`PluginC`，`PluginC`依赖`PluginA`，出现了**循环依赖**的情况。导致的结果是生成解决方案通过，但是运行UEEditor在73%的进度会报`Plugin 'Plugin*' failed to load...`的错误。

首先当我发现这三个插件循环依赖后，我就感觉做插件开发并不应该把代码设计成这样，会导致耦合关系变得复杂，以后移植插件还需确保三个插件缺一不可。我的A插件是必然依赖B插件的，用到了其定义的类，才能开展功能。B插件依赖C是伙伴开发的功能也要依赖，而C插件依赖A插件只是做了**数据传递**的功能。

因此分析结束后我考虑将C插件和A插件的依赖关系进行倒置，这样就打破了循环依赖，而且以后A插件即便去除，也不影响B和C插件。思路是正确的，改完代码后也正常运行了，下面对改动代码时出现的另一个问题进行总结。

# 无法解析的外部符号，该符号在另一处被引用

错误列表截图如下：

![image-20191127125953509](illustration/UE-%E8%AF%AD%E6%B3%95%E6%B2%A1%E9%94%99%E8%AF%AF%E7%94%9F%E6%88%90%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B8%8D%E9%80%9A%E8%BF%87%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB/image-20191127125953509.png)

情况是接着上一个问题。我要在两个插件间进行**数据传递**，思路是插件C中定义一个类，类中存放需要传递的三个属性，因为在程序中只可能存在一个实例，因此我才用了单利模式。代码十分简洁，如下：

```c++
//PortConfigManager.h
#pragma once

#include <string>

class PortConfigManager
{
public:
	static PortConfigManager& instance();
private:
	PortConfigManager();
public:
	//供插件A访问的数据
	bool m_bGeneratePortInfoFileDone;
	std::string m_strGeneratedFilePath;
	std::string m_strLoadJcLevelNameStdString;

};

//PortConfigManager.cpp
#include "PortConfigManager.h"

PortConfigManager& PortConfigManager::instance()
{
	static PortConfigManager instance;
	return instance;
}

PortConfigManager::PortConfigManager()
{
	m_bGeneratePortInfoFileDone = false;
	m_strGeneratedFilePath = "";
	m_strLoadJcLevelNameStdString = "";
}
```

该类的变量我直接使用`public`修饰了，严谨点的可以设置`private`并加`set get`方法。之后我在插件A中引入插件C的`PortConfigManager.h`头文件，通过`PortConfigManager::instance().m_***`去设置变量或者获取变量值，编译就报上图的错误了。插件的`*.Build.cs`文件没有问题，该有的依赖设置都有，分析后感觉是和UE的作用域机制有关系，和原生C++略有不同。因此我将`PortConfigManager.h`文件改动了一下，部分代码如下：

```c++
//PortConfigManager.h
#pragma once

#include "CoreMinimal.h"
//...

class COMMUNICATE_API PortConfigManager
{
    //...
};
```

我添加了头文件`CoreMinimal.h`，并在类名前加了`COMMUNICATE_API`，此时再编译，通过了。因此可以得出结论，想要跨插件进行数据访问，需要对类进行修饰符限定，根据UE的规则，加上`**_API`。我之所以做这样的尝试，是因为我以前C插件依赖A插件时，在C里获取A的变量就没问题。想到A用到了修饰符限定，同时考虑到插件最后生成的结果是`*.lib *.dll`库，这修饰符限定又和C++的库形式很相似，所以做此尝试，成功解决问题。