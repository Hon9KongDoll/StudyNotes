**EGameplayEffectReplicationMode ReplicationMode;**

```c++
// GameplayEffect 在网络多人游戏中，以什么方式同步给客户端

// 对于混合复制模式（Mixed Replication Mode）：OwnerActor 的 Owner 必须是 Controller。
// 对于 Pawn，这一设置会在 PossessedBy() 中自动完成。
// PlayerState 的 Owner 会自动设置为 Controller。
// 因此，如果你的 OwnerActor 不是 PlayerState，且使用混合复制模式，
// 则必须在 OwnerActor 上调用 SetOwner()，将其 Owner 设置为 Controller。
```

| ReplicationMode |       何时使用        |                             描述                             |
| :-------------: | :-------------------: | :----------------------------------------------------------: |
|    `Minimal`    |  多人，AI控制的Actor  | `GameplayEffect` 从不同步到任何客户端, 只有 `GameplayTag` 和 `GameplayCue` 同步到所有客户端. |
|     `Mixed`     | 多人, 玩家控制的Actor | `GameplayEffect` 只同步到其所属客户端, 只有 `GameplayTag` 和 `GameplayCue` 同步到所有客户端. |
|     `Full`      |         单人          |             所有 `GameplayEffect` 都同步到客户端             |

