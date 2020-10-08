<!-- YAML
added: v0.3.4
changes:
  - version: v14.5.0
    pr-url: https://github.com/nodejs/node/pull/33617
    description: Add `maxTotalSockets` option to agent constructor.
  - version: v14.5.0
    pr-url: https://github.com/nodejs/node/pull/33278
    description: Add `scheduling` option to specify the free socket
                 scheduling strategy.
-->

* `options` {Object} 要在代理上设置的可配置选项集。可以包含以下字段： 
  * `keepAlive` {boolean} 即使没有未完成的请求，也要保持套接字，这样它们就可以被用于将来的请求而无需重新建立 TCP 连接。
    不要与 `Connection` 请求头的 `keep-alive` 值混淆。 
    `Connection: keep-alive` 请求头始终在使用代理时发送，除非明确指定 `Connection` 请求头、或者 `keepAlive` 和 `maxSockets` 选项分别设置为 `false` 和 `Infinity`，在这种情况下将会使用 `Connection: close`。
    **默认值:** `false`。
  * `keepAliveMsecs` {number} 当使用 `keepAlive` 选项时，指定用于 TCP Keep-Alive 数据包的[初始延迟][initial delay]。当 `keepAlive` 选项为 `false` 或 `undefined` 时则忽略。**默认值:** `1000`。
  * `maxSockets` {number} 每个主机允许的套接字的最大数量。**默认值:** `Infinity`。
  * `maxTotalSockets` {number} Maximum number of sockets allowed for
    all hosts in total. Each request will use a new socket
    until the maximum is reached.
    **Default:** `Infinity`.
  * `maxFreeSockets` {number} 在空闲状态下保持打开的套接字的最大数量。仅当 `keepAlive` 被设置为 `true` 时才相关。**默认值:** `256`。
  * `scheduling` {string} 挑选下一个空闲的套接字所使用的策略。可以是“先进先出”或者“后进先出”。
    两者之间的主要差别是，后进先出选择最近所使用过的套接字，而先进先出选择最早使用过的套接字。
    In case of a low rate of request per second, the `'lifo'` scheduling
    will lower the risk of picking a socket that might have been closed
    by the server due to inactivity.
    In case of a high rate of request per second,
    the `'fifo'` scheduling will maximize the number of open sockets,
    while the `'lifo'` scheduling will keep it as low as possible.
    **Default:** `'fifo'`.
  * `timeout` {number} 套接字的超时时间，以毫秒为单位。这会在套接字被连接之后设置超时时间。

[`socket.connect()`] 中的 `options` 也受支持。
 
被 [`http.request()`] 使用的默认的 [`http.globalAgent`] 具有所有这些值且被设置为各自的默认值。

要配置其中任何一个，则必须创建自定义的 [`http.Agent`] 实例。

```js
const http = require('http');
const keepAliveAgent = new http.Agent({ keepAlive: true });
options.agent = keepAliveAgent;
http.request(options, onResponseCallback);
```

