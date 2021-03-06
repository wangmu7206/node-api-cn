<!-- YAML
added: v0.11.15
changes:
  - version: v14.9.0
    pr-url: https://github.com/nodejs/node/pull/34729
    description: This function is also available as `buf.readUintBE()`.
  - version: v10.0.0
    pr-url: https://github.com/nodejs/node/pull/18395
    description: Removed `noAssert` and no implicit coercion of the offset
                 and `byteLength` to `uint32` anymore.
-->

* `offset` {integer} 开始读取之前要跳过的字节数。必须满足：`0 <= offset <= buf.length - byteLength`。
* `byteLength` {integer} 要读取的字节数。必须满足：`0 < byteLength <= 6`。
* 返回: {integer}

从 `buf` 中指定的 `offset` 读取 `byteLength` 个字节，并将读取的值解析为无符号大端序的整数，最高支持 48 位精度。

```js
const buf = Buffer.from([0x12, 0x34, 0x56, 0x78, 0x90, 0xab]);

console.log(buf.readUIntBE(0, 6).toString(16));
// 打印: 1234567890ab
console.log(buf.readUIntBE(1, 6).toString(16));
// 抛出异常 ERR_OUT_OF_RANGE。
```

