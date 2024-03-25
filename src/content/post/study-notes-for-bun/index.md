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
bun run index.js
bun run index.jsx
bun run index.ts
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
bun run --low-memory index.tsx
```
