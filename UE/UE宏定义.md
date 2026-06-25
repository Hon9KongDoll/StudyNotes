**UPROPERTY**

```
UPROPERTY(Replicated)
    变量只要在Server上修改，就会同步到客户端
UPROPERTY(ReplicatedUsing=OnRep_Xxx)
    变量同步到客户端时，会自动调用指定函数
```

​	

**GetLifetimeReplicatedProps(TArray<class FLifetimeProperty>& OutLifetimeProps) const {}**

```c++
// DOREPLIFETIME 增强版：支持复制条件、RepNotify行为控制
DOREPLIFETIME_CONDITION_NOTIFY(ClassName,			// 所属类
                               PropertyName,		// 复制的属性
                               Condition,			// 控制复制给谁
                               RepNotifyCondition)	// 控制什么时候调用 OnRep_Xxx
/*
 Condition：
	COND_None：默认复制给所有相关客户端
	COND_OwnerOnly：只复制给拥有者
  	COND_SkipOwner：除拥有者外都复制
  	COND_InitialOnly：只在初始同步时发送
 RepNotifyCondition：
 	REPNOTIFY_Always：属性值相同时也调用
	REPNOTIFY_OnChanged:只有属性值变化时调用
*/
```

