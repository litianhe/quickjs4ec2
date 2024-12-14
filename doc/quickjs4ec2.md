# 改造 QuickJS 以支持 EC2 的设计书

## 汉字关键词及语法解析

QuickJS 对关键词做了原子化处理，对大部分关键词，修改原子对应的关键词字面值即可。

具体见 [quickjs-atom.h](../quickjs-atom.h) 文件。

部分 JS 关键词和 EC2 关键词是一一匹配的，但有少部分关键词并不匹配，需要添加相应的关键词并修改解析器以支持这些关键词。

下表是 JS 和 EC2 关键词对应表：

|  JavaScript 关键词 | EC2 关键词 | 备注 |
|--                  | --         | --   |
| `if`               | `若始`     |      |
| `endif`            | `若终`     | 新增关键词并修改解析器          |
| `ifel`             | `若又`     | 增加 `ifel` 关键词并修改解析器  |
| `else`             | `若否`     |      |
| `while`            | `当始`     |      |
| `endwhile`         | `当始`     | 新增关键词并修改解析器          |
| `break`            | `跳出`     |       |
| `continue`         | `继续`     |       |
| `switch`           | `岔始`     |       |
| `endswitch`        | `岔终`     | 新增关键词并修改解析器          |
| `case`             | `岔道`     |           |
| `false`            | `假`       |           |
| `true`             | `真`       |           |
| `null`             | `空`       |           |
| `undefined`        | `未定义`   |           |
| `function`         | `函始`     |           |
| `endfunc`          | `函终`     | 新增关键词并修改解析器          |
| `algorithm`        | `算始`     | 新增关键词并修改解析器          |
| `endalgo`          | `算终`     | 新增关键词并修改解析器          |



改造内容：

1) 增加新的配置选项 `CONFIG_HANZI_KEYWORD`。

2) 调整 `DEF` 宏的定义和使用：

```c
// 在 quickjs-atom.h 中定义关键词时：
DEF(false, "false", "假")

// 在 quickjs.c 中用于生成原子值时
#define DEF(name, str, han_str) #define DEF(name, str) JS_ATOM_ ## name,

// 在 quickjs.c 中用于生成关键词字符串时
#if CONFIG_HANZI_KEYWORD
#define DEF(name, str, han_str) han_str "\0"
#else
#define DEF(name, str, han_str) str "\0"
#endif
```

3) 调整解析器，使之符合 EC2 语法要求：
   - 不使用 `{}`，支持 `若终/当终/函终/算终` 等。
   - 严格的代码块缩进要求。

4) 将 `算始/算终` 定义的函数视作入口函数处理，并自动生成调用入口函数的代码，包括：
   - 在调用之前，根据入口函数要求的参数提示用户输入对应的字符串。
   - 在调用之后，将入口函数返回的值转储到标准输出。

```
算始 你好 (name)
    输出 ("Hello, ", name)
算终
```

相当于如下 JS 代码

```js
function 你好 (name) {
    printf ("Hello, ", name);
    return true;
}

name = window.prompt("请为 `你好` 算法输入参数 name 的值：");
console.log(你好(name))
```

## 数据类型

JS 中缺乏 EC2 所需要的字节、字符等基础数据类型，需要修改 quickjs 以支持。

（未完待续）

