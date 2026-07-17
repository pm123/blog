---
title: Claude Code 配置相关
tags: cloude
---

# Claude Code 配置相关

1.  setting 配置文件

   在用户自己的setting.json中配置env 不对外（项目）暴露

   使用CCSwitch管理env不对外暴露

   在工作目录里面创建 `.claude/setting.json` ，添加允许的操作和禁止的操作

```json
{
  // 允许 cloude code 执行的操作（不用每次确认）
  "permissions": {
    "allow": [
      "Read", // 读取文件
      "Write" // 写入文件
      "Bash(npm *)"  // 执行 npm 命令
      "Bash(git *)"  // 执行 git 命令
      "Bash(node *)"  // 执行 node 命令
    ],
    "deny": [
      "Bash(rm -rf *)"  // 禁止执行危险的删除命令
    ],
  },
  // 默认使用的模型
  "model": "sonnet",
  // 自动紧凑阈值（上下文使用超过此比例时自动压缩）
  "autoCompactThreshold": 80
}
```



2. CLAUDE.md： 你的项目“说明书”

   CLAUDE.md 是 Claude Code 中最重要的配置文件之一，——告诉AI这个项目的背景，技术栈，编码规范和当前进度。

   没有CLAUDE.md时，Claude Code 每次开始要作都要花时间“重新认识”你的项目，有了CLAUDE.md，它一启动就知道项目的全部背景，效率大幅提升。

   CLAUDE.md 模板

   ````markdown
   # 项目名称
   
   ## 项目概述
   一句话描述这个项目做什么。
   
   ## 技术栈
   
   
   ## 项目结构
   
   ``` bash
   + project
       + public  # 不参与编译的静态资源
       + src
           -------- 公用组件/指令/过滤器 ---------
           + components
           + filters
           + directives
           + plugins
               - http.js   # http 服务
               - ...
   
           -------- 公用数据模型 ----------
           models
               - user.js   # 定义一个User类
               -...
   
           -------- 数据中心，不同组件共享数据 --------
           + store
   
           -------- 全局样式管理 ------------
           + theme
               - index.less
               - var.less
   
           -------- 业务模块 ---------------
           + modules
               + moduleA
                   + components    # 模块内组件
                   + models        # 定义数据模型
                   - index.js   # 模块入口文件
                   - ...
               + moduleB
                   ...
       --------- 项目配置文件 -----------
       package.json
       .eslint.js
       .jmodule.js                # 项目配置文件
       .npmrc
       .gitignore
       ....
   ```
   
   ## 编码规范
   - 使用函数式组件
   - 组件文件使用 PascalCase 命名（如：BookmarkCard.tsx）
   - 工具函数使用 camelCase 命名
   - API 路由返回统一格式： {success: boolean, data?: any, error?: string}
   
   ## 当前开发状态
   
   ## 注意事项
   - 所有功能先创建 Git 分支再开发
   ````

   全局Claude.md (个人习惯和要求)

   ```markdown
   ## 沟通方式
   1. 默认中文回复；代码、命令、变量名、文件路径保持英文
   2. 结论先行，简洁直接，不先铺垫背景
   3. 不谄媚，不夸“这是个很好的问题”，不以"当然可以"开头
   4. 给真实判断一一方案有问题直接指出，发现更好做法主动说明
   
   ## Git
   1. 不自动git commit 或 git push， 除非我明确要求
   2. 提交前先展示将要提交的变更摘要
   3. commit message 使用简法英文
   
   ## 红线操作
   以下操作即使在 atuo-accept 模式下也必须先问我：
   1. 删除文件、目录或git历史
   2. 修改 .env 、密钥、token、证书、CI/CD配置
   3. git push, git rebase, git reset --hard 强制推送
   4. 公开发布（npm publish, 生产部署等）
   ```

   

3. 第二层记忆：Auto Memory (cloude code 自已的笔记本)

   /memory  命令

   单独起一下agent进程记忆

   1. 它只在当前项目生效（文件存在项目目录下），换项目需重新积累
   2. 启用后cc 不会每次都把所有记忆全部加载进上下文，只会读一份 memory.md 索引 ——遇到具体问题才去读对应的子文件，占token很少
   3. 随时可以用快捷键 ctrl + o 在会话中查看实际被调用过的记忆内容
   4. 记错了就跟它说“记掉刚刚说的不喜欢深色主题”，它会自己删掉

   > 一句话区分CLOUDE.md vs Auto Memory:  CLOUDE.md是第一优先级，全量注入的明规则；Auto Memory 是第二优先级，按需注入的隐规则，两者配合，cc 越用越懂你

4. 第三层记忆：自建参考文档（渐进式披露）

   避免大量内容都写在 CLAUDE.md 中，手动写一份专项参考文档

   例如在 CLAUDE.md 里加上指引：

   ```markdown
   ## 外部参考文档
   
   - 修改前端视觉、调颜色、调间距时 → 必读`docs/brand-visual.md` 
   - 写产品文案、按钮文字、提示语时 → 必读`docs/copywriting-style.md` 
   - 写API，定义返回格式时 → 必读`docs/api-convertions.md` 
   ```

   这样cc 只在“需要的时候”才去读完整文档，既保证 准确性，又不占多余上下文



> 本质认知：agent 的所的记忆，本质上都是在合适的时候向大模型注入压缩过的上下文。粗暴点说——这些记忆机制本质上还是提示词工程，只不过由cc帮你组织了层次

`.cloudeignore` 文件： 类似于`.gitignore` 文件，用来告诉 Cloude Code 跟些文件/目录不需要观注



https://github.com/multica-ai/andrej-karpathy-skills

