# typescript

TypeScript 是 JavaScript 的超集。

TypeScript = Type + JavaScript

## 环境

首先安装 `node.js`

然后全局安装 `typescript`

```bash
npm i -g typescript
```

运行代码时需要先将 `ts` 文件转换为 `js` 文件，命令：`tsc <filename.ts>`

然后执行转换后的文件，命令：`node <filename.js>`

安装 `node-ts` 包简化该过程，`npm i -g node-ts`

直接运行 `node-ts <filename.ts>` 即可运行代码。

## 静态类型

### 基础类型

- `boolean` 表示真/假
- `number` 表示数字
- `string` 表示字符串
- `any` 表示任意类型
- `void` 表示没有任何类型
- `undefined`
- `null`
- `never` 表示那些用不存在的值的类型
- `object` 表示非原始类型，也就是除 `number`, `string`, `boolean`, `symbol`, `null`, `undefined` 之外的类型

直接跟在变量后面即可