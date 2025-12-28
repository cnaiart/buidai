---
title: "编译部署流程"
description: "必定AI项目的编译部署流程说明"
icon: 'folder-tree'
---

# 🚀 BuildingAI 开发工作流程完全指南

> **💡 小白友好版本** - 从零开始,手把手教你修改代码后该怎么做

## 📚 阅读指南

**🎯 如果你是新手,请从头开始阅读**  
**⚡ 如果你赶时间,直接看 [快速理解](#-快速理解) 和 [常见场景](#-常见场景处理)**

---

## 📋 目录

1. [快速理解](#-快速理解) - 什么时候需要构建?
2. [开发环境](#-开发环境推荐日常使用) - 自动热重载,保存即生效
3. [生产环境](#-生产环境部署上线时使用) - 需要手动构建
4. [Docker 环境](#-docker-环境容器化部署) - 容器化部署
5. [常见场景处理](#-常见场景处理) - 6 个实际场景的完整步骤
6. [故障排查](#-故障排查) - 遇到问题怎么办
7. [快速参考](#-快速参考) - 一句话速查表

---

## 🎯 快速理解

### 什么时候需要构建?

| 环境 | 是否需要构建 | 什么时候需要 |
|------|------------|-------------|
| **🔧 开发环境** | ❌ **不需要** | 保存文件就自动生效 |
| **🚀 生产环境** | ✅ **需要** | 准备部署到服务器时 |
| **🐳 Docker 环境** | ✅ **需要** | 重新构建 Docker 镜像时 |

### 一句话总结

- **日常开发**: 用开发环境,改完保存就能看到效果,不需要任何额外操作 ✨
- **准备上线**: 用生产环境,需要先构建再重启服务 🚀
- **Docker 部署**: 需要重新构建 Docker 镜像 🐳

---

## 🔧 开发环境(推荐日常使用)

### ⭐ 核心特点

- ✅ **热模块替换(HMR)**: 修改代码后自动刷新,无需手动刷新浏览器
- ✅ **自动重启**: 后端代码修改后自动重启服务
- ✅ **即时反馈**: 保存文件后 1-3 秒内看到效果
- ✅ **开发友好**: 详细的错误提示和日志

---

### 🚀 第一步:启动开发服务

#### 方式一:同时启动前后端(推荐新手)

```bash
# 在项目根目录执行
pnpm dev
```

**会发生什么?**
1. ⚡ Turbo 自动启动所有服务
2. 🔥 前端服务启动在 `http://localhost:3000`
3. 🔌 后端 API 启动在 `http://localhost:4090`
4. 📝 终端显示详细的启动日志

**你需要做什么?**
- 等待启动完成(首次启动约 30-60 秒)
- 看到 "ready" 或 "✓" 提示后,打开浏览器访问 `http://localhost:3000`
- **不要关闭终端窗口**,让它一直运行

---

#### 方式二:只启动前端(适合只改前端)

```bash
# 在项目根目录执行
pnpm dev:web
```

**等同于:**
```bash
cd packages/web/buildingai-ui
pnpm dev
```

**什么时候用?**
- 只需要修改前端页面或组件
- 后端 API 已经在运行或使用远程 API
- 节省系统资源

---

#### 方式三:只启动后端(适合只改后端)

```bash
# 在项目根目录执行
pnpm dev:api
```

**等同于:**
```bash
cd packages/api
pnpm dev
```

**什么时候用?**
- 只需要修改后端逻辑
- 测试 API 接口
- 使用 Postman 等工具测试

---

### ✏️ 第二步:修改代码

#### 场景 1:修改前端 Vue 文件

**示例:修改 `packages/web/buildingai-ui/app/pages/index.vue`**

```vue
<!-- 修改前 -->
<template>
  <div>欢迎使用 BuildingAI</div>
</template>

<!-- 修改后 -->
<template>
  <div>欢迎使用 BuildingAI - 我的第一次修改!</div>
</template>
```

**完整流程:**
1. ✏️ 打开文件,修改内容
2. 💾 保存文件(Ctrl+S 或 Cmd+S)
3. ⏱️ 等待 1-3 秒
4. ✨ **浏览器自动刷新,立即看到效果!**

**你需要做什么?**
- **什么都不需要做!** 保存后自动生效
- 如果浏览器没自动刷新,手动刷新一下(F5)

---

#### 场景 2:修改前端组件

**示例:修改 `packages/web/buildingai-ui/app/pages/public/agent/components/chats-list.vue`**

**完整流程:**
1. ✏️ 修改组件代码(模板、脚本、样式都可以)
2. 💾 保存文件
3. ⏱️ 等待 1-3 秒
4. ✨ **浏览器自动刷新,组件立即更新!**

**与修改页面文件完全一样!**

---

#### 场景 3:修改样式文件

**示例:修改 `.css`、`.scss` 文件**

**完整流程:**
1. ✏️ 修改样式
2. 💾 保存文件
3. ⏱️ 等待 1-2 秒
4. ✨ **样式立即生效,甚至不需要刷新页面!**

**特别说明:**
- CSS 修改通常比其他文件更快,几乎是瞬间生效
- 这是 Vite HMR 的强大之处

---

#### 场景 4:修改后端文件

**示例:修改 `packages/api/src/modules/chat/chat.controller.ts`**

```typescript
// 修改前
@Get('list')
getList() {
  return { message: 'Chat list' };
}

// 修改后
@Get('list')
getList() {
  return { message: 'Chat list - 已更新' };
}
```

**完整流程:**
1. ✏️ 修改后端代码
2. 💾 保存文件
3. ⏱️ 等待 2-3 秒
4. 📝 终端显示 "[Nest] restarting..." 或类似信息
5. ✅ 终端显示 "ready" 或 "✓"
6. ✨ **后端服务已重启,API 已更新!**

**你需要做什么?**
- **什么都不需要做!** Nodemon 会自动重启后端服务
- 如果有前端页面调用这个 API,刷新一下页面就能看到新数据

---

#### 场景 5:修改环境变量(.env 文件)

**示例:修改 `.env` 文件**

```bash
# 修改前
DB_HOST=localhost

# 修改后
DB_HOST=127.0.0.1
```

**完整流程:**
1. ✏️ 修改 `.env` 文件
2. 💾 保存文件
3. ⛔ **注意:环境变量修改不会自动生效!**
4. 🛑 停止开发服务器(在终端按 `Ctrl+C`)
5. 🚀 重新启动开发服务器(`pnpm dev`)

**为什么要重启?**
- 环境变量在服务启动时加载,运行中不会重新读取

---

#### 场景 6:修改 package.json(添加/删除依赖)

**示例:添加新的 npm 包**

**完整流程:**
1. ✏️ 修改 `package.json` 或直接运行安装命令
2. 📦 重新安装依赖
   ```bash
   pnpm install
   ```
3. 🛑 停止开发服务器(`Ctrl+C`)
4. 🚀 重新启动开发服务器(`pnpm dev`)

**推荐方式:**
```bash
# 先停止服务(Ctrl+C)
# 安装新依赖
pnpm add <包名>
# 重新启动
pnpm dev
```

---

### 📊 开发环境总结表

| 修改的文件类型 | 需要手动操作 | 生效时间 | 自动处理机制 |
|--------------|------------|---------|-------------|
| `.vue` 文件 | ❌ 不需要 | 1-3秒 | Vite HMR 自动刷新 |
| 前端 `.ts/.js` 文件 | ❌ 不需要 | 1-3秒 | Vite HMR 自动刷新 |
| `.css/.scss` 样式 | ❌ 不需要 | 1-2秒 | Vite HMR 热更新(无需刷新) |
| 后端 `.ts` 文件 | ❌ 不需要 | 2-3秒 | Nodemon 自动重启 |
| `.env` 环境变量 | ✅ 需要重启服务 | - | 无自动机制 |
| `package.json` | ✅ 需要 `pnpm install` + 重启 | - | 无自动机制 |
| `packages/@buildingai/*` | ✅ 需要重启服务 | - | 可能需要重新构建 |

---

## 🚀 生产环境(部署上线时使用)

### ⭐ 核心特点

- 🏗️ **代码编译**: 将 TypeScript/Vue 编译为优化的 JavaScript
- 📦 **代码压缩**: 减小文件体积,提升加载速度
- ⚡ **性能优化**: Tree-shaking、代码分割等优化
- 🔒 **生产配置**: 使用生产环境配置和环境变量

### 🤔 什么时候需要构建?

- ✅ 准备部署到生产服务器
- ✅ 测试生产版本的性能
- ✅ 生成最终的部署文件
- ❌ 日常开发(用开发环境更快)

---

### 📝 构建前的准备

#### 1. 确保环境正确

```bash
# 检查 Node 版本(必须是 22.20.x)
node -v
# 应该显示: v22.20.0 或类似版本

# 检查 pnpm 版本
pnpm -v
# 应该显示: 10.20.0 或更高
```

#### 2. 确保依赖已安装

```bash
# 在项目根目录执行
pnpm install
```

#### 3. 确保环境变量配置正确

```bash
# 确保根目录有 .env 文件
# 如果没有,从示例文件复制
cp .env.example .env
# 然后编辑 .env 文件,填入正确的配置
```

---

### 🎯 场景 A:只修改了前端文件(最常用)

#### 适用情况

- ✅ 修改了 `.vue` 文件(如 `index.vue`、`chats-list.vue`)
- ✅ 修改了前端 `.ts/.js` 文件(组件逻辑、工具函数等)
- ✅ 修改了样式文件(`.css`、`.scss`)
- ✅ 修改了前端配置(`nuxt.config.ts`)
- ❌ **没有**修改后端代码
- ❌ **没有**修改 `packages/@buildingai/*` 下的共享包

#### 完整操作步骤

```bash
# 第 1 步:确保在项目根目录
cd e:/本地项目/BuildingAI-master

# 第 2 步:构建前端(只构建前端,速度快)
pnpm build:web
```

**会发生什么?**
1. 🔄 执行 `cd packages/web/buildingai-ui && pnpm run generate`
2. 🏗️ Nuxt 开始构建(使用 Vite 编译)
3. 📦 生成静态文件到 `packages/web/buildingai-ui/.output/public`
4. 🚀 运行 `scripts/release.mjs` 复制文件到 `public/web`
5. ✅ 构建完成

**预计耗时: 2-5 分钟**

**终端输出示例:**
```
✓ Building client... (5.2s)
✓ Generating pages... (3.1s)
✓ Client built in 8.3s
✓ Generated to .output/public
```

```bash
# 第 3 步:重启 PM2 服务
pnpm pm2:restart
```

**会发生什么?**
1. 🔄 PM2 重启 buildingai 进程
2. 🚀 加载新构建的前端文件
3. ✅ 服务更新完成

**预计耗时: 2-5 秒**

```bash
# 第 4 步:验证是否成功
pnpm pm2:status
```

**正常输出示例:**
```
┌─────┬────────────┬─────────┬─────────┬─────────┬──────────┐
│ id  │ name       │ status  │ restart │ uptime  │ memory   │
├─────┼────────────┼─────────┼─────────┼─────────┼──────────┤
│ 0   │ buildingai │ online  │ 1       │ 10s     │ 145 MB   │
└─────┴────────────┴─────────┴─────────┴─────────┴──────────┘
```

**检查要点:**
- ✅ status 显示 "online"
- ✅ restart 次数增加了 1
- ✅ uptime 显示刚刚重启(几秒或几分钟)

```bash
# 第 5 步:访问网站验证
# 打开浏览器,访问你的网站地址
# 刷新页面(Ctrl+F5 强制刷新)
# 检查你的修改是否生效
```

---

### 🎯 场景 B:只修改了后端文件

#### 适用情况

- ✅ 修改了后端 `.ts` 文件(`.controller.ts`、`.service.ts`、`.module.ts` 等)
- ✅ 修改了数据库实体(`.entity.ts`)
- ✅ 修改了后端配置
- ❌ **没有**修改前端代码
- ❌ **没有**修改 `packages/@buildingai/*` 下的共享包

#### 完整操作步骤

```bash
# 第 1 步:确保在项目根目录
cd e:/本地项目/BuildingAI-master

# 第 2 步:构建后端(只构建后端)
pnpm build:api
```

**会发生什么?**
1. 🔄 执行 `cd packages/api && pnpm run build`
2. 🏗️ NestJS CLI 开始编译(使用 TypeScript Compiler)
3. 📦 生成编译后的 JavaScript 文件到 `packages/api/dist`
4. ✅ 构建完成

**预计耗时: 2-5 分钟**

**终端输出示例:**
```
Info: Nest CLI v11.0.10
Info: Building packages...
Info: Successfully built packages
```

```bash
# 第 3 步:重启 PM2 服务
pnpm pm2:restart

# 第 4 步:验证服务状态
pnpm pm2:status

# 第 5 步:测试 API 接口
# 可以用 Postman 或浏览器测试你修改的接口
```

---

### 🎯 场景 C:前端和后端都修改了

#### 适用情况

- ✅ 同时修改了前端和后端代码
- ✅ 修改了前后端的配置
- ❌ **没有**修改 `packages/@buildingai/*` 下的共享包

#### 完整操作步骤

**方式一:分别构建(推荐,更清晰)**

```bash
# 第 1 步:确保在项目根目录
cd e:/本地项目/BuildingAI-master

# 第 2 步:构建前端
pnpm build:web
# ⏱️ 等待 2-5 分钟

# 第 3 步:构建后端
pnpm build:api
# ⏱️ 等待 2-5 分钟

# 第 4 步:重启服务
pnpm pm2:restart

# 第 5 步:验证
pnpm pm2:status
```

**总耗时: 4-10 分钟**

**方式二:一次性构建(如果时间充裕)**

```bash
# 第 1 步:确保在项目根目录
cd e:/本地项目/BuildingAI-master

# 第 2 步:完整构建(使用 Turbo)
pnpm build
```

**会发生什么?**
1. 🔄 Turbo 分析依赖关系
2. 🏗️ 按顺序构建所有包(共享包 → 前端 → 后端)
3. 📦 生成所有输出文件
4. ✅ 完整构建完成

**预计耗时: 5-10 分钟**

```bash
# 第 3 步:重启服务
pnpm pm2:restart

# 第 4 步:验证
pnpm pm2:status
```

---

### 🎯 场景 D:修改了 packages/@buildingai 下的共享包

#### 适用情况

- ✅ 修改了 `packages/@buildingai/ai-sdk`
- ✅ 修改了 `packages/@buildingai/db`
- ✅ 修改了 `packages/@buildingai/utils`
- ✅ 修改了任何 `packages/@buildingai/*` 下的包

#### 为什么必须用 `pnpm build`?

因为这些包是被前端和后端共同依赖的,Turbo 会:
1. 先构建被修改的共享包
2. 再构建依赖它的前端包
3. 最后构建依赖它的后端包

如果只用 `pnpm build:web` 或 `pnpm build:api`,共享包不会被重新构建,修改不会生效!

#### 完整操作步骤

```bash
# 第 1 步:确保在项目根目录
cd e:/本地项目/BuildingAI-master

# 第 2 步:完整构建(必须用这个命令)
pnpm build
```

**会发生什么?**
1. 🔄 Turbo 分析哪些包被修改了
2. 🏗️ 从被修改的包开始,按依赖顺序构建
3. 📦 生成所有相关包的输出文件
4. ✅ 完整构建完成

**预计耗时: 5-10 分钟**

**终端输出示例:**
```
• Packages in scope: @buildingai/ai-sdk, @buildingai/api, @buildingai/buildingai-ui, ...
• Running build in 15 packages
• Remote caching disabled

 @buildingai/ai-sdk:build: cache hit, replaying logs
 @buildingai/api:build: building...
 @buildingai/buildingai-ui:build: building...
```

```bash
# 第 3 步:重启服务
pnpm pm2:restart

# 第 4 步:验证
pnpm pm2:status
```

---

### 📊 生产环境命令对比表

| 修改内容 | 推荐命令 | 实际执行的操作 | 耗时 | 何时使用 |
|---------|---------|--------------|------|----------|
| 只改前端 | `pnpm build:web` | `cd packages/web/buildingai-ui && pnpm run generate` | 2-5分钟 | ✅ 最常用 |
| 只改后端 | `pnpm build:api` | `cd packages/api && pnpm run build` | 2-5分钟 | ✅ API 修改 |
| 前后端都改 | `pnpm build:web` + `pnpm build:api` | 分别执行上面两个 | 4-10分钟 | ✅ 同时修改 |
| 改了共享包 | `pnpm build` | Turbo 完整构建所有包 | 5-10分钟 | ✅ 修改依赖 |
| 不确定改了啥 | `pnpm build` | Turbo 完整构建所有包 | 5-10分钟 | ✅ 保险起见 |

---

### 🛠️ PM2 服务管理命令详解

#### 查看服务状态

```bash
pnpm pm2:status
```

**输出示例:**
```
┌─────┬────────────┬─────────┬─────────┬─────────┬──────────┐
│ id  │ name       │ status  │ restart │ uptime  │ memory   │
├─────┼────────────┼─────────┼─────────┼─────────┼──────────┤
│ 0   │ buildingai │ online  │ 0       │ 2h      │ 145 MB   │
└─────┴────────────┴─────────┴─────────┴─────────┴──────────┘
```

**状态说明:**
- `online` - 运行中(正常)
- `stopped` - 已停止
- `errored` - 错误状态
- `launching` - 启动中

#### 重启服务

```bash
pnpm pm2:restart
```

**等同于:**
```bash
pnpm restart
```

**什么时候用?**
- 构建完成后
- 修改了 .env 文件后
- 服务出现异常需要重启

#### 停止服务

```bash
pnpm stop
```

**等同于:**
```bash
pnpm pm2:stop
```

**什么时候用?**
- 需要完全停止服务
- 准备重新部署

#### 启动服务

```bash
pnpm start
```

**会发生什么?**
1. 🔄 运行 `pnpm i` 安装依赖
2. 🔄 运行 `pnpm sync-env` 同步环境变量
3. 🚀 启动 PM2 服务

#### 查看日志

```bash
# 查看实时日志
pnpm pm2:logs

# 或直接查看日志文件
cat logs/pm2/api-combined.log
```

#### 查看实时监控

```bash
pnpm pm2:monitor
```

**会显示:**
- CPU 使用率
- 内存使用情况
- 重启次数
- 实时日志

#### 清空日志

```bash
pnpm pm2:flush
```

**什么时候用?**
- 日志文件太大
- 需要清理旧日志

#### 重置服务(完全重新开始)

```bash
pnpm pm2:reset
```

**会发生什么?**
1. 🧹 删除 PM2 日志目录
2. 🛑 删除所有 PM2 进程
3. 🚀 重新启动服务

**什么时候用?**
- 遇到无法解决的 PM2 问题
- 想要完全重置 PM2 状态

---

## 🐳 Docker 环境(容器化部署)

### ⭐ 核心特点

- 📦 **容器化**: 所有服务运行在 Docker 容器中
- 🔒 **隔离性**: 不影响主机环境
- 🚀 **一键部署**: 包含所有依赖(Node.js、PostgreSQL、Redis)
- 🔄 **易于复制**: 相同环境可在任何机器上运行

---

### 🚀 首次启动

```bash
# 第 1 步:确保已安装 Docker 和 Docker Compose
docker --version
docker compose version

# 第 2 步:确保在项目根目录
cd e:/本地项目/BuildingAI-master

# 第 3 步:启动所有服务
pnpm docker:up
```

**等同于:**
```bash
docker compose up -d
```

**会发生什么?**
1. 🔄 读取 `docker-compose.yml` 配置
2. 🏗️ 构建 BuildingAI 镜像(首次需要 10-15 分钟)
3. 📦 拉取依赖镜像(PostgreSQL、Redis)
4. 🚀 启动所有容器
5. ✅ 所有服务运行在后台

**预计耗时:**
- 首次启动: 10-15 分钟
- 后续启动: 1-2 分钟

---

### 📝 修改代码后的完整流程

#### 第 1 步:停止容器

```bash
pnpm docker:down
```

**等同于:**
```bash
docker compose down
```

**会发生什么?**
1. 🛑 停止所有容器
2. 🧹 删除所有容器
3. ✅ 网络和卷保留(数据不丢失)

#### 第 2 步:修改代码

- ✏️ 修改你需要修改的文件
- 💾 保存所有修改

#### 第 3 步:重新构建并启动

```bash
pnpm docker:up
```

**会发生什么?**
1. 🔄 Docker 检测到代码变化
2. 🏗️ 重新构建 BuildingAI 镜像
3. 📦 创建新的容器
4. 🚀 启动所有服务
5. ✅ 新代码已部署

**预计耗时: 5-10 分钟**

#### 第 4 步:验证服务状态

```bash
# 查看容器状态
docker compose ps
```

**正常输出示例:**
```
NAME                     IMAGE                STATUS         PORTS
buildingai-app           buildingai:latest    Up 2 minutes   0.0.0.0:3000->3000/tcp, 0.0.0.0:4090->4090/tcp
buildingai-postgres      postgres:17          Up 2 minutes   0.0.0.0:5432->5432/tcp
buildingai-redis         redis:7              Up 2 minutes   0.0.0.0:6379->6379/tcp
```

**检查要点:**
- ✅ STATUS 都显示 "Up"
- ✅ PORTS 端口映射正确

```bash
# 查看容器日志
docker compose logs -f
```

**正常日志示例:**
```
buildingai-app  | [Nest] 1  - 12/28/2025, 10:30:45 AM     LOG [NestApplication] Nest application successfully started
buildingai-app  | [Nuxt] Listening on http://localhost:3000
```

#### 第 5 步:访问应用

```bash
# 打开浏览器访问
http://localhost:3000

# 测试 API
http://localhost:4090/api/health
```

---

### 🛠️ Docker 常用命令

#### 查看运行中的容器

```bash
docker compose ps
```

#### 查看所有容器日志

```bash
# 实时查看所有日志
docker compose logs -f

# 只查看 buildingai 应用日志
docker compose logs -f buildingai-app

# 只查看最后 100 行
docker compose logs --tail=100
```

#### 进入容器内部

```bash
# 进入 buildingai 容器
docker compose exec buildingai-app sh

# 进入 PostgreSQL 容器
docker compose exec postgres psql -U buildingai

# 进入 Redis 容器
docker compose exec redis redis-cli
```

#### 重启特定容器

```bash
# 只重启 buildingai 应用
docker compose restart buildingai-app

# 重启所有容器
docker compose restart
```

#### 停止容器(保留容器)

```bash
docker compose stop
```

#### 启动已停止的容器

```bash
docker compose start
```

#### 完全清理(包括数据卷)

```bash
# ⚠️ 警告:这会删除所有数据!
docker compose down -v
```

---

### 💡 Docker 环境说明

#### 优点

- ✅ 环境一致性: 开发、测试、生产完全相同
- ✅ 快速部署: 一键启动所有服务
- ✅ 易于管理: 统一的容器管理
- ✅ 资源隔离: 不污染主机环境

#### 缺点

- ❌ 启动较慢: 每次修改都需要重新构建镜像
- ❌ 调试不便: 需要进入容器查看日志
- ❌ 资源占用: Docker 会占用额外的系统资源

#### 什么时候用 Docker?

- ✅ 生产环境部署
- ✅ 测试完整部署流程
- ✅ 多人协作,需要统一环境
- ❌ 日常开发(推荐用开发环境)

---

## 🎬 常见场景处理

### 场景 1:我改了 index.vue 文件,怎么看到效果?

#### 开发环境(推荐)

```bash
# 第 1 步:启动开发服务(如果还没启动)
pnpm dev

# 第 2 步:修改 packages/web/buildingai-ui/app/pages/index.vue
# 第 3 步:保存文件(Ctrl+S)
# 第 4 步:等待 1-3 秒,浏览器自动刷新
# 第 5 步:完成!✨
```

**总耗时: 保存后 1-3 秒**

#### 生产环境

```bash
# 第 1 步:修改并保存文件
# 第 2 步:构建前端
pnpm build:web
# ⏱️ 等待 2-5 分钟

# 第 3 步:重启服务
pnpm pm2:restart

# 第 4 步:刷新浏览器(Ctrl+F5)
# 第 5 步:完成!✨
```

**总耗时: 2-5 分钟**

---

### 场景 2:我改了 chats-list.vue 组件,怎么看到效果?

**与场景 1 完全一样!** 所有 `.vue` 文件的处理流程都相同。

---

### 场景 3:我改了后端 API 接口,怎么测试?

#### 开发环境(推荐)

```bash
# 第 1 步:启动开发服务
pnpm dev

# 第 2 步:修改 packages/api/src/modules/*/xxx.controller.ts
# 第 3 步:保存文件
# 第 4 步:等待 2-3 秒,终端显示 "restarting..."
# 第 5 步:使用 Postman 或前端页面测试接口
# 第 6 步:完成!✨
```

**总耗时: 保存后 2-3 秒**

#### 生产环境

```bash
# 第 1 步:修改并保存文件
# 第 2 步:构建后端
pnpm build:api
# ⏱️ 等待 2-5 分钟

# 第 3 步:重启服务
pnpm pm2:restart

# 第 4 步:测试接口
# 第 5 步:完成!✨
```

**总耗时: 2-5 分钟**

---

### 场景 4:我改了样式文件,怎么看到效果?

#### 开发环境

```bash
# 第 1 步:启动开发服务
pnpm dev

# 第 2 步:修改 .css 或 .scss 文件
# 第 3 步:保存文件
# 第 4 步:等待 1-2 秒,页面自动更新(甚至不刷新)
# 第 5 步:完成!✨
```

**总耗时: 保存后 1-2 秒**

#### 生产环境

```bash
# 样式文件属于前端资源,按照场景 1 处理
pnpm build:web
pnpm pm2:restart
```

---

### 场景 5:我添加了新的 npm 包,怎么使用?

#### 开发环境

```bash
# 第 1 步:停止开发服务(Ctrl+C)

# 第 2 步:安装依赖
pnpm add <包名>
# 例如: pnpm add axios

# 第 3 步:重新启动开发服务
pnpm dev

# 第 4 步:在代码中使用新包
import axios from 'axios'

# 第 5 步:完成!✨
```

#### 生产环境

```bash
# 第 1 步:安装依赖
pnpm install

# 第 2 步:重新构建
pnpm build
# ⏱️ 等待 5-10 分钟

# 第 3 步:重启服务
pnpm pm2:restart

# 第 4 步:完成!✨
```

---

### 场景 6:我修改了 .env 文件,怎么生效?

#### 开发环境

```bash
# 第 1 步:修改 .env 文件
# 第 2 步:停止开发服务(Ctrl+C)
# 第 3 步:重新启动
pnpm dev
# 第 4 步:完成!✨
```

#### 生产环境

```bash
# 第 1 步:修改 .env 文件
# 第 2 步:重新构建(环境变量在构建时注入)
pnpm build
# ⏱️ 等待 5-10 分钟

# 第 3 步:重启服务
pnpm pm2:restart

# 第 4 步:完成!✨
```

---

## 🔧 故障排查

### 问题 1:开发环境浏览器没有自动刷新

#### 症状

- 修改了 .vue 文件
- 保存后浏览器没有自动更新
- 需要手动刷新才能看到效果

#### 诊断步骤

```bash
# 第 1 步:检查开发服务是否正在运行
# 终端应该显示类似:
# ✓ Vite server running at http://localhost:3000

# 第 2 步:检查文件是否真的保存了
# 有些编辑器需要手动保存或等待自动保存

# 第 3 步:检查浏览器控制台是否有错误
# 按 F12 打开开发者工具
# 查看 Console 标签是否有红色错误

# 第 4 步:检查是否修改了正确的文件
# 确保修改的是 packages/web/buildingai-ui/app/pages/index.vue
# 而不是其他地方的同名文件
```

#### 解决方案

```bash
# 方案 1:手动刷新浏览器
# 按 F5 或 Ctrl+F5 强制刷新

# 方案 2:重启开发服务
# Ctrl+C 停止服务
pnpm dev
# 重新启动

# 方案 3:清理缓存重启
# Ctrl+C 停止服务
pnpm clean
pnpm dev
# 重新启动

# 方案 4:检查防火墙
# 确保防火墙没有阻止 localhost:3000
```

---

### 问题 2:生产环境构建失败

#### 症状

- 运行 `pnpm build:web` 或 `pnpm build:api` 报错
- 构建过程中断
- 显示错误信息

#### 常见原因和解决方案

**原因 1: 内存不足**

```bash
# 症状: JavaScript heap out of memory
# 解决: 增加 Node.js 内存限制(已在 package.json 中配置)
# 确保系统至少有 8GB 可用内存
```

**原因 2: Node 版本不匹配**

```bash
# 检查 Node 版本
node -v
# 必须是 v22.20.x

# 如果版本不对,安装正确版本
# 使用 nvm:
nvm install 22.20.0
nvm use 22.20.0
```

**原因 3: 依赖未安装**

```bash
# 重新安装依赖
rm -rf node_modules
pnpm install
```

**原因 4: .env 文件缺失或配置错误**

```bash
# 确保根目录有 .env 文件
ls -la .env

# 如果没有,从示例文件复制
cp .env.example .env

# 然后编辑 .env 文件,填入正确的配置
```

**原因 5: 缓存问题**

```bash
# 清理所有缓存
pnpm clean
rm -rf .turbo
rm -rf node_modules
pnpm install
pnpm build
```

---

### 问题 3:PM2 服务启动失败

#### 症状

- 运行 `pnpm pm2:restart` 后服务显示 "errored"
- 服务无法启动
- 网站无法访问

#### 诊断步骤

```bash
# 第 1 步:查看 PM2 状态
pnpm pm2:status

# 第 2 步:查看错误日志
pnpm pm2:logs --lines 50
# 或
cat logs/pm2/api-error.log

# 第 3 步:检查构建产物是否存在
# 前端构建产物
ls -lh public/web/

# 后端构建产物
ls -lh packages/api/dist/main.js
```

#### 解决方案

```bash
# 方案 1:重新构建
pnpm build
pnpm pm2:restart

# 方案 2:检查端口是否被占用
# Windows
netstat -ano | findstr :4090
# Linux/Mac
lsof -i :4090

# 如果端口被占用,关闭占用的进程或修改端口

# 方案 3:完全重置 PM2
pnpm pm2:delete
pnpm start

# 方案 4:检查环境变量
cat .env
# 确保所有必需的环境变量都已配置
```

---

### 问题 4:Docker 容器启动失败

#### 症状

- 运行 `docker compose up` 后容器退出
- 容器状态显示 "Exited"
- 服务无法访问

#### 诊断步骤

```bash
# 第 1 步:查看容器状态
docker compose ps

# 第 2 步:查看容器日志
docker compose logs buildingai-app
# 查看详细错误信息

# 第 3 步:检查镜像是否正确构建
docker images | grep buildingai

# 第 4 步:进入容器检查
docker compose exec buildingai-app sh
ls -la /buildingai
```

#### 解决方案

```bash
# 方案 1:完全重建
docker compose down
docker compose up -d --build

# 方案 2:清理旧镜像和容器
docker compose down
docker system prune -a
docker compose up -d --build

# 方案 3:检查 docker-compose.yml 配置
cat docker-compose.yml
# 确保配置正确

# 方案 4:检查 .env 文件
# Docker Compose 会自动读取 .env 文件
cat .env
```

---

### 问题 5:修改后效果不生效

#### 开发环境诊断

```bash
# 第 1 步:确认开发服务正在运行
pnpm dev
# 看到 "ready" 提示

# 第 2 步:清除浏览器缓存
# Chrome: Ctrl+Shift+Delete
# 选择"缓存的图像和文件"
# 点击"清除数据"

# 第 3 步:强制刷新浏览器
# Windows/Linux: Ctrl+Shift+R
# Mac: Cmd+Shift+R

# 第 4 步:检查文件是否真的修改了
# 查看文件的修改时间
ls -lh packages/web/buildingai-ui/app/pages/index.vue

# 第 5 步:重启开发服务
# Ctrl+C 停止
pnpm dev
```

#### 生产环境诊断

```bash
# 第 1 步:确认已构建
ls -lh packages/web/buildingai-ui/.output/public/
# 检查文件时间是否是最新的

# 第 2 步:确认已重启
pnpm pm2:status
# 检查 "restart" 列的次数是否增加了

# 第 3 步:检查构建产物
# 前端
ls -lh public/web/
# 后端
ls -lh packages/api/dist/

# 第 4 步:查看服务日志
pnpm pm2:logs --lines 50
# 查找是否有错误

# 第 5 步:强制重新构建
pnpm clean
pnpm build
pnpm pm2:restart

# 第 6 步:清除浏览器缓存并强制刷新
# Ctrl+Shift+R (或 Cmd+Shift+R)
```

---

## 📊 快速参考

### 常用命令速查

| 场景 | 命令 | 耗时 |
|------|------|------|
| **开发前端** | `pnpm dev:web` | 10-30秒 |
| **开发后端** | `pnpm dev:api` | 10-30秒 |
| **同时开发** | `pnpm dev` | 20-60秒 |
| **构建前端** | `pnpm build:web` | 2-5分钟 |
| **构建后端** | `pnpm build:api` | 2-5分钟 |
| **完整构建** | `pnpm build` | 5-10分钟 |
| **重启服务** | `pnpm pm2:restart` | 5-10秒 |
| **零停机重载** | `pnpm pm2:reload` | 5-10秒 |
| **查看状态** | `pnpm pm2:status` | 即时 |
| **查看日志** | `pnpm pm2:logs` | 即时 |

### 构建命令选择指南

```
修改了什么?
    │
    ├── 只改 .vue / 前端 .ts       → pnpm build:web
    ├── 只改后端 API 代码         → pnpm build:api
    ├── 前后端都改了             → pnpm build
    └── 改了依赖包 (@buildingai/*) → pnpm build
```

### 工作流程速查表

| 场景 | 开发环境 | 生产环境 |
|-----|---------|----------|
| 改 .vue 文件 | 保存即生效 | `pnpm build:web` + `pnpm pm2:restart` |
| 改前端 .ts | 保存即生效 | `pnpm build:web` + `pnpm pm2:restart` |
| 改后端 .ts | 保存即生效 | `pnpm build:api` + `pnpm pm2:restart` |
| 改样式文件 | 保存即生效 | `pnpm build:web` + `pnpm pm2:restart` |
| 改依赖包 | 重启服务 | `pnpm build` + `pnpm pm2:restart` |
| 改 .env | 重启服务 | 重新构建 + `pnpm pm2:restart` |
| 改 package.json | `pnpm install` + 重启 | `pnpm install` + 重新构建 + 重启 |

---

## 🎓 学习建议

### 给小白的学习路径:

**第 1 天**: 只用开发环境
```bash
pnpm dev
# 修改代码,看效果,不用管构建
```

**第 2 天**: 尝试构建前端
```bash
pnpm build:web
# 理解构建过程,但不用重启
```

**第 3 天**: 完整部署流程
```bash
pnpm build:web
pnpm pm2:restart
pnpm pm2:status
# 掌握完整流程
```

**第 4 天**: 了解高级命令
```bash
pnpm build              # 完整构建
pnpm pm2:reload        # 零停机重载
pnpm pm2:logs          # 日志查看
```

---

## 📝 总结

### 最重要的两句话:

1. **开发时**: `pnpm dev` 一次启动,修改代码后只需保存,其他什么都不用管
2. **部署时**: `pnpm build:web && pnpm pm2:reload` 一条命令搞定

### 一句话记忆法:

> 💡 **开发环境 = 保存即生效**  
> 💡 **生产环境 = 构建 + 重启**  
> 💡 **Docker 环境 = 重建镜像**

### 关键提醒:

- ✅ 开发环境 = 自动热重载,无需构建
- ✅ 生产环境 = 必须构建 + 重启
- ✅ 前端改动 = `build:web`
- ✅ 后端改动 = `build:api`
- ✅ 依赖包改动 = `build`(完整构建)
- ✅ 环境变量改动 = 必须重启

---


