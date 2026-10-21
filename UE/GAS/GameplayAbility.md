| Gameplay Tag             | 描述                                                         |
| :----------------------- | :----------------------------------------------------------- |
| `AbilityTags`            | 此技能具有这些标签                                           |
| `CancelAbilitiesWithTag` | 执行此技能时，会取消具有这些标签的技能                       |
| `BlockAbilitiesWithTag`  | 此技能处于激活状态期间，具有这些标签的技能将被阻止激活       |
| `ActivationOwnedTags`    | 此技能处于激活状态期间，应用于激活者的标签<br />如果在 `AbilitySystemGlobals` 中启用了 `ReplicateActivationOwnedTags`， 这些标签将通过网络复制进行同步 |
| `ActivationRequiredTags` | 只有当激活者的 `Actor` **/** `Component` 拥有所有这些标签时，才能激活此技能 |
| `ActivationBlockedTags`  | 如果激活者的 `Actor` **/** `Component` 具有这些标签中的任意一个，禁止激活此技能 |
| `SourceRequiredTags`     | 只有当源 `Actor` **/** `Component` 拥有所有这些标签时，才能激活此技能 |
| `SourceBlockedTags`      | 如果源 `Actor` **/** `Component` 具有这些标签中的任意一个，禁止激活此技能 |
| `TargetRequiredTags`     | 只有当目标 `Actor` **/** `Component` 拥有所有这些标签时，才能激活此技能 |
| `TargetBlockedTags`      | 如果目标 `Actor` **/** `Component` 具有这些标签中的任意一个，禁止激活此技能 |



**ReplicateInputDirectly**

```c++
// 如果为 true，则此技能始终会将输入按下和释放事件同步到服务器
```

