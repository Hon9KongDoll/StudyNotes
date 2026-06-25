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

