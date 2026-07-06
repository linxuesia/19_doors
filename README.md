# 19分贝门窗 — 高端系统门窗全栈管理平台

<p align="center">
  <strong>B2B SaaS 门窗行业数字化解决方案</strong><br/>
  微信小程序 · H5 · 多角色协作 · 工单流转 · 数据看板
</p>

<p align="center">
  <img src="https://img.shields.io/badge/frontend-Taro_4_%2B_React_18-blue" alt="frontend"/>
  <img src="https://img.shields.io/badge/backend-NestJS_%2B_Prisma_%2B_MySQL-red" alt="backend"/>
  <img src="https://img.shields.io/badge/auth-JWT_%2B_RBAC-green" alt="auth"/>
  <img src="https://img.shields.io/badge/license-MIT-brightgreen" alt="license"/>
</p>

<p align="center">
  <a href="https://www.axureshow.com/project/hBFkYgxf/">
    <img src="https://img.shields.io/badge/在线原型-Axure-8A2BE2?style=for-the-badge" alt="在线原型"/>
  </a>
</p>

---

## 项目简介

**19分贝门窗** 是一套面向高端系统门窗行业的全栈数字化管理平台：

- **C端**（微信小程序）：产品展示、装修案例、门店查找、在线预约量尺
- **B端**（H5管理后台）：安装工/店长/老板/总台管理员 多角色协作
- **核心流程**：门店入驻审批 → 客户预约量尺 → 施工工单流转 → 施工日志上传 → 数据看板

🏠 **真实业务场景**：已在上海多家直营/加盟门店投入使用。

---

## 架构概览

```
┌─────────────────────────────────────────────────┐
│                   19_doors (根仓库)               │
│  ┌─────────────────┐  ┌───────────────────────┐  │
│  │   backend/       │  │   frontend/            │  │
│  │   (submodule)   │  │   (submodule)         │  │
│  │                 │  │                       │  │
│  │  NestJS + Prisma │  │  Taro 4 + React 18    │  │
│  │  MySQL + JWT     │  │  TailwindCSS + TS     │  │
│  │  → 微信小程序API  │  │  → 微信小程序 + H5     │  │
│  └─────────────────┘  └───────────────────────┘  │
│                                                   │
│  独立仓库，独立部署，独立 CI/CD                     │
└─────────────────────────────────────────────────┘
```

| 层级 | 仓库 | 技术栈 |
|------|------|--------|
| 前端 | [19_doors_frontend](https://github.com/linxuesia/19_doors_frontend) | Taro 4.x + React 18 + TypeScript + TailwindCSS |
| 后端 | [19_doors_backend](https://github.com/linxuesia/19_doors_backend) | NestJS + Prisma ORM + MySQL + JWT + RBAC |

> **注意**：backend 和 frontend 均为独立 Git 仓库，拥有各自的提交历史、CI/CD 流水线和部署配置。根仓库通过 git submodule 关联，仅用于展示全栈架构全貌。

---

## 功能模块

### C端（微信小程序）

- 产品系列展示（图片/规格/价格）
- 装修案例浏览（图文+户型标签）
- 门店查找（按位置/距离排序）
- 在线预约量尺（选择门店+时间）
- 订单进度查询

### B端（多角色管理后台）

| 角色 | 功能 |
|------|------|
| **安装工** | 查看分配工单 → 上传施工照片 → 填写施工日志 → 标记完成 |
| **店长** | 量尺预约管理 → 工单指派 → 进度审核 → 门店信息维护 |
| **老板** | 多门店数据看板 → 营收统计 → 员工管理 → 审批入驻申请 |
| **总台管理** | 门店入驻审批 → 全平台数据看板 → 系统配置 |

---

## 快速开始

### 克隆（含子模块）

```bash
git clone --recurse-submodules git@github.com:linxuesia/19_doors.git
```

### 后端

```bash
cd backend
cp .env.example .env   # 编辑 .env 填入数据库连接等信息
npm install
npx prisma db push
npm run dev             # http://localhost:3000
```

### 前端

```bash
cd frontend
npm install
npm run dev:h5          # H5 开发模式
# 或
npm run dev:weapp       # 微信小程序开发模式
```

### 一键启动

```bash
npm install             # 安装 concurrently
npm run dev             # 同时启动前后端
```

---

## 项目文档

| 文档 | 说明 |
|------|------|
| [技术详设.md](./技术详设.md) | 完整技术设计文档（数据库ER、API设计、权限模型） |
| [在线原型 (Axure)](https://www.axureshow.com/project/hBFkYgxf/) | 可交互的完整产品原型 |
| [backend/API.md](./backend/API.md) | 后端 API 接口文档 |
| [backend/TODO.md](./backend/TODO.md) | 后端开发待办 |

---

## 安全设计

- **JWT 认证**：无状态 Token，支持刷新机制
- **RBAC 权限**：5 种角色，接口级权限校验（NestJS Guard）
- **越权防护**：门店数据隔离，用户仅能访问所属门店资源
- **限流保护**：敏感接口（登录、验证码）频率限制

---

## 子模块说明

本项目采用 **git submodule** 组织：

- `backend/` → [linxuesia/19_doors_backend](https://github.com/linxuesia/19_doors_backend)
- `frontend/` → [linxuesia/19_doors_frontend](https://github.com/linxuesia/19_doors_frontend)

**日常开发**：分别在 `backend/` 和 `frontend/` 目录内独立 `git commit` + `git push`，与普通仓库完全一致，互不影响。

**更新子模块引用**（根仓库记录最新提交）：
```bash
cd backend
git pull                    # 拉取最新代码
cd ..
git add backend              # 根仓库记录 backend 最新 commit
git commit -m "更新 backend 子模块"
git push
```

---

## License

MIT
