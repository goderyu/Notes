# TMap

新建C++类，基类为Actor，命名为`MyMap`。
在`MyMap.h`中声明如下：
```c++
public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	TMap<int32, FString> MyMap;
```

> 将测试代码写在构造函数中，是因为我的目的主要是熟悉这些数据结构的C++用法，我使用的是VSCode来编码调试。使用的UE版本为4.24，启动速度比4.18提升了很多。在调试时会发现UE的加载界面一旦到达71%，我在构造函数中下的断点就会触发，我便可以快速的验证代码功能逻辑。无需完全进入UE4Editor。

在`MyMap.cpp`的构造函数中写了下列代码：


```c++
	MyMap.Add(1, "Good1");
	MyMap.Add(2, "Good2");
	MyMap.Add(3, "Good3");
	MyMap.Add(4, "Good4");
	MyMap.Add(5, "Good5");
	MyMap.Add(6, "Good6");
	MyMap.CompactStable();
	bool bContains = MyMap.Contains(1);
	for(auto& Item : MyMap)
	{
		auto Key = Item.Key;
		auto Value = Item.Value;
	}
	for(auto It = MyMap.CreateIterator(); It; ++It)
	{
		auto Key = It->Key;
		auto Value = It->Value;
	}
    //Emplace 的效率始终高于 Add
    //Emplace不会创建临时变量
	MyMap.Emplace(2, "Good22");
	//Find返回值为指针类型，因此想要通过修改
	//变量来间接修改Map就需要解引用来赋值
	auto ItemValue = MyMap.Find(3);
	*ItemValue = "Good33";
	//无法通过修改ItemValueRef来改变Map里的值
	auto ItemValueRef = MyMap.FindChecked(4);
	ItemValueRef = "Good44";

	auto ItemKey = MyMap.FindKey("Good5");

	//无法通过修改ItemValueFOA1来改变Map里的值
	auto ItemValueFOA1 = MyMap.FindOrAdd(5);
	ItemValueFOA1 = "Good55";

    //如果Map中不存在该键，FindOrAdd 将返回新创建的元素
	auto ItemValueFOA2 = MyMap.FindOrAdd(7);

	//无法通过修改ItemRef来改变Map里的值
    //FindRef 不会创建新元素
    //不要被名称迷惑，FindRef 会返回与给定键关联的值副本
	auto ItemRef = MyMap.FindRef(6);
	ItemRef = "Good66";

	TArray<int32> KeyArray;
	MyMap.GenerateKeyArray(KeyArray);
	TArray<FString> ValueArray;
	MyMap.GenerateValueArray(ValueArray);

	auto AllocatedSize = MyMap.GetAllocatedSize();
	//使用GetKeys前要确保传入的参数Array是空的，
	//否则会将Map的Keys追加到Array后面。
	MyMap.GetKeys(KeyArray);

	auto ArrNum = MyMap.Num();

    //移除元素将在数据结构中留下空位
	MyMap.Remove(7);
	//Remove之后，查看调试台发现键为7的数据被清空
	//但是索引到此键的内存还在使用，显示的内容为valid
	//使用Shrink后会将valid清空
    //使用 Collapse 和 Shrink 函数可移除 TMap 中的全部slack。
    //Shrink 将从容器的末端移除所有slack，但这会在中间或开始
    //处留下空白元素。Shrink 只删除了一个无效元素，因为末端只有
    //一个空元素。要移除所有slack，首先应调用 Compact 函数，将
    //空白空间组合在一起，为调用 Shrink 做好准备。
	MyMap.Shrink();
	MyMap.Reset();
	MyMap.Shrink();

	TMap<int32, int32> TempMap;
	auto MemSize = TempMap.GetAllocatedSize();
	TempMap.Reserve(1000);
	MemSize = TempMap.GetAllocatedSize();

```

TMap可以进行排序。排序后，迭代映射会以排序的顺序显示元素，但下次修改映射时，排序可能会发生变化。排序是不稳定的，因此等值元素在MultiMap中可能以任何顺序出现。

使用 KeySort 或 ValueSort 函数可分别按键和值进行排序。两个函数均使用二进制谓词指定排序顺序：
```c++
    FruitMap.KeySort([](int32 A, int32 B) {
        return A > B; // sort keys in reverse
    });
    // FruitMap == [
    //  { Key:9, Value:"Melon"  },
    //  { Key:5, Value:"Mango"  },
    //  { Key:4, Value:"Kiwi"   },
    //  { Key:3, Value:"Orange" }
    // ]

    FruitMap.ValueSort([](const FString& A, const FString& B) {
        return A.Len() < B.Len(); // sort strings by length
    });
    // FruitMap == [
    //  { Key:4, Value:"Kiwi"   },
    //  { Key:5, Value:"Mango"  },
    //  { Key:9, Value:"Melon"  },
    //  { Key:3, Value:"Orange" }
    // ]

```

# TArray

新建C++类，基类为Actor，命名为`MyArray`。
在`MyArray.h`中声明如下：
```c++
public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	TArray<int32> MyArray;
```

在`MyArray.cpp`的构造函数中写了下列代码：

```c++
	//初始化3个元素，值均为5，注意参数顺序含义
	MyArray.Init(5, 3);
	//以下三种均是从尾部增加元素
	MyArray.Add(1);
	MyArray.Push(2);
	MyArray.Emplace(3);
	//追加
	TArray<int32> TempArr;
	TempArr.Init(0, 3);
	//MyArray.Append(TempArr);
	//唯一添加，没有的元素值才能添加成功到尾部
	MyArray.AddUnique(5);
	MyArray.AddUnique(6);
	//按索引插入元素，在索引0处插入元素7，注意顺序
	MyArray.Insert(7, 0);
	//获得大小/元素个数
	auto ArrSize = MyArray.Num();
	//扩增或缩减长度，从尾部
	MyArray.SetNum(20);
	MyArray.SetNum(ArrSize);
	
	for(auto& Item: MyArray)
	{
		auto Number = Item;
	}
	for(auto Index = 0; Index != MyArray.Num(); ++Index)
	{
		auto Number = MyArray[Index];
	}
	//从头部查找大于3的首个元素，返回值是指针
	//不要用得到的指针做指针偏移访问元素的操作
	//这里返回的指针并不是满足查找条件的所有元素的数组
	auto FindNumber = MyArray.FindByPredicate(
		[](int32 It){
		return It > 3;
	});
	//从尾部查找 MyArray.FindLastByPredicate

	//排序
	MyArray.Sort();

	MyArray.Sort([](const int A, const int B) {
		return A > B;
	});
	//不明所以...本来意思是让奇数排前偶数排后
	//知道为什么了...我取余符号用了&而不是%，这都能犯糊涂..
	MyArray.Sort([](const int A, const int B) {
		return (A % 2) > (B % 2);
	});
	//获得指向首地址的指针
	auto IntPtr = MyArray.GetData();
	for(auto Index = 0; MyArray.IsValidIndex(Index); ++Index)
	{
		auto I = IntPtr[Index];
	}
	//int32就是4个字节32位，因此ElementSize为4
	auto ElementSize = MyArray.GetTypeSize();
	//Last可以额外给索引值，从尾部的偏移量，Top只有无参一种
	auto ElemEnd = MyArray.Last();
	auto ElemEnd0 = MyArray.Last(0);
	auto ElemEnd1 = MyArray.Last(1);
	auto ElemTop = MyArray.Top();//Top是尾部第一个元素
	//但凡有一个元素满足条件就返回真
	auto bContains = MyArray.ContainsByPredicate(
		[](int32 Item) {
			return Item < 0;
		}
	);

	//写的函数符，意思是能被2整除返回真，
	//这里得到的Filter是偶数的数组
	auto Filter = MyArray.FilterByPredicate(
		[](int32 Item) {
			return ((Item % 2) == 0);
		}
	);
```

# FString

新建C++类，基类为Actor，命名为`MyString`。
在`MyString.h`中声明如下：
```c++
public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FString MyString;
```

在`MyString.cpp`的构造函数中写了下列代码：

```c++
	//创建FString
	MyString = FString(TEXT("This is my test FString"));

	//转换
	//因为FName不区分大小写，所以转换存在损耗
	auto MyName = FName(*MyString);
	auto MyText = FText::FromString(MyString);

	auto Str1 = MyName.ToString();
	auto Str2 = MyText.ToString();

	auto FloatStr = FString::SanitizeFloat(3.3f);
	auto IntStr = FString::FromInt(30);
	auto BoolStr = true ? FString(TEXT("true")) : FString(TEXT("false"));
	//字符串格式为'X= Y= Z='
	auto VectorStr = FVector(100).ToString();
	//格式为'P= Y= R='
	auto RotatorStr = FRotator(30).ToString();
	
	// auto TempObj = CreateDefaultSubobject<UStaticMesh>(TEXT("Mesh Name"));
	// auto ObjStr = TempObj ? TempObj->GetName() : FString(TEXT("None"));

	//FString转数值
	auto bBoolFromStr = BoolStr.ToBool();
	auto IntFromStr = FCString::Atoi(*IntStr);
	auto FloatFromStr = FCString::Atof(*FloatStr);

	//对比
	auto Str3 = FString(TEXT("good"));
	auto Str4 = FString(TEXT("GOOD"));
	auto bEquals1 = (Str3 == Str4);
	auto bEquals2 = (Str3.Equals(Str4, ESearchCase::CaseSensitive));
	auto bEquals3 = (Str3.Equals(Str4, ESearchCase::IgnoreCase));

	//搜索
	//FromEnd只是从字符串结尾向前找，并不是字符串每一个字符都颠倒
	//因此下列的两个搜索，bEquals4为true，bEquals5为false
	auto bEquals4 = Str3.Contains(TEXT("go"), ESearchCase::CaseSensitive, ESearchDir::FromEnd);
	auto bEquals5 = Str3.Contains(TEXT("og"), ESearchCase::CaseSensitive, ESearchDir::FromEnd);
	//最后一个参数为从哪个索引开始查找（包含该索引处的字符）
	//查找到返回该子串在整个字符串中的第一个索引值
	//查不到返回-1
	auto FindIndex1 = Str4.Find(TEXT("od"), ESearchCase::IgnoreCase, ESearchDir::FromStart, 3);
	auto FindIndex2 = Str4.Find(TEXT("od"), ESearchCase::IgnoreCase, ESearchDir::FromStart, 2);
	//使用 %s 参数包含 FStrings 时，必须使用 * 运算符返回 %s 参数所需的 TCHAR*。
	auto MakeString = FString::Printf(TEXT("%02d %.3f %s"), 30, 35.44444f, TEXT("good"));

	//TCHAR_TO_ANSI - 将引擎字符串（TCHAR*）转换为 ANSI 字符串。
	//ANSI_TO_TCHAR - 将 ANSI 字符串转换为引擎字符串（TCHAR*）。
	auto Str5 = FString(ANSI_TO_TCHAR("132"));
```