# 调度器

## kube-scheduler Filter 执行两次的原因

````
```go
// We run filters twice in some cases. If the node has greater or equal priority
// nominated pods, we run them when those pods are added to PreFilter state and nodeInfo.
// If all filters succeed in this pass, we run them again when these
// nominated pods are not added. This second pass is necessary because some
// filters such as inter-pod affinity may not pass without the nominated pods.
// If there are no nominated pods for the node or if the first run of the
// filters fail, we don't run the second pass.
// We consider only equal or higher priority pods in the first pass, because
// those are the current "pod" must yield to them and not take a space opened
// for running them. It is ok if the current "pod" take resources freed for
// lower priority pods.
// Requiring that the new pod is schedulable in both circumstances ensures that
// we are making a conservative decision: filters like resources and inter-pod
// anti-affinity are more likely to fail when the nominated pods are treated
// as running, while filters like pod affinity are more likely to fail when
// the nominated pods are treated as not running. We can't just assume the
// nominated pods are running because they are not running right now and in fact,
// they may end up getting scheduled to a different node.
```
````

之所以可能执行两次，与 Pod 驱逐有关：

第一遍：假设优先级 >= 当前 Pod 的被提名的 Pod 运行在 Node 上，判断 Node 是否满足（比如亲和性）

如果第一遍直接挂了或者压根没有优先级 >= 当前 Pod 的被提名的 Pod，那么就不再执行第二遍

第二遍：假设优先级 >= 当前 Pod 的被提名的 Pod 没有运行在 Node 上，判断 Node 是否满足要求

之所以执行两遍，就是为了确保无论被提名的 Pod 是否在目标节点上，目标节点都满足调度要求（保险）
