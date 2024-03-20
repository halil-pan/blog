---
title: "try using hono"
description: "a small, simple, and ultrafast web framework for the Edges"
publishDate: "19 March 2024"
tags: ["nodejs", "hono"]
draft: true
---

## API

### App

Hono是一个用于构建Web应用的JavaScript框架，特别适合用在服务工作器（如Cloudflare Workers）和支持Service Worker API的环境中。它提供了一种简单高效的方式来处理HTTP请求和响应。让我们用简单的语言来解释Hono的主要特性和如何使用它。

#### 开始使用Hono

首先，你需要导入Hono，并创建一个新的应用实例：

```typescript
import { Hono } from 'hono'

const app = new Hono()
```

#### 处理请求

Hono允许你定义如何响应不同类型的HTTP请求，比如GET、POST等。你可以为特定的路径定义处理函数（也称为handler）或中间件。

```typescript
// 响应GET请求
app.get('/hello', (c) => c.text('Hello, Hono!'))

// 使用中间件
app.use((c, next) => {
  console.log('Middleware in action');
  return next();
})
```

#### 路由和路径

你可以为不同的路径和HTTP方法定义处理程序。Hono还支持使用`.all()`来响应所有类型的HTTP请求，以及`.on()`来同时处理多种HTTP方法。

#### 错误处理和404响应

使用`.notFound()`来自定义404响应，以及`.onError()`来处理应用中发生的错误。

```typescript
// 自定义404消息
app.notFound((c) => c.text('Custom 404 Message', 404))

// 错误处理
app.onError((err, c) => {
  console.error(`${err}`)
  return c.text('Custom Error Message', 500)
})
```

#### 进阶特性

- **自动事件监听**：`app.fire()`自动为你的应用添加全局fetch事件监听器，适合Service Worker API环境。
- **请求处理**：`app.fetch()`是应用的入口点，用于处理请求。适用于Cloudflare Workers等环境。
- **测试支持**：`app.request()`方法允许你发送GET请求或使用Request对象进行测试。
- **应用挂载**：`app.mount()`允许你将其他框架构建的应用挂载到Hono应用中。

#### 可配置选项

- **严格模式**：默认情况下，Hono区分路径末尾是否有斜杠。你可以通过设置`strict: false`来改变这种行为。
- **路由选项**：Hono允许你选择使用的路由器。默认是`SmartRouter`，但你也可以选择`RegExpRouter`。

#### 使用泛型

你可以使用泛型来指定Cloudflare Workers绑定和中间件中使用的变量类型，这有助于提高代码的类型安全性。

```typescript
type Bindings = {
  TOKEN: string
}

type Variables = {
  user: User
}

const app = new Hono<{ Bindings: Bindings; Variables: Variables }>()
```

#### 总结

Hono提供了一个简单而强大的方式来构建Web应用，特别是在服务工作器环境中。通过定义路由、处理请求、自定义错误响应，以及集成测试，Hono使得Web开发变得更加高效和有趣。

### Routing

当我们谈论Web开发时，路由是一个非常重要的概念。路由就是根据用户的请求URL，将其导向正确的处理程序。在这里，我将用简单的语言解释上述文档中的路由概念和示例。

#### 基本路由

假设你有一个网站，你希望根据用户执行的操作（如查看页面、提交表单等）来做出不同的响应。这时，你可以为不同的HTTP方法设置路由处理程序。例如：

- 当用户访问网站首页时，你可以显示一个欢迎信息。
- 当用户提交一个表单时，你可以处理这个表单。

使用Hono框架，你可以很容易地为不同的HTTP方法（GET, POST, PUT, DELETE）设置路由，就像这样：

```typescript
app.get("/", (c) => c.text("GET /"));
app.post("/", (c) => c.text("POST /"));
app.put("/", (c) => c.text("PUT /"));
app.delete("/", (c) => c.text("DELETE /"));
```

#### 通配符和自定义HTTP方法

- **通配符**：你可以使用`*`作为URL的一部分，来匹配多个路由。例如，`/wild/*/card`可以匹配`/wild/anything/card`。
- **自定义HTTP方法**：除了标准的HTTP方法外，你还可以定义自己的方法。比如，`PURGE`方法用来清除缓存。

#### 路径参数和正则表达式

- **路径参数**：允许你在URL中捕获一部分作为参数，例如`/user/:name`可以匹配`/user/john`，并且`john`会被作为参数。
- **正则表达式**：可以使用正则表达式来更精确地匹配URL路径，例如`/post/:date{[0-9]+}/:title{[a-z]+}`。

#### 链式路由和分组路由

- **链式路由**：允许你在同一个路径上为不同的HTTP方法添加多个处理程序。
- **分组路由**：如果你有一组相关的路由，你可以将它们组合在一起，然后将这个组添加到主应用程序中。这有助于组织和管理你的路由。

#### 基础路径和主机名路由

- **基础路径**：你可以为一组路由指定一个基础路径，这样所有的路由都会以这个路径为前缀。
- **主机名路由**：Hono允许你根据请求的主机名来设置路由，这样你可以为不同的域名提供不同的响应。

#### 路由优先级和分组排序

- **路由优先级**：路由处理程序按照它们注册的顺序执行。如果一个处理程序匹配并处理了请求，后续的处理程序将不会执行。
- **分组排序**：当你使用分组路由时，确保你正确地将它们添加到应用程序中，否则可能会导致404错误。

通过上述解释，你应该对Hono框架中的路由有了更清晰的理解。它提供了灵活且直观的路由设置方式，让你能够根据不同的请求URL和HTTP方法来处理用户的请求。

### Context

在Web开发中，处理HTTP请求和响应是核心任务之一。Hono框架提供了一个强大的工具来简化这个过程，称为"Context"对象。这个对象让你能够轻松访问请求信息、设置响应头、状态码，以及返回不同类型的响应。让我们用简单的语言来解释这些概念。

#### 请求信息

想象一下，你的服务器需要根据用户的浏览器类型来做出不同的响应。使用Context对象，你可以通过`c.req.header('User-Agent')`轻松获取用户的浏览器信息。

#### 设置响应

当你想要回应用户请求时，你可能需要设置响应头、状态码，然后返回响应体。Hono让这一切变得简单：

- 使用`c.header()`来设置响应头，例如`c.header('X-Message', 'Hello!')`。
- 使用`c.status()`来设置HTTP状态码，例如`c.status(201)`表示创建成功。
- 使用`c.body()`来设置响应体，例如`c.body('Thank you for coming')`。

Hono还提供了一些快捷方法来返回特定类型的响应：

- `c.text('Hello!')`：返回纯文本响应。
- `c.json({ message: 'Hello!' })`：返回JSON格式的响应。
- `c.html('<h1>Hello! Hono!</h1>')`：返回HTML格式的响应。

#### 重定向和404响应

如果你需要重定向用户到另一个URL，可以使用`c.redirect('/')`。默认情况下，这会使用302状态码，但你也可以指定其他状态码，如`c.redirect('/', 301)`。

如果你想返回一个“未找到”（404）响应，可以使用`c.notFound()`。

#### 中间件和变量

Hono允许你使用中间件来处理请求。你可以在中间件中设置一些变量，然后在后续的处理程序中使用这些变量。例如，你可以在一个中间件中设置一个消息，然后在处理程序中显示这个消息：

```typescript
app.use(async (c, next) => {
 c.set("message", "Hono is cool!!");
 await next();
});

app.get("/", (c) => {
 const message = c.get("message");
 return c.text(`The message is "${message}"`);
});
```

#### 响应布局和自定义渲染

你可以使用`c.setRenderer()`来定义一个全局的响应布局，然后在处理程序中使用`c.render()`来填充这个布局的内容。这对于创建具有一致布局的Web页面非常有用。

#### 访问环境变量和处理错误

在Cloudflare Workers等环境中，你可以通过`c.env`来访问绑定到工作器的环境变量、秘密或其他资源。

如果处理程序抛出一个错误，这个错误对象会被放置在`c.error`中。你可以在中间件中检查这个错误并做出相应的处理。

#### 总结

Hono的Context对象提供了一个简单而强大的接口，用于处理HTTP请求和响应。通过利用它的各种方法，你可以轻松地设置响应头、状态码、返回不同类型的响应，以及处理重定向和错误。这让Web开发变得更加高效和灵活。

### HonoRequest

在Hono框架中，`HonoRequest`是一个重要的对象，它封装了标准的`Request`对象，提供了一系列便捷的方法来处理HTTP请求。下面我们用更通俗易懂的方式来讲解`HonoRequest`的主要功能。

#### 获取路径参数（param）

当你定义路由时，可能会在URL路径中包含一些参数，`HonoRequest`允许你轻松获取这些参数的值。

```typescript
// 获取单个参数
app.get('/entry/:id', (c) => {
  const id = c.req.param('id')
  // 使用id做一些事情...
})

// 一次性获取所有参数
app.get('/entry/:id/comment/:commentId', (c) => {
  const { id, commentId } = c.req.param()
  // 使用id和commentId做一些事情...
})
```

#### 获取查询字符串（query）

你可以使用`query`方法来获取URL查询字符串中的参数。

```typescript
// 获取单个查询参数
app.get('/search', (c) => {
  const query = c.req.query('q')
  // 使用查询参数做一些事情...
})

// 一次性获取所有查询参数
app.get('/search', (c) => {
  const { q, limit, offset } = c.req.query()
  // 使用这些查询参数做一些事情...
})
```

#### 获取多个查询字符串（queries）

如果你的URL中有多个相同名称的查询参数，可以使用`queries`方法来获取它们的数组。

```typescript
app.get('/search', (c) => {
  const tags = c.req.queries('tags') // tags将是一个字符串数组
  // 使用tags做一些事情...
})
```

#### 获取请求头（header）

通过`header`方法可以获取请求中的特定头信息。

```typescript
app.get('/', (c) => {
  const userAgent = c.req.header('User-Agent')
  // 使用userAgent做一些事情...
})
```

#### 解析请求体（parseBody）

`parseBody`方法支持解析`multipart/form-data`或`application/x-www-form-urlencoded`类型的请求体。

```typescript
app.post('/entry', async (c) => {
  const body = await c.req.parseBody()
  // 使用请求体中的数据做一些事情...
})
```

这个方法支持单文件和多文件的解析。

#### 解析JSON请求体（json）

如果请求体是`application/json`类型，可以使用`json`方法来解析。

```typescript
app.post('/entry', async (c) => {
  const body = await c.req.json()
  // 使用解析后的JSON对象做一些事情...
})
```

#### 解析文本请求体（text）

对于`text/plain`类型的请求体，可以使用`text`方法来解析。

```typescript
app.post('/entry', async (c) => {
  const body = await c.req.text()
  // 使用解析后的文本做一些事情...
})
```

#### 其他有用的方法

- `arrayBuffer()`：将请求体解析为`ArrayBuffer`。
- `valid()`：获取经过验证的数据。
- `routePath()`：获取处理程序中注册的路径。
- `matchedRoutes()`：返回处理程序中匹配的路由，对于调试很有用。
- `path`：获取请求的路径名。
- `url`：获取请求的URL字符串。
- `method`：获取请求的方法名（如GET、POST）。
- `raw`：获取原始的`Request`对象，适用于需要访问特定平台特性的场景，如Cloudflare Workers。

通过这些方法，`HonoRequest`为处理HTTP请求提供了强大而灵活的功能。

### Exception

在构建Web应用时，处理异常是一个非常重要的部分，尤其是当遇到致命错误，比如认证失败时。在Hono框架中，我们可以通过抛出`HTTPException`来优雅地处理这类问题。下面我们用简单的语言来解释这个概念。

#### 抛出HTTP异常

当你的应用中发生了一个错误，比如用户认证失败，你可以通过抛出一个`HTTPException`来中断当前的请求处理流程，并返回一个错误响应给用户。

```typescript
import { HTTPException } from 'hono/http-exception'

app.post('/auth', async (c, next) => {
  // 进行认证...
  if (用户未认证) {
    throw new HTTPException(401, { message: '自定义错误信息' })
  }
  await next()
})
```

在这个例子中，如果用户未通过认证，将会抛出一个状态码为401的`HTTPException`，并附带一条自定义的错误信息。

#### 定制错误响应

除了简单的错误信息外，你还可以定制返回给用户的错误响应。比如，你可以设置响应的状态码、响应头和响应体。

```typescript
const errorResponse = new Response('未授权', {
  status: 401,
  headers: {
    Authenticate: 'error="invalid_token"',
  },
})
throw new HTTPException(401, { res: errorResponse })
```

这样，当异常被抛出时，用户将收到一个包含更详细信息的自定义响应。

#### 处理HTTP异常

当`HTTPException`被抛出时，你可以使用`app.onError`方法来统一处理这些异常。

```typescript
import { HTTPException } from 'hono/http-exception'

app.onError((err, c) => {
  if (err instanceof HTTPException) {
    // 获取自定义的响应
    return err.getResponse()
  }
  // 处理其他类型的错误...
})
```

这个方法允许你检查错误是否为`HTTPException`的实例，如果是，你可以获取并返回之前定义的自定义响应。

#### 添加错误原因

在某些情况下，你可能还想要附加一些额外的错误信息，比如错误的原因。`HTTPException`支持一个`cause`选项，让你可以添加这些信息。

```typescript
app.post('/auth', async (c, next) => {
  try {
    authorize(c)
  } catch (e) {
    throw new HTTPException(401, { message: '自定义消息', cause: e })
  }
  await next()
})
```

在这个例子中，如果认证函数`authorize`抛出了异常，`HTTPException`将会被抛出，并附带原始异常作为`cause`。

总的来说，`HTTPException`提供了一种强大而灵活的方式来处理Web应用中的错误和异常。通过定制错误响应和统一处理异常，可以让你的应用更加健壮和用户友好。

### Presets

在Hono框架中，为了适应不同的使用场景，提供了几种预设的路由器。这些预设旨在简化开发流程，让你不必每次都指定使用哪个路由器。虽然所有预设导入的`Hono`类都是相同的，但它们所使用的路由器有所不同。因此，你可以根据具体需求灵活选择使用。

下面是对这些预设的简单说明：

#### hono

**使用方式**:

```typescript
import { Hono } from 'hono'
```

**路由器配置**:

```typescript
this.router = new SmartRouter({
  routers: [new RegExpRouter(), new TrieRouter()],
})
```

#### hono/quick

**使用方式**:

```typescript
import { Hono } from 'hono/quick'
```

**路由器配置**:

```typescript
this.router = new SmartRouter({
  routers: [new LinearRouter(), new TrieRouter()],
})
```

#### hono/tiny

**使用方式**:

```typescript
import { Hono } from 'hono/tiny'
```

**路由器配置**:

```typescript
this.router = new PatternRouter()
```

#### 应该使用哪个预设？

- **hono**: 这个预设适用于大多数使用场景，尤其推荐给需要长时间运行的服务器，比如使用Deno、Bun或Node.js构建的服务器。对于Cloudflare Workers、Deno Deploy这样的环境也很适合，因为它们使用的v8隔离区在启动后会持续一段时间。虽然在注册阶段可能稍慢，但一旦启动，性能表现很好。

- **hono/quick**: 这个预设设计用于每个请求都会初始化应用程序的环境，比如Fastly Compute。如果你的应用运行在这样的环境中，推荐使用这个预设。

- **hono/tiny**: 这是最小的路由器包，适用于资源有限的环境。如果你需要在资源受限的环境下运行Hono，可以选择这个预设。

根据你的具体需求和运行环境，可以灵活选择合适的预设以获取最佳的性能和资源利用率。
