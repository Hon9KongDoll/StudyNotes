**Duration Policy**

```c++
// Instant : 瞬间效果
//	GE 应用时立即执行，无法进入 ASC 的 ActiveGameplayEffectsContainer
//	直接修改 Attribute 的 BaseValue


// Infinite : 永久持续
// GE 应用后，会进入 ASC 的 ActiveGameplayEffectsContainer
//	持续期间 Modifier 会影响 Attribute 的 CurrentValue

// HasDuration : 持续时间
//	GE 应用后，会进入 ASC 的 ActiveGameplayEffectsContainer，在指定时间内持续生效
//	持续期间 Modifier 会影响 Attribute 的 CurrentValue
```



 **Duration Magnitude**

```C++
// 决定 GE 持续多长时间
```



**Period**

```C++
// 决定 GameplayEffect 每隔多少秒执行一次周期性效果
// 启用 Period 后，周期性执行的 Modifier 通常修改的是 Attribute 的 BaseValue，而不是像普通持续性 Modifier 那样持续叠加到 CurrentValue 上
	ExecutePeriodicEffectOnApplication
		// GE 刚刚应用时，是否立即执行一次周期效果
```

![](UE/GameplayEffect.assets/ExecutePeriodicEffectOnApplication.png)



**Periodic Inhibition Policy**

```C++
// 当一个正在运行的 PeriodicGameplayEffect 被 Inhibit（抑制）后，已经安排的周期执行应该如何处理

// NeverReset : 保持原来的周期时间
	// Period = 2 秒
	// 0 秒：GE 开始
	// 2 秒：第一次执行
	// 3 秒：GE 被 Inhibit
	// 4 秒：原定执行时间
	// 5 秒：GE 解除 Inhibit
	// 6 秒：下一次执行
	// 解除抑制后，按照原来的周期节奏继续执行

// ResetPeriod : 重新开始计时
	// Period = 2 秒
	// 0 秒：GE 开始
	// 2 秒：第一次执行
	// 3 秒：GE 被 Inhibit
	// 5 秒：GE 解除 Inhibit
	// 7 秒：下一次执行
	// 9 秒：再次执行
	// 解除抑制后，必须重新等待完整的 2 秒

// ExecuteAndResetPeriod : 解除抑制时立即执行
	// Period = 2 秒
	// 0 秒：GE 开始
	// 2 秒：第一次执行
	// 3 秒：GE 被 Inhibit
	// 5 秒：GE 解除 Inhibit，立即执行
	// 7 秒：再次执行
	// 9 秒：再次执行
	// 解除抑制后立即执行一次周期效果，重置 Period 计时器，等待完整的 Period 后再执行下一次
```

