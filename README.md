# tRPC

存储tRPC的demo，用于配合某些next.js项目。tRPC是适用于全栈TypeScript开发人员轻松构建API的库。
在使用tRPC之前需要知道几个重要概念。

## Router

要开始构建基于 tRPC 的 API，首先需要定义路由器。
```js
import { initTRPC } from '@trpc/server';
import { createTRPCProxyClient } from '@trpc/client';

const t = initTRPC.create();
const router = t.router;
const appRouter = router({
// 服务端定义好greeting路由
  greeting: publicProcedure.query(() => 'hello tRPC v10!'),
});
// 实例化客户端
trpc = createTRPCProxyClient({})
// 从客户端调用greeting路由的query请求
const res = await trpc.greeting.query();
```

## Procedure

Procedure表示API终端的意思，它是TypeScript服务端初始化后的子对象，也是整个tRPC的核心部分。

```js
import { initTRPC } from '@trpc/server';
const t = initTRPC.create();
const publicProcedure = t.procedure;
```
通过多种方法实现了API的增删改查功能：

```ts
publicProcedure.query()
publicProcedure.input()
publicProcedure.mutation()
```

## 验证器
本案例中使用zod库作为ts的数据类型的推断器，常用于Procedure的输入输出的方法的参数上，基础使用方式如下：
```js
import { z } from "zod";

// 原始值
z.string();
z.number();
z.bigint();
z.boolean();
z.date();

const Dog = z.object({
  name: z.string(),
  age: z.number(),
});
```

```js
const appRouter = router({
// 服务端定义好greeting路由
  greeting: publicProcedure.input(
      z.object({
        name: z.string(),
      }),
    )
    .output(
      z.object({
        greeting: z.string(),
      }),
    ).query(() => 'hello tRPC v10!'),
});
```

## useQuery
`useQuery`来自`@tanstack/react-query`包，我们可以快速调用路由
```js
  const helloWithArgs = trpc.greeting.useQuery({ name: 'Jay' });
```

## useContext

通常我们采用`useContext`对全局数据进行更新。

```js
const utils = api.useContext();

utils.agent.getAll.setData(voidFunc(), (oldData) => [
        data,
        ...(oldData ?? []),
]);
```