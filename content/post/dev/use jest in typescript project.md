---
title: 'Use Jest in Typescript Project'
date: 2021-03-27T19:23:05+08:00
tags: [jest]
categories: [dev]
---

### 安装依赖：

```bash
yarn add -D jest ts-jest
```

### 配置 ts

在测试文件加下，添加`tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "es2020",
    "module": "commonjs"
  },
  "extends": "jest"
}
```

### 配置 jest

添加 `js.config.js`

```

module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  testMatch: ['**/test/**/*.test.ts'],
}
```
