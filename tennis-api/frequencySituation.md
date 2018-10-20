# 常见场景

## 我认为的关键环节

### 数据库操作

- 列表查询，详情查询、删除
- 新增
- 修改

原来使用 orm 是这么的方便

#### 更改了表结构之后

使用 orm 回滚，删表，建表

```bash
sequelize db:migrate:undo # 执行n次
sequelize db:migrate # 执行n次
```

使用 migrate 添加字段

### hapi 中使用`async/await`遇到错误如何更简单的处理？

看一些别人使用 hapi 的方法，应该有可以学到的吧
看到了一个插件 poop, 看起来不是我想要的

我看的教程是 v16
现在找到一篇升级教程 [hapi-v17-upgrade-guide-your-move-to-async-await](https://futurestud.io/tutorials/hapi-v17-upgrade-guide-your-move-to-async-await)

看到有个 failAction 设置

想到我添加了个 transformRoute 的方法转化 router 写法，也可以添加一个转换的写法

#### 我写的 handler 可能是同步的，也可能是 async 的，这会造成差异吗？

search keyword: `await a non async function`

set out a test:

```javascript
// 最外边一层 async， 返回 111
;(async () => (() => '111')())().then(console.info)

// 对async 使用一个 await 返回 111
;(async () => await (async () => '111')())().then(console.info)

// 连续两个async 返回 111
;(async () => (async () => '111')())().then(console.info)

// 从外到内连续一个Promise和一个async 返回 111
;(async () => (() => Promise.resolve('111'))())().then(console.info)

// 对non-async function 使用await 返回 111
;(async () => await (() => '111')())().then(console.info)
```

都得到了 111，所以，对于是否 aysnc hanlder 无特殊情况

#### 识别 async Function

另外，针对未使用 babel 转译的 js，可以识别是否 AsyncFunction, 方法如下:

```javascript
const a = async () => 'a'
const b = () => 'b'

a[Symbol.toStringTag] === 'AsyncFunction'
;({}.toString.call(a) === '[object AsyncFunction]')

typeof a === 'function' && Object.toString.call(a).indexOf('async') === 0

// 多为无效
a.constructor instanceof AsyncFunction
```
