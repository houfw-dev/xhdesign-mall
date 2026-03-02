
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

**创建日期：2026-03-01**

---

## 项目概览

**yshop-pro** 是一个电商商城系统，版本 2.3.0，包含三个主要子项目：

| 目录 | 技术栈 | 说明 |
|------|--------|------|
| `yshop-pro/` | Java 17 + Spring Boot 3.2.2 + MyBatis-Plus | 后端服务 |
| `yshop-pro-vue3/` | Vue 3.4 + TypeScript + Vite + Element Plus | 管理后台前端 |
| `yshop-pro-uniapp/` | UniApp + Vue 3 + Vant | 移动端/小程序 |

---

## 项目架构

### 后端架构（yshop-pro）

采用 Maven 多模块架构：

```
yshop-pro/
├── yshop-dependencies/      # 依赖版本管理
├── yshop-framework/         # 框架层
├── yshop-server/            # 启动模块（主入口）
└── yshop-module-*/          # 业务模块
    ├── yshop-module-mall/   # 商城核心（商品、订单等）
    ├── yshop-module-member/ # 会员模块
    ├── yshop-module-pay/    # 支付模块
    ├── yshop-module-system/ # 系统模块
    ├── yshop-module-mp/     # 微信小程序/公众号
    ├── yshop-module-message/# 消息模块
    ├── yshop-module-express/# 快递模块
    └── yshop-module-infra/  # 基础设施
```

**主启动类：** `co.yixiang.yshop.server.YshopServerApplication`

**配置文件位置：** `yshop-server/src/main/resources/`
- `application.yaml` - 主配置
- `application-local.yaml` - 本地开发配置
- `application-dev.yaml` - 开发环境配置

**关键配置（application-local.yaml）：**
- 服务端口：48081
- 数据库：MySQL (jdbc:mysql://127.0.0.1:3306/b2c-mall-boot)
- 缓存：Redis (127.0.0.1:6379)

### 前端架构（yshop-pro-vue3）

```
yshop-pro-vue3/
├── src/
│   ├── api/              # API 接口定义
│   │   └── mall/product/ # 商城商品相关 API
│   ├── views/            # 页面组件
│   └── config/           # 配置（axios 等）
├── .env                  # 基础环境变量
├── .env.dev              # 开发环境变量
└── vite.config.ts        # Vite 配置
```

**API 地址配置：** `.env.dev` 中的 `VITE_BASE_URL`

### 移动端架构（yshop-pro-uniapp）

```
yshop-pro-uniapp/
├── api/                  # API 接口定义
│   └── product.js        # 商品 API
├── pages/                # 页面
├── components/           # 组件
├── utils/
│   └── request.js        # 请求工具
└── uni_modules/          # uv-ui 组件库
```

---

## 常用命令

### 后端命令

```bash
# 进入后端目录
cd yshop-pro

# 编译项目（跳过测试）
mvn clean install -DskipTests

# 启动服务
cd yshop-server
mvn spring-boot:run
```

### 管理后台前端命令

```bash
# 进入前端目录
cd yshop-pro-vue3

# 安装依赖
pnpm install
# 或
npm install --legacy-peer-deps

# 启动开发服务器
pnpm dev
# 或
npm run dev

# 类型检查
npm run ts:check

# 代码 lint
npm run lint:eslint

# 构建生产版本
npm run build:prod
```

### 移动端命令

```bash
# 进入移动端目录
cd yshop-pro-uniapp

# 安装依赖
npm install

# 使用 HBuilderX 打开项目进行开发
```

---

## 典型查询流程示例

以商品列表查询为例：

| 层级 | 文件路径 |
|------|---------|
| 前端 API | `yshop-pro-uniapp/api/product.js` - `getProductList()` |
| 后端 Controller | `yshop-pro/yshop-module-mall/yshop-module-product-biz/.../AppStoreProductController.java` - `goodsList()` |
| 后端 Service | `yshop-pro/yshop-module-mall/yshop-module-product-biz/.../AppStoreProductServiceImpl.java` - `getGoodsList()` |
| 后端 Mapper | `yshop-pro/yshop-module-mall/yshop-module-product-biz/.../StoreProductMapper.java` |
| 数据库表 | `yshop_store_product` |

---

## 关键技术栈

| 组件 | 技术 |
|------|------|
| ORM | MyBatis-Plus |
| 缓存 | Redis + Redisson |
| 数据库连接池 | Druid |
| 权限验证 | Spring Security |
| API 文档 | Knife4j/Swagger |
| 前端构建 | Vite |
| 状态管理 | Pinia |
| UI 组件库 | Element Plus (后台) / Vant (移动端) |
