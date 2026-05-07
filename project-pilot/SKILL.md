---
name: project-pilot
description: 项目初始化技能，在创建新项目时使用脚手架而非手动编写，并主动推荐开发体验增强库。
version: 1.0.0
user-invocable: true
# disable-model-invocation: false
# allowed-tools: ["Bash", "Read", "Edit", "Write"]
# argument-hint: "可选的参数提示"
# model: sonnet
---

# Project Pilot Skill

## 概述

帮助用户快速初始化新项目。核心原则：**优先使用脚手架工具创建项目骨架，避免从零手动搭建；项目创建完成后，主动推荐适合的开发体验增强库。**

## 触发条件

当用户表达以下意图时触发此 skill：
- 创建新项目 / 初始化项目
- 搭建 XXX 项目
- 新建一个 XXX 应用
- 类似 "帮我建个项目" 的表述

## 指令

### 第一步：识别项目类型

向用户确认或推断以下信息：

1. **项目类型**：Web 前端、后端 API、全栈、CLI 工具、库/SDK、移动端等
2. **技术栈偏好**：
   - 前端：React / Vue / Svelte / Angular
   - 后端：Node.js (Express/Koa/Fastify/Nest) / Python (FastAPI/Django/Flask) / Go / Rust
   - 全栈：Next.js / Nuxt / Remix / SvelteKit
3. **包管理器**：npm / yarn / pnpm / bun
4. **语言**：TypeScript（默认推荐）/ JavaScript / Python / Go / Rust

### 第二步：使用脚手架创建项目

根据项目类型选择对应的脚手架命令，**禁止手动创建文件**。常用脚手架：

| 项目类型 | 脚手架命令 |
|---------|-----------|
| React (Vite) | `npm create vite@latest my-app -- --template react-ts` |
| Next.js | `npx create-next-app@latest my-app --typescript` |
| Vue (Vite) | `npm create vue@latest my-app` |
| SvelteKit | `npm create svelte@latest my-app` |
| Express API | `npx express-generator --typescript my-app` |
| NestJS | `npx @nestjs/cli new my-app` |
| FastAPI | `fastapi create my-app` |
| Go 项目 | `go mod init my-app` + 标准目录结构 |
| React Native | `npx react-native@latest init MyApp` |
| Electron | `npm init electron-app@latest my-app` |

执行脚手架命令后，进入项目目录并安装依赖。

### 第三步：推荐开发体验增强库

项目初始化完成后，根据技术栈**主动推荐**以下类别的库（每类推荐 1-2 个，说明用途）：

#### 代码质量
- **ESLint** — 代码检查（脚手架通常已内置）
- **Prettier** — 代码格式化
- **Biome** — ESLint + Prettier 替代方案，更快
- **oxlint** — 高性能 linter

#### 类型安全
- **TypeScript** — 如果脚手架未默认启用
- **Zod** / **Valibot** — 运行时类型校验

#### 测试
- **Vitest** — 单元测试（Vite 生态首选）
- **Jest** — 单元测试（通用选择）
- **Playwright** — E2E 测试
- **Testing Library** — 组件测试

#### 开发工具
- **Husky** + **lint-staged** — Git hooks，提交时自动检查
- **commitlint** — 规范 commit message
- **dotenv-cli** — 环境变量管理
- **concurrently** — 并行运行多个命令

#### 构建与部署
- **Docker** — 容器化（提供 Dockerfile 模板）
- **GitHub Actions** — CI/CD（提供 workflow 模板）

#### 文档
- **Storybook** — 组件文档（前端项目）
- **Swagger/OpenAPI** — API 文档（后端项目）

### 第四步：询问用户并安装

将推荐的库以列表形式展示，让用户选择需要安装的项，然后执行安装命令。同时询问是否需要：

- 初始化 Git 仓库（如尚未初始化）
- 配置 `.editorconfig`
- 配置 `.gitignore` 补充条目
- 添加 Docker 支持
- 添加 CI/CD 配置

### 第五步：输出项目结构概览

完成所有配置后，输出：
1. 项目目录结构树
2. 已安装的依赖清单
3. 可用的 npm scripts 命令列表
4. 快速开始指引（如何运行项目）

## 注意事项

- **始终使用脚手架**，不要手动编写 `package.json`、`tsconfig.json` 等配置文件
- 推荐库时说明选择理由，但不要强制安装
- 脚手架命令优先使用最新版本
- 如果用户指定的技术栈不常见，先搜索确认最佳实践再操作
- 创建完成后检查是否成功运行，确保项目可正常启动
