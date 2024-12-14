# QuickJS4EC2

针对可执行考鼎码（EC2）的 QuickJS JavaScript 引擎的变种。

## 最新动态

- 2024-12-14
    - 创建仓库。

## 简介

可执行考鼎码（Executable Coding Code，EC2）是一种主要用于少儿信息学启蒙的编程语言，其主要特征为：

- 解释型脚本编程语言。
- 支持汉字关键词及命名。
- 弱类型。
   1. 基础类型：未定义、空、布尔、整数（系统位宽一致的整数）、浮点数、字节、字符、字节串、字符串。
   1. 高精度算术相关类型：任意整数（任意精度整数）、任意小数（任意精度十进制小数）、任意浮点数（任意精度浮点数）。
   1. 容器类型：序列（数组）、映射（字典）。
   1. 跨平台；优先支持 WASM（Web Assembly）平台。

EC2 的规范可见：

<https://courses.fmsoft.cn/plzs/enlightenment-spec-of-executable-coding-code.html>

[QuickJS](https://bellard.org/quickjs/) 是一个小型且可嵌入的 JavaScript 引擎，支持 [ES2023](https://tc39.github.io/ecma262/2023) 规范，包括模块、异步生成器、代理和BigInt。它可选地支持数学扩展，如任意精度十进制浮点数（BigDecimal）、任意精度二进制浮点数（BigFloat）和运算符重载。

我们选择 QuickJS 作为 EC2 的引擎基础，主要有如下几点考虑：

1. 自包含，体积小，效率适中，不需要第三方库，方便运行在 WASM 上。
1. 全球顶尖程序员的作品，高质量代码。
1. 商业友好的许可证。

## 文档

- [改造设计书](doc/quickjs4ec2.md)

## 在线游玩场

（建设中）

## 下载

（下载）

## 作者

- [魏永明](https://github.com/VincentWei)

## 许可证

QuickJS4EC2 在 MIT 许可证下发布。

