# TMap

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

排序
TMap 可以进行排序。排序后，迭代映射会以排序的顺序显示元素，但下次修改映射时，排序可能会发生变化。排序是不稳定的，因此等值元素在MultiMap中可能以任何顺序出现。

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