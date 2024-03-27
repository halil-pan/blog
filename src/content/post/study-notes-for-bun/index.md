---
title: "study notes for bun"
description: "Bun is an all-in-one JavaScript runtime & toolkit designed for speed, complete with a bundler, test runner, and Node.js-compatible package manager."
publishDate: "25 March 2024"
tags: ["bun"]
draft: true
---

## `bun run`

- `bun` CLI 可以用来执行 JavaScript/TypeScript 文件，`package.json` 脚本，还有可执行的包。
- Bun 使用了由 Apple Safari 开发的 JavaScript 引擎 JavaScriptCore，性能高于 V8。转译器运行时使用 Zig 编写。整体性能优于 Node.js。
- Bun 开箱支持 TypeScript 和 JSX，文件会在执行前由 Bun 的原生转译器转译。
- `--watch` 必须紧跟在 `bun` 之后，否则加在最后会被当成脚本标记 Script Flags。
- `npm run` 启动大致需要 170ms，Bun 是 6ms。

```bash
# run a file
bun run index.tsx

# naked command
bun index.tsx

# watch mode
bun --watch run index.tsx

# run package.json script
bun run dev

# override shebang, run with Bun instead of Node.js
bun run --bun vite

# pipe code from stdin
echo "console.log('hello')" | bun run

# reduce memory usage at the cost of performance
bun --smol run index.tsx
```

## 文件类型

- TypeScript: Bun 开箱支持 `.ts` 文件，Bun 在执行前通过原生转译器转译。Bun 不会进行类型检查，只是移除文件中的类型注解。
- JSX: Bun 开箱支持 `.jsx` `.tsx` 文件，Bun 在执行前通过使用内置的转译器将 JSX 语法转换为原生 JavaScript

```bash
bun index.js
bun index.jsx
bun index.ts
bun index.tsx
```

- TXT: 在 JavaScript 中引入时被加载为 string
- JSON & TOML: 也可以直接在 JavaScript 文件中引入，会被加载为 JavaScript Object
- SQLite: 引入 SQLite 数据库文件，会被加载为一个 `Database` 对象

```ts title="index.ts"
import text from "./text.txt";
import pkg from "./package.json";
import data from "./data.toml";
import db from "./my.db" with {type: "sqlite"};

console.log(db.query("select * from users LIMIT 1").get());
```

## TypeScript

Bun 无需额外配置就可以直接执行 `.ts` `.tsx` 文件，执行前转译的额外开销微不足道，可以将 TypeScript 代码直接用于生产环境。

如果 `tsconfig.json` 有配置 `compilerOptions` 的 `baseUrl` 或 `paths`，Bun 运行时的模块解析会基于这些配置完成路径映射。
