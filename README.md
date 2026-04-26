# 小野AI 内容创作平台

<div align="center">
  <a href="https://ai.xiaoye.io/" target="_blank">
    <img src="docs/screenshots/xiaoye.png" alt="XiaoYe API" width="96" />
  </a>
  <h3>小野 API 中转站</h3>
  <p>统一的大模型接口网关，更好的价格，更好的稳定性。支持 OpenAI、Claude、Gemini、DeepSeek、Qwen、Volcengine、Moonshot AI 等 30+ 主流模型供应商。</p>
  <p><a href="https://ai.xiaoye.io/"><strong>立即访问 https://ai.xiaoye.io/</strong></a></p>
</div>

[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](LICENSE)

开源多模态 AI 内容创作平台，支持 Google Gemini、火山引擎 Seedream/Seedance 等模型进行图像和视频生成。

**在线体验: [https://muse-ai.nlink.vip/](https://muse-ai.nlink.vip/)**

[中文](#功能特性) | [English](README_EN.md)

## 截图预览

| 灵感广场                                 | 生成界面                                      |
|--------------------------------------|-------------------------------------------|
| ![生成](docs/screenshots/generate.png) | ![灵感广场](docs/screenshots/inspiration.png) |

## 功能特性

- **AI 图像生成** — 多模型切换 (Gemini, Seedream)，支持参考图，最高 4K 分辨率
- **AI 视频生成** — 文生视频 & 图生视频 (Seedance, Veo 3.1)，异步任务轮询
- **电商图片批量生成** — 模板化批量生成商品图
- **提示词优化** — 基于 DeepSeek 的 AI 提示词改写
- **反向提示词** — 从图片提取生成提示词
- **灵感广场** — 社区作品展示，点赞、Remix、审核
- **积分系统** — 密钥兑换、每日签到、邀请奖励、在线充值
- **用户系统** — 邮箱注册登录、Linux.do OAuth、邮箱绑定
- **国际化** — 中文 & 英文

## 技术栈

| 层级 | 技术 |
|------|------|
| 前端 | Vue 3, Naive UI, Pinia, Vue Router, Vite |
| 后端 | Go (Gin), GORM, MySQL, JWT |
| AI 供应商 | Google Gemini, 火山引擎 (豆包) |
| 存储 | 阿里云 OSS |
| 管理后台 | Vue 3 SPA (独立构建) |

## 项目结构

```
.
├── frontend/                # Vue 3 前端应用
│   └── src/
│       ├── views/           # 页面
│       ├── components/      # 可复用组件
│       ├── composables/     # 组合式 API hooks
│       ├── stores/          # Pinia 状态管理
│       ├── locales/         # 国际化 (zh/en)
│       └── router/
├── frontend-admin/          # 管理后台审核面板
├── backend/
│   ├── cmd/                 # CLI 工具 (密钥生成等)
│   ├── internal/
│   │   ├── api/             # HTTP 处理器 & 中间件
│   │   ├── api/admin/       # 管理后台 API
│   │   ├── auth/            # JWT 认证
│   │   ├── config/          # 环境配置
│   │   ├── db/              # 数据库模型 & 初始化
│   │   ├── email/           # SMTP 邮件服务
│   │   ├── payment/         # 支付集成
│   │   ├── provider/        # AI 供应商适配器
│   │   └── storage/         # OSS 存储
│   └── migrations/          # SQL 迁移脚本
└── LICENSE
```

## 快速开始

### 环境要求

- Go 1.21+
- Node.js 18+
- MySQL 8.0+
- 阿里云 OSS 存储桶 (用于媒体存储)
- 至少一个 AI 供应商 API Key (Google Gemini 或火山引擎)

### 后端

```bash
cd backend
cp .env.example .env    # 编辑 .env 填入你的配置
go run main.go          # 启动于 :8092
```

### 前端

```bash
cd frontend
npm install
npm run dev             # 启动于 http://localhost:5173
```

### 管理后台

```bash
cd frontend-admin
npm install
npm run dev             # 启动于 http://localhost:5174
```

## 配置说明

所有配置通过环境变量管理。复制 `backend/.env.example` 为 `backend/.env` 并填入对应值。

### 必填项

| 变量 | 说明 |
|------|------|
| `DB_USER` | MySQL 用户名 |
| `DB_PASSWORD` | MySQL 密码 |
| `DB_NAME` | MySQL 数据库名 |
| `JWT_SECRET` | JWT 签名密钥 (使用强随机字符串) |
| `GOOGLE_API_KEY` | Google Gemini API Key ([获取](https://aistudio.google.com/app/apikey)) |

### 可选项

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `DB_HOST` | MySQL 主机 | `localhost` |
| `DB_PORT` | MySQL 端口 | `3306` |
| `PORT` | 后端服务端口 | `8092` |
| `HTTP_PROXY` | 外部 API 调用代理 | - |
| `ARK_API_KEY` | 火山引擎 API Key (Seedream/Seedance) | - |
| `DEEPSEEK_API_KEY` | DeepSeek API Key (提示词优化) | - |
| `DEEPSEEK_BASE_URL` | DeepSeek API 基础地址 | `https://api.deepseek.com` |
| `DEEPSEEK_MODEL` | DeepSeek 模型名 | `deepseek-chat` |
| `PROMPT_OPTIMIZE_CREDITS` | 提示词优化消耗积分 | `1` |
| `OSS_ENDPOINT` | 阿里云 OSS Endpoint | - |
| `OSS_ACCESS_KEY_ID` | OSS Access Key ID | - |
| `OSS_ACCESS_KEY_SECRET` | OSS Access Key Secret | - |
| `OSS_BUCKET_NAME` | OSS 存储桶名 | - |
| `OSS_REGION` | OSS 地域 | - |
| `OSS_PUBLIC_DOMAIN` | OSS 自定义域名 | - |
| `SMTP_HOST` | SMTP 服务器地址 | - |
| `SMTP_PORT` | SMTP 端口 | - |
| `SMTP_USER` | SMTP 用户名 | - |
| `SMTP_PASSWORD` | SMTP 密码 / 授权码 | - |
| `SMTP_FROM_EMAIL` | 发件人邮箱 | - |
| `SMTP_FROM_NAME` | 发件人名称 | - |
| `ADMIN_TOKEN` | 管理后台认证 Token | - |
| `INSPIRATION_AUTO_APPROVE` | 灵感帖子自动审核通过 | `false` |
| `CORS_ORIGINS` | CORS 允许来源 (逗号分隔) | `http://localhost:5173,http://localhost:5174` |
| `LINUXDO_CLIENT_ID` | Linux.do OAuth Client ID | - |
| `LINUXDO_CLIENT_SECRET` | Linux.do OAuth Client Secret | - |
| `OAUTH_REDIRECT_URL` | OAuth 回调地址 | - |
| `LINUXDO_CREDIT_PID` | Linux.do Credit 商户 ID | - |
| `LINUXDO_CREDIT_KEY` | Linux.do Credit 商户密钥 | - |
| `LINUXDO_CREDIT_NOTIFY_URL` | 支付回调地址 | - |
| `LINUXDO_CREDIT_RETURN_URL` | 支付完成跳转地址 | - |

## 数据库

后端使用 GORM 自动迁移，首次启动时自动创建表结构。`backend/migrations/` 目录下的 SQL 脚本供参考和手动变更使用。

## 部署

### 生产构建

```bash
# 前端
cd frontend && npm run build        # 产物: frontend/dist/

# 管理后台
cd frontend-admin && npm run build  # 产物: frontend-admin/dist/

# 后端
cd backend && go build -o server main.go
```

### Nginx 配置示例

```nginx
server {
    listen 443 ssl;
    server_name your-domain.com;

    # 前端
    location / {
        root /path/to/frontend/dist;
        try_files $uri $uri/ /index.html;
    }

    # 后端 API
    location /api/ {
        proxy_pass http://127.0.0.1:8092;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

生产环境在 `.env` 中设置 `CORS_ORIGINS=https://your-domain.com`。

## 参与贡献

1. Fork 本仓库
2. 创建功能分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add amazing feature'`)
4. 推送分支 (`git push origin feature/amazing-feature`)
5. 发起 Pull Request

## 未来功能

- **画布支持** — 可视化画布编辑器，支持图层操作、局部重绘与自由拼接

## 致谢

感谢 [Linux.do](https://linux.do) 社区的交流与支持，项目的许多想法和改进都来自社区成员的反馈。

## 开源协议

本项目基于 [AGPL-3.0](LICENSE) 协议开源。如果你将修改后的版本部署为网络服务，必须向该服务的用户公开源代码。
