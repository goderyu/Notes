## 常用数据类型

### 类命名前缀

- 派生自AActor  prefix A, such as AController
- 派生自UObject prefix U, such as UComponent
- Enums         prefix E, such as EFortificationType
- Interface     prefix I, such as IAbilitySystemInterface
- Template      prefix T, such as TArray
- Slate UI      prefix S, such as SButton
- Other         prefix F, such as FVector

## BlueprintNativeEvent

UFUNCTION使用该属性后，可以声明一个方法，不定义，同时C++自己可以定义一个同名但是带有`_Implementation`后缀的特征标相同的函数。

注意：该属性只是能让蓝图覆写C++默认实现，想要在蓝图调用该方法还是需要额外添加BlueprintCallable属性

# BP&CPP

写了Demo测试了一下继承的使用。记一下心得：

在CPP中定义了一个基类，其定义了三个虚函数，实现是空体。如果是使用`virtual void func() = 0;`的写法，则要求继承类要全部实现，因此在基类中没有使用纯虚函数的写法，而是先留了空体。

在继承类中，使用`override`字段覆写方法，并为其指定了`UFUNCTION(BlueprintNativeEvent)`字段，可以使用。但如果是直接在父类中指定字段，会报`NativeEvent can't be virtual`样子的错误。

这之后，在UEEditor中新建蓝图类继承C++的类，实现方法，调用，有效！代码如下：

CPP基类头文件：

```c++
//AbnormalActor.h
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "AbnormalActor.generated.h"

UENUM(BlueprintType)
enum class FEnumAbnormalType : uint8
{
	ENUM_DYNAMIC_TYPE UMETA(DisplayName = "动态"),
	ENUM_STATIC_TYPE UMETA(DisplayName = "静态"),
};

UCLASS()
class ABNORMALPLUGIN_API AAbnormalActor : public AActor
{
	GENERATED_BODY()

public:
	// Sets default values for this actor's properties
	AAbnormalActor();

	UPROPERTY(BlueprintReadOnly, VisibleAnywhere, Category = Abnormal)
		FEnumAbnormalType AbnormalType;
	UPROPERTY(BlueprintReadOnly, VisibleAnywhere, Category = Abnormal)
		FString Id;

	virtual void BeforeExec() {}

	virtual void Exec() {}

	virtual void AfterExec() {}

	virtual void Do() {}
private:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;
	// Called every frame
	virtual void Tick(float DeltaTime) override;
};

```

CPP继承类头文件：

```c++
//DynamicAbnormalActor.h
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "AbnormalActor.h"
#include "Runtime/LevelSequence/Public/LevelSequence.h"
#include "DynamicAbnormalActor.generated.h"

USTRUCT(BlueprintType)
struct FDynamicAbnormalPlayInfo
{
	GENERATED_USTRUCT_BODY()

		/** 播放次数, 设置为-1表示一直循环播放 */
		UPROPERTY(BlueprintReadWrite, EditAnywhere, Category = "DefaultAbnormalBase")
		int32 PlayCount;

	/** 一段LevelCinematics的区间起始时间，单位是秒 */
	UPROPERTY(BlueprintReadWrite, EditAnywhere, Category = "DefaultAbnormalBase")
		float CinematicStartTime;

	/** 一段LevelCinematics的区间结束时间，要求它必须比起始时间值大，单位是秒 */
	UPROPERTY(BlueprintReadWrite, EditAnywhere, Category = "DefaultAbnormalBase")
		float CinematicEndTime;

	//UPROPERTY(BlueprintReadWrite, Category = "DefaultAbnormalBase")
	//	FEnumPlayState PlayState;
};
/**
 *
 */
UCLASS()
class ABNORMALPLUGIN_API ADynamicAbnormalActor : public AAbnormalActor
{
	GENERATED_BODY()

public:

	ADynamicAbnormalActor();

	/** 非正常关卡动画资源 */
	UPROPERTY(BlueprintReadWrite, EditAnywhere, Category = Abnormal, meta = (AllowedClasses = "LevelSequence"))
		class ULevelSequence* AbnormalLevelSequence;
	/** 动画播放属性表 */
	UPROPERTY(BlueprintReadWrite, EditAnywhere, Category = Abnormal)
		TMap<FString, FDynamicAbnormalPlayInfo> PlayInfoMap;

	UPROPERTY(BlueprintReadWrite, Category = Abnormal)
		FString CurrentPlayName;

	/** 创建动画播放器并按指定规则播放 */
	UFUNCTION(BlueprintImplementableEvent, BlueprintCallable, Category = Abnormal)
		void PlayLevelSequence();

	/** 继续播放动画，暂停时可调用 */
	UFUNCTION(BlueprintImplementableEvent, BlueprintCallable, Category = Abnormal)
		void PlayPlayer();

	/** 暂停动画，正在播放时可调用 */
	UFUNCTION(BlueprintImplementableEvent, BlueprintCallable, Category = Abnormal)
		void PausePlayer();

	/** 中止动画播放 */
	UFUNCTION(BlueprintImplementableEvent, BlueprintCallable, Category = Abnormal)
		void StopPlayer();

	void Test();

	UFUNCTION(BlueprintNativeEvent, BlueprintCallable)
		void BeforeExec() override;
	void BeforeExec_Implementation();
	UFUNCTION(BlueprintNativeEvent, BlueprintCallable)
		void Exec() override;
	void Exec_Implementation();
	UFUNCTION(BlueprintNativeEvent, BlueprintCallable)
		void AfterExec() override;
	void AfterExec_Implementation();

	UFUNCTION(BlueprintNativeEvent, BlueprintCallable)
		void Do() override;
	void Do_Implementation();
};

```

蓝图继承类实现：

