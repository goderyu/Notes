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

