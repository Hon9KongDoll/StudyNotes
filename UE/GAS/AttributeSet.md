```c++
#define ATTRIBUTE_ACCESSORS(ClassName, PropertyName) \
	GAMEPLAYATTRIBUTE_PROPERTY_GETTER(ClassName, PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_GETTER(PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_SETTER(PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_INITTER(PropertyName)

/*
ATTRIBUTE_ACCESSORS(ClassName, PropertyName) 本质是：
	为 Attribute 自动生成4个辅助函数
	PROPERTY_GETTER：返回 FGameplayAttribute
	VALUE_GETTER：   获取当前值
	VALUE_SETTER：   设置值
	VALUE_INITTER：  初始化值
*/
```



**属性写法模板**

```c++
.h

UFUNCTION()
void OnRep_Health(const FGameplayAttributeData& OldHealth)

UPROPERTY(BlueprintReadOnly, ReplicatedUsing=OnRep_Health)
FGameplayAttributeData Health;
ATTRIBUTE_ACCESSORS(UAuraAttributeSet, Health)
```

```C++
.cpp

void UAuraAttributeSet::OnRep_Health(const FGameplayAttributeData& OldHealth)
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UAuraAttributeSet, Health, OldHealth);
}

// GAMEPLAYATTRIBUTE_REPNOTIFY宏作用
// 1. 通知 AbilitySystem 属性改变，GAS内部监听器能收到事件
// 2. 正确广播 Attribute Change
// 3. 封装底层调用，本质上内部做的事情类似：SetBaseAttributeValueFromReplication(...);
```

