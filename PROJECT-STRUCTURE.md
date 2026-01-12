# 项目标准结构

```
project-root/
│
├── frontend/                      # 用户端前端 (React + TailwindCSS)
│   ├── src/
│   │   ├── components/           # 通用组件
│   │   │   ├── ui/              # 基础 UI 组件
│   │   │   └── layout/          # 布局组件
│   │   ├── pages/                # 页面
│   │   ├── services/             # API 调用
│   │   ├── hooks/                # 自定义 hooks
│   │   ├── stores/               # Zustand 状态管理
│   │   ├── utils/                # 工具函数
│   │   ├── i18n/                 # 国际化
│   │   │   ├── locales/
│   │   │   │   ├── zh.json
│   │   │   │   └── en.json
│   │   │   └── index.js
│   │   ├── styles/               # 全局样式
│   │   ├── assets/               # 静态资源
│   │   └── App.jsx
│   ├── public/
│   ├── package.json
│   ├── tailwind.config.js
│   ├── Dockerfile
│   └── .env.example
│
├── admindashboard/                # 管理后台 (Material Kit React)
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── hooks/
│   │   ├── stores/
│   │   ├── utils/
│   │   ├── i18n/
│   │   └── App.jsx
│   ├── public/
│   ├── package.json
│   ├── Dockerfile
│   └── .env.example
│
├── backend/                       # 主后端 API
│   ├── src/
│   │   ├── api/
│   │   │   └── v1/              # API 版本
│   │   │       ├── routes/
│   │   │       └── __init__.py
│   │   ├── services/             # 业务逻辑
│   │   ├── models/               # 数据模型
│   │   ├── repositories/         # 数据访问
│   │   ├── middleware/           # 中间件
│   │   ├── utils/                # 工具
│   │   └── config/               # 配置
│   ├── tests/
│   ├── requirements.txt          # Python
│   ├── package.json              # Node.js
│   ├── Dockerfile
│   ├── main.py
│   └── .env.example
│
├── server/                        # 后台服务
│   ├── src/
│   │   ├── tasks/                # 定时任务
│   │   ├── workers/              # 工作进程
│   │   ├── websocket/            # WebSocket 服务
│   │   └── queue/                # 消息队列
│   ├── requirements.txt
│   ├── Dockerfile
│   └── main.py
│
├── database/                      # 数据库
│   ├── migrations/               # 迁移文件
│   ├── seeds/                    # 种子数据
│   ├── backups/                  # 备份 (git忽略)
│   └── schema.sql                # 完整表结构
│
├── docs/                          # 文档
│   ├── README.md
│   ├── CHANGELOG.md
│   ├── api/
│   ├── architecture/
│   ├── deployment/
│   └── dev/
│
├── uploads/                       # 上传文件
│   ├── images/
│   ├── documents/
│   └── temp/
│
├── logs/                          # 日志
│   ├── app/
│   ├── error/
│   └── ISSUES.md
│
├── baota-peizhi/                  # 宝塔配置
│   ├── nginx-config.md
│   ├── supervisor-config.md
│   └── README.md
│
├── scripts/
│   ├── deploy.sh
│   └── backup-db.sh
│
├── .github/
│   └── workflows/
│       └── deploy.yml
│
├── docker-compose.yml
├── docker-compose.dev.yml
├── .env.example
├── .gitignore
└── README.md
```
