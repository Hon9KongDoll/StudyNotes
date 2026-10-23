**void AddGameFrameworkComponentReceiver(AActor* Receiver, bool bAddOnlyInGameWorlds = true)**

```c++
/**
 * 将一个 Actor 注册为组件接收者，使 Modular Gameplay / Game Feature 系统能够在该 Actor 上动态添  	  加组件，并接收扩展事件。

 * Receiver：要注册的 Actor，例如 ACharacter、APawn
 * bAddOnlyInGameWorlds：否仅在游戏 World 中执行注册。默认 true，避免在编辑器等非游戏 World 中进行操作。
 */
```



**void SendGameFrameworkComponentExtensionEvent(AActor* Receiver, const FName& EventName, bool bOnlyInGameWorlds = true)**

```c++
/**
 * 向指定的 Actor 发送一个扩展事件，通知已经注册的 Extension Handler 执行对应的回调逻辑。

 * Receiver：事件的目标 Actor，例如 Pawn、Character 或 HUD
 * EventName：事件名称，监听者根据它判断应该执行什么操作。
 * bOnlyInGameWorlds：是否只在游戏 World 中发送事件，默认 true。
 */
```



**void RegisterInitState(FGameplayTag NewState, bool bAddBefore, FGameplayTag ExistingState)**

```c++
/**
 * 注册一个 Actor Feature 初始化状态，并指定它在全局初始化状态序列中的位置。
   属于 UE 的初始化状态系统，主要用于协调多个组件或 Game Feature 的初始化顺序，避免一个组件依赖的数据还没准备好，另一个组件就开始初始化

 * NewState：要注册的新初始化状态
 * bAddBefore：决定新状态插入已有状态的前面还是后面。true 表示前面，false 表示后面。
 * ExistingState：作为插入位置参照的已有状态。
 */
```



**void RegisterInitStateFeature()**

```c++
/**
 * 将当前对象代表的 Feature 注册到 GameFrameworkComponentManager 的初始化状态系统中，让系统能够跟踪它的初始化状态。
 */
```



**void BindOnActorInitStateChanged(FName FeatureName, FGameplayTag RequiredState, bool bCallIfReached)**

```c++
/**
 * 绑定 Actor 初始化状态变化的监听回调，当指定 Feature 达到指定状态条件时，调用 OnActorInitStateChanged()。
 
 * FeatureName：要监听的 Feature 名称。NAME_None 通常表示不限定具体 Feature，监听该 Actor 上符合条件的 Feature 状态变化。
 * RequiredState：要求达到的初始化状态。指定后，通常只在 Feature 达到该状态或更后面的状态时触发回调。
 * bCallIfReached：如果注册监听时，目标 Feature 已经达到所需状态，是否立即调用回调。
 */
```



**bool TryToChangeInitState(FGameplayTag DesiredState)**

```c++
/**
 * 尝试将当前 Feature 的初始化状态切换到指定状态，并在满足条件时执行状态切换、更新管理器中的状态记录，以及通知相关监听者。
 
 * DesiredState：希望切换到的目标初始化状态。
 */
```



**void CheckDefaultInitializationForImplementers()**

```C++
/**
 * 遍历同一个 Actor 上其他实现了 IGameFrameworkInitStateInterface 的 Feature，并调用它们的 CheckDefaultInitialization()，让这些组件也尝试推进自己的初始化流程。
 */
```

