---
title: 面试准备清单
tags: 面试
---

# 面试准备清单

根据简历中的专业技能，整理了每个技术点的核心要点和常见面试题，供复习备战使用。

---

## 一、JavaScript 面向对象编程与 ES6+

### 技术要点

- 原型链、构造函数、`class` 语法糖与继承（`extends`/`super`）
- `this` 指向规则：普通函数、箭头函数、`call`/`apply`/`bind`
- 闭包的形成与内存泄漏场景
- Promise 状态机、链式调用、`Promise.all`/`allSettled`/`race`/`any` 的区别
- `async/await` 的本质（Generator + Promise 的语法糖）、错误处理（try/catch vs `.catch`）
- 事件循环：宏任务/微任务执行顺序，浏览器 vs Node.js 的差异
- 变量提升、暂时性死区（TDZ）、`let`/`const`/`var` 的作用域差异

### 面试题

1. 说说原型链的原理，`class` 底层是如何实现继承的？
2. 手写一个 `Promise.all` 的简化实现。
3. `async/await` 中如果一个 `await` 抛出异常但没有 `try/catch` 包裹，会发生什么？
4. 请解释一段包含 `setTimeout`、`Promise.then`、同步代码的执行顺序（现场给一段代码分析输出顺序）。
5. 闭包在什么场景下可能造成内存泄漏？如何避免？
6. 箭头函数和普通函数在 `this` 绑定上有什么本质区别？箭头函数能否作为构造函数？

---

## 二、Vue 2/3 框架与响应式原理

### 技术要点

- Vue 2：`Object.defineProperty` 实现响应式的原理与局限（无法监听新增属性、数组索引变化）
- Vue 3：`Proxy` + `Reflect` 实现响应式，`reactive`/`ref` 的区别与实现原理
- 依赖收集与派发更新：`Dep`/`Watcher`（Vue2）、`effect`/`track`/`trigger`（Vue3）
- Composition API vs Options API 的设计动机（逻辑复用、TS 支持）
- `nextTick` 原理：微任务队列与异步更新机制
- `keep-alive` 的缓存原理与生命周期钩子（`activated`/`deactivated`）
- 虚拟 DOM 与 Diff 算法：双端比较（Vue2）vs 最长递增子序列优化（Vue3）
- TSX 在 Vue 中的使用场景与优势

### 面试题

1. Vue 3 的响应式系统相比 Vue 2 做了哪些改进？为什么要用 Proxy？
2. `ref` 和 `reactive` 的区别是什么？为什么 `ref` 需要 `.value`？
3. 说说 `nextTick` 的实现原理，为什么修改数据后 DOM 不会立刻更新？
4. Vue 3 的 Diff 算法相比 Vue 2 做了哪些优化？（说说最长递增子序列的作用）
5. Composition API 解决了 Options API 的哪些痛点？
6. `keep-alive` 是如何实现组件缓存的？`activated`/`deactivated` 的触发时机？
7. 手写一个简化版的响应式实现（`reactive` + `effect`）。

---

## 三、TypeScript

### 技术要点

- 类型系统基础：接口 vs 类型别名（`interface` vs `type`）的区别与选择
- 泛型的使用场景：函数泛型、泛型约束（`extends`）、条件类型
- 高级类型：联合类型、交叉类型、映射类型、`Partial`/`Pick`/`Omit`/`Record` 等工具类型
- 类型推导与类型收窄（`typeof`/`instanceof`/自定义类型守卫）
- JS → TS 渐进式重构策略：`any` 的合理使用、`strict` 模式分阶段开启
- 前端 model 层重构：如何用 TS 保证接口数据类型安全（结合 GraphQL codegen）

### 面试题

1. `interface` 和 `type` 有什么区别？什么场景下必须用其中一个？
2. 说说 TS 中的类型收窄，手写一个自定义类型守卫函数。
3. 泛型的作用是什么？举一个业务中用泛型解决实际问题的例子。
4. 在一个存量 JS 项目中做渐进式 TS 重构，你会怎么规划迁移步骤？
5. `Partial<T>`、`Pick<T, K>`、`Omit<T, K>` 分别是怎么实现的？能否手写？
6. 结合 GraphQL codegen，说说你是如何做到前端数据链路类型安全的？

---

## 四、GraphQL 与接口设计

### 技术要点

- GraphQL vs REST：按需取数据、单一端点、强类型 Schema 的优劣势
- Query / Mutation / Subscription 的使用场景
- `graphql-request` + `codegen` 自动生成类型的工作流程
- N+1 查询问题与 DataLoader（后端概念，但前端也需理解影响）
- 前端 model 层设计：如何统一封装请求、减少字段冗余请求
- 缓存策略：客户端缓存（如 Apollo Client 的 normalized cache，若涉及）

### 面试题

1. GraphQL 相比 REST 解决了前端的哪些痛点？又带来了哪些新问题？
2. `codegen` 自动生成类型是怎么工作的？给项目带来了什么收益？
3. 如果一个页面需要聚合多个来源的数据，GraphQL 是如何简化这个过程的？
4. 说说你在项目中是如何通过重构 model 层减少 HTTP 请求和数据拼接问题的？

---

## 五、工程化：Git / pnpm Monorepo + Nx

### 技术要点

- Git 工作流：`rebase` vs `merge`、`cherry-pick`、`reflog` 找回丢失提交
- pnpm 的硬链接机制与 `node_modules` 结构优化，相比 npm/yarn 的优势
- Monorepo 的意义：代码复用、统一版本管理、原子化提交
- Nx 的任务编排、依赖图分析、增量构建（affected 命令）与缓存机制
- 多子包协同开发时的版本管理与发布策略（如 changesets）

### 面试题

1. pnpm 是如何通过硬链接节省磁盘空间、解决幽灵依赖问题的？
2. Monorepo 相比 Multi-repo 的优劣势分别是什么？你们是怎么权衡的？
3. Nx 的 `affected` 命令原理是什么？如何做到只构建/测试受影响的包？
4. 说一次你用 `git rebase` 解决冲突或整理提交历史的实际经历。
5. 多包协同开发时，如何管理包之间的版本依赖，避免版本地狱？

---

## 六、UI 组件库与样式方案

### 技术要点

- Element UI / Naive UI 的主题定制机制（CSS 变量 / SCSS 变量覆盖）
- Tailwind CSS 的原子化 CSS 思想、`@apply`、按需生成（PurgeCSS/JIT）
- 组件库二次封装的设计原则：Props 透传、插槽复用、可配置化
- 响应式布局方案：Flex/Grid、媒体查询、移动端适配（rem/vw）

### 面试题

1. Tailwind CSS 的原子化 CSS 思想解决了什么问题？相比传统 CSS 方案的取舍是什么？
2. 你是如何对 Element UI/Naive UI 组件进行二次封装的？如何保证 Props 的透传和扩展性？
3. 说说你做过的一个组件库主题定制方案，是如何实现动态换肤的？

---

## 七、数据可视化：ECharts / Chart.js

### 技术要点

- ECharts 的按需引入与体积优化
- 大数据量图表的渲染性能优化（数据抽样、Canvas vs SVG 渲染模式）
- 实时监控图表的数据更新策略（轮询 vs WebSocket 增量更新）
- 图表交互设计：tooltip、缩放、联动

### 面试题

1. 处理大数据量（比如几万个点）的图表渲染时，你做了哪些性能优化？
2. 实时监控图表的数据是如何更新的？如何避免频繁重绘导致的性能问题？
3. ECharts 按需引入是怎么做的？对打包体积有多大影响？

---

## 八、AI 能力工程化：SSE / Tool Calls / Sub-Agents

### 技术要点

- SSE（Server-Sent Events）与 WebSocket 的区别、适用场景
- 流式对话的前端实现：`EventSource` 或 `fetch` + `ReadableStream` 解析分片数据
- Tool Calls 的前端展示逻辑：如何渲染模型发起的工具调用过程与结果
- Sub-Agents 多智能体协作场景下的 UI 状态管理（多轮嵌套任务的展示）
- 流式内容的 Markdown 实时渲染与增量更新性能优化

### 面试题

1. SSE 和 WebSocket 各自的适用场景是什么？为什么流式对话选择用 SSE？
2. 用 `fetch` + `ReadableStream` 实现流式数据解析时会遇到哪些坑？（比如分片粘包、编码问题）
3. 如何设计 Tool Calls 的前端展示组件，让用户清楚看到 AI 当前在执行什么操作？
4. 流式输出的文本要实时渲染成 Markdown，如何避免频繁 diff 渲染带来的性能问题？

---

## 九、前端性能优化

### 技术要点

- 防抖（debounce）与节流（throttle）的实现原理与应用场景区别
- 路由懒加载：动态 `import()`、webpack 分包（splitChunks）
- 大 Diff/大列表场景的渲染优化：虚拟滚动、分片渲染（requestIdleCallback/时间切片）
- 首屏加载优化：资源预加载（preload/prefetch）、CDN、图片懒加载
- 长列表 diff 算法优化（如你简历中提到的关键字与词级差异高亮）

### 面试题

1. 手写一个 `debounce` 和 `throttle`，并说说各自的应用场景。
2. 路由懒加载的原理是什么？结合 Webpack/Vite 说说分包策略。
3. 遇到大 Diff（比如几千行代码的对比）导致页面卡死，你是怎么优化的？
4. 说说虚拟滚动（Virtual List）的实现原理，什么场景下需要用它？
5. 首屏加载慢，你会从哪些维度排查和优化？

---

## 综合应用类问题（结合简历项目经验）

1. 介绍一个你主导设计的组件抽象案例，说说抽象前后的收益对比（如 Diff 组件、Markdown 编辑器组件拆分）。
2. 在 Monorepo + Nx 架构下，团队协作和 CI 流程是如何设计的？
3. 你在项目中是如何做 JS → TS 渐进式重构的？遇到最大的挑战是什么？
4. 结合 AI Chat 模块的开发经验，说说前端在接入大模型能力时需要重点考虑哪些工程问题（流式渲染、错误重试、上下文管理等）。
5. 作为团队中较资深的前端，你是如何推动前端架构演进、说服团队采纳新方案的？

---

*建议：面试前挑选 2-3 个你最熟悉的项目，提前梳理成「背景-难点-方案-数据结果」的 STAR 结构，比单纯背技术点更容易在面试中出彩。*
