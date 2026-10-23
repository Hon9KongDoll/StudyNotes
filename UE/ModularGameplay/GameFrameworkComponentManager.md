**void AddGameFrameworkComponentReceiver(AActor* Receiver, bool bAddOnlyInGameWorlds = true)**

```c++
/**
 * 将一个 Actor 注册为组件接收者，使 Modular Gameplay / Game Feature 系统能够在该 Actor 上动态添  	  加组件，并接收扩展事件。
 
   Receiver：要注册的 Actor，例如 ACharacter、APawn
   bAddOnlyInGameWorlds：否仅在游戏 World 中执行注册。默认 true，避免在编辑器等非游戏 World 中进行操作。
 */
```

