<!-- YAML
added: v0.7.1
-->

* `settings` {Object}

每当 [`.setupMaster()`] 被调用时触发。

`settings` 对象是 [`.setupMaster()`] 被调用时的 `cluster.settings` 对象，并且只能查询，因为在一个时间点内 [`.setupMaster()`] 可以被调用多次。

如果精确度十分重要，则使用 `cluster.settings`。

