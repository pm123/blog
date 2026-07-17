---
title: pnpm workspace + Nx 实现 Monorepo
tags: Monorepo
---

这个项目的 Monorepo 实现基于 **pnpm workspace + Nx**，以下是具体分析：

------

## 一、包管理层：pnpm workspace

[`pnpm-workspace.yaml`](vscode-webview://16uju04ejg1agt9l1iqtk9ejrcmno8k3as59t86ck4tqi0rgjenk/index.html?id=bc3b0316-9ad8-454f-b234-95f2bd52c79c&parentId=1&origin=550eadb9-7982-4406-a0a7-8377756dfc42&swVersion=4&extensionId=JoyCoder.joycoder-fe&platform=electron&vscode-resource-base-authority=vscode-resource.vscode-cdn.net&parentOrigin=vscode-file%3A%2F%2Fvscode-app&purpose=webviewView#) 只有一条配置：

```yaml
packages:
  - 'packages/*'
```



`packages/` 下每个目录都是一个独立子包，每个子包有自己的 `package.json`，名称遵循 `@jindowin/<name>` 的 scope 规范，例如：

- `@jindowin/jdos3`
- `@jindowin/common`
- `@jindowin/api-jdos`

**包间依赖通过 `workspace:\*` 声明**，例如 `jdos3/package.json` 中：

```json
"@jindowin/common": "workspace:*",
"@jindowin/api-jdos": "workspace:*"
```



pnpm 会将这些依赖直接软链到本地包，无需发布即可相互引用，实现本地联调。

------

## 二、任务调度层：Nx

[`nx.json`](vscode-webview://16uju04ejg1agt9l1iqtk9ejrcmno8k3as59t86ck4tqi0rgjenk/index.html?id=bc3b0316-9ad8-454f-b234-95f2bd52c79c&parentId=1&origin=550eadb9-7982-4406-a0a7-8377756dfc42&swVersion=4&extensionId=JoyCoder.joycoder-fe&platform=electron&vscode-resource-base-authority=vscode-resource.vscode-cdn.net&parentOrigin=vscode-file%3A%2F%2Fvscode-app&purpose=webviewView#) 负责任务的并行执行与缓存：

**并行构建**：`"parallel": 6`，同时运行 6 个任务。

**构建缓存**：`cacheableOperations: ["build", "dev"]`，未变更的包直接复用上次产物，不重新编译。

**依赖顺序**：`dependsOn: [{projects: "dependencies", target: "build"}]`，构建前自动先构建所依赖的子包（如先 build `common`，再 build `jdos3`）。

根目录 [`package.json`](vscode-webview://16uju04ejg1agt9l1iqtk9ejrcmno8k3as59t86ck4tqi0rgjenk/index.html?id=bc3b0316-9ad8-454f-b234-95f2bd52c79c&parentId=1&origin=550eadb9-7982-4406-a0a7-8377756dfc42&swVersion=4&extensionId=JoyCoder.joycoder-fe&platform=electron&vscode-resource-base-authority=vscode-resource.vscode-cdn.net&parentOrigin=vscode-file%3A%2F%2Fvscode-app&purpose=webviewView#) 中的命令如：

```json
"jdos:dev": "nx run-many --target=dev --projects=@jindowin/jdos3*"
```



一条命令并行启动所有匹配 `jdos3*` 的子包的开发服务。

------

## 三、整体结构关系

```javascript
根目录（jindowin）
  ├── packages/common        ← 公共组件库，被其他包引用
  ├── packages/api-jdos      ← 自动生成的 API 层（OpenAPI）
  ├── packages/jdos3         ← 主业务包，依赖 common + api-*
  ├── packages/jdos3-cd      ← CD 子模块
  ├── packages/jdos3-platform
  └── ...
```



pnpm workspace 管理依赖安装与本地链接，Nx 负责任务编排与缓存加速，两者分工明确，共同构成完整的 Monorepo 方案。
