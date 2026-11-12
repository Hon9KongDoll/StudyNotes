### 1 UPROPERTY

#### 1.1 Replicated

​		`UPROPERTY(Replicated)`

​		该属性参与网络复制

#### 1.2 ReplicatedUsing

​		`UPROPERTY(ReplicatedUsing = OnRep_XXX)`

​		该属性参与网络复制，当该属性完成网络复制时通知 `Actor`，调用 `OnRep_XXX`

```C++
UFUNCTION()
void OnRep_ReplicateMovement();

UPROPERTY(ReplicatedUsing = OnRep_ReplicateMovement)
uint8 bReplicateMovement:1;
```

#### 1.3 ExpandBoolAsExecs

​		`UPROPERTY(Meta = (ExpandBoolAsExecs = "ReturnValue"))`

​		将 C++ 函数的 bool 返回值，转换成蓝图节点上的两个执行引脚：True 和 False



### 2 UCLASS

#### 2.1 Blueprintable

​		`UCLASS(Blueprintable)`

​		允许一个 C++ 类被蓝图继承，从而创建该类的蓝图子类



### 3 UENUM

#### 3.1 BlueprintType

​		`UENUM(BlueprintType)`

​		UE 的 Blueprint 暴露机制目前只支持 uint8 类型的枚举，只需要在枚举名称后面添加 : uint8

```C++
UENUM(BlueprintType)
enum class EEffectApplicationPolicy : uint8 {}
```







**void GetLifetimeReplicatedProps( TArray< class FLifetimeProperty > & OutLifetimeProps ) const**

```c++
// DOREPLIFETIME 增强版：支持复制条件、RepNotify行为控制
DOREPLIFETIME_CONDITION_NOTIFY(ClassName,			// 所属类
                               PropertyName,		// 复制的属性
                               Condition,			// 控制复制给谁
                               RepNotifyCondition)	// 控制什么时候调用 OnRep_Xxx
/*
 Condition：
	COND_None：          默认复制给所有相关客户端
	COND_OwnerOnly：     只复制给拥有者
  	COND_SkipOwner：     除拥有者外都复制
  	COND_InitialOnly：   只在初始同步时发送
 RepNotifyCondition：
 	REPNOTIFY_Always：   属性值相同时也调用
	REPNOTIFY_OnChanged：只有属性值变化时调用
*/
```



`DOREPLIFETIME_WITH_PARAMS_FAST`

- 使用 `FDoRepLifetimeParams` 结构体复制属性，以控制该属性的网络复制方式

- 利用编译期生成的 `ENetFields_Private` 属性索引，构建复制属性描述符，减少运行时查找和构建开销

- 优化的是复制属性的注册过程，不是网络传输速度，也不代表每次同步的数据包更小
- 不支持数组属性的注册场景，需要支持数组，考虑 `DOREPLIFETIME_WITH_PARAMS_FAST_STATIC_ARRAY`

```C++
// 注册复制属性
void AActor::GetLifetimeReplicatedProps(
    TArray< FLifetimeProperty > & OutLifetimeProps ) const
{
	FDoRepLifetimeParams Params;
    // 属性发生变化时，由游戏代码主动通知网络系统该属性需要复制
    // 减少服务器在网络复制过程中对大量属性进行重复检查的开销
	Params.bIsPushBased = true;
	Params.Condition = COND_OwnerOnly;
    DOREPLIFETIME_WITH_PARAMS_FAST(AActor, Health, Params);
}

// 修改属性时
void AActor::SetHealth(float NewHealth)
{
    if (!HasAuthority())
    {
        return;
    }
    Health = NewHealth;
	// 通知网络系统该属性已经发生变化，需要参与复制
    // 如果忘记标记 Dirty，就可能导致属性没有按预期同步
    MARK_PROPERTY_DIRTY_FROM_NAME(AActor, Health, this);
}
```

