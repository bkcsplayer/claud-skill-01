# 技术规范文档

## 技术栈总览

| 层级 | 技术 |
|------|------|
| 前端 (frontend) | React + TailwindCSS + Zustand |
| 管理后台 (admindashboard) | Material Kit React |
| 后端 (backend) | FastAPI (Python) 或 Express (Node.js) |
| 数据库 | PostgreSQL |
| 缓存 | Redis |
| 实时通信 | WebSocket |
| 容器 | Docker |
| 部署 | 宝塔面板 |

---

## 前端规范

### 依赖清单

```json
{
  "dependencies": {
    "react": "^18.x",
    "react-dom": "^18.x",
    "react-router-dom": "^6.x",
    "zustand": "^4.x",
    "axios": "^1.x",
    "react-hook-form": "^7.x",
    "i18next": "^23.x",
    "react-i18next": "^13.x",
    "socket.io-client": "^4.x"
  },
  "devDependencies": {
    "tailwindcss": "^3.x",
    "autoprefixer": "^10.x",
    "postcss": "^8.x",
    "eslint": "^8.x",
    "prettier": "^3.x"
  }
}
```

### 目录规范

```
src/
├── components/
│   ├── ui/                    # 基础组件 (Button, Input, Modal...)
│   │   ├── Button.jsx
│   │   ├── Input.jsx
│   │   └── index.js          # 统一导出
│   └── layout/               # 布局组件
│       ├── Header.jsx
│       ├── Sidebar.jsx
│       └── Footer.jsx
├── pages/                     # 页面 (一个路由一个文件夹)
│   ├── Home/
│   │   ├── index.jsx
│   │   └── components/       # 页面私有组件
│   └── Login/
│       └── index.jsx
├── services/                  # API 封装
│   ├── api.js                # axios 实例
│   ├── auth.js               # 认证相关
│   └── user.js               # 用户相关
├── stores/                    # Zustand 状态
│   ├── useAuthStore.js
│   ├── useThemeStore.js
│   └── useI18nStore.js
├── hooks/                     # 自定义 hooks
│   ├── useAuth.js
│   └── useWebSocket.js
├── utils/                     # 工具函数
│   ├── format.js
│   └── storage.js
└── i18n/                      # 国际化
    ├── index.js
    └── locales/
        ├── zh.json
        └── en.json
```

### 组件规范

```jsx
// components/ui/Button.jsx
import { forwardRef } from 'react'
import { cn } from '@/utils/cn'

const Button = forwardRef(({ 
  children, 
  variant = 'primary', 
  size = 'md',
  className,
  ...props 
}, ref) => {
  return (
    <button
      ref={ref}
      className={cn(
        'rounded font-medium transition-colors',
        // variant
        variant === 'primary' && 'bg-blue-600 text-white hover:bg-blue-700',
        variant === 'secondary' && 'bg-gray-200 text-gray-800 hover:bg-gray-300',
        // size
        size === 'sm' && 'px-3 py-1.5 text-sm',
        size === 'md' && 'px-4 py-2',
        size === 'lg' && 'px-6 py-3 text-lg',
        className
      )}
      {...props}
    >
      {children}
    </button>
  )
})

export default Button
```

### 状态管理 (Zustand)

```js
// stores/useAuthStore.js
import { create } from 'zustand'
import { persist } from 'zustand/middleware'

export const useAuthStore = create(
  persist(
    (set) => ({
      user: null,
      token: null,
      setAuth: (user, token) => set({ user, token }),
      logout: () => set({ user: null, token: null }),
    }),
    { name: 'auth-storage' }
  )
)
```

### 主题切换

```js
// stores/useThemeStore.js
import { create } from 'zustand'
import { persist } from 'zustand/middleware'

export const useThemeStore = create(
  persist(
    (set) => ({
      theme: 'light', // 'light' | 'dark' | 'system'
      setTheme: (theme) => {
        set({ theme })
        if (theme === 'dark') {
          document.documentElement.classList.add('dark')
        } else {
          document.documentElement.classList.remove('dark')
        }
      },
    }),
    { name: 'theme-storage' }
  )
)
```

### 国际化 (i18n)

```js
// i18n/index.js
import i18n from 'i18next'
import { initReactI18next } from 'react-i18next'
import zh from './locales/zh.json'
import en from './locales/en.json'

i18n.use(initReactI18next).init({
  resources: {
    zh: { translation: zh },
    en: { translation: en },
  },
  lng: 'zh',
  fallbackLng: 'zh',
})

export default i18n
```

```json
// i18n/locales/zh.json
{
  "common": {
    "confirm": "确认",
    "cancel": "取消",
    "save": "保存",
    "delete": "删除"
  },
  "auth": {
    "login": "登录",
    "logout": "退出登录"
  }
}
```

### 响应式断点

```js
// tailwind.config.js
module.exports = {
  darkMode: 'class',
  content: ['./src/**/*.{js,jsx}'],
  theme: {
    screens: {
      'sm': '640px',   // 手机横屏
      'md': '768px',   // 平板
      'lg': '1024px',  // 小桌面
      'xl': '1280px',  // 桌面
      '2xl': '1536px', // 大桌面
    },
  },
}
```

---

## 后端规范

### Python (FastAPI) 方案

```
backend/
├── src/
│   ├── api/
│   │   └── v1/
│   │       ├── routes/
│   │       │   ├── auth.py
│   │       │   ├── users.py
│   │       │   └── __init__.py
│   │       └── __init__.py
│   ├── services/
│   │   ├── auth_service.py
│   │   └── user_service.py
│   ├── models/
│   │   ├── user.py
│   │   └── base.py
│   ├── repositories/
│   │   └── user_repository.py
│   ├── middleware/
│   │   ├── auth.py
│   │   └── logging.py
│   ├── utils/
│   │   ├── security.py
│   │   └── response.py
│   └── config/
│       ├── settings.py
│       └── database.py
├── main.py
└── requirements.txt
```

```txt
# requirements.txt
fastapi==0.104.1
uvicorn==0.24.0
sqlalchemy==2.0.23
alembic==1.12.1
asyncpg==0.29.0
pydantic==2.5.2
pydantic-settings==2.1.0
python-jose[cryptography]==3.3.0
passlib[bcrypt]==1.7.4
python-multipart==0.0.6
redis==5.0.1
websockets==12.0
```

### Node.js (Express) 方案

```
backend/
├── src/
│   ├── api/
│   │   └── v1/
│   │       ├── routes/
│   │       │   ├── auth.js
│   │       │   └── users.js
│   │       └── index.js
│   ├── services/
│   ├── models/
│   ├── repositories/
│   ├── middleware/
│   ├── utils/
│   └── config/
├── main.js
└── package.json
```

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "prisma": "^5.6.0",
    "@prisma/client": "^5.6.0",
    "jsonwebtoken": "^9.0.2",
    "bcryptjs": "^2.4.3",
    "cors": "^2.8.5",
    "helmet": "^7.1.0",
    "socket.io": "^4.7.2",
    "redis": "^4.6.10"
  }
}
```

---

## API 规范

### 基础格式

```json
// 成功响应
{
  "code": 200,
  "message": "success",
  "data": { ... }
}

// 分页响应
{
  "code": 200,
  "message": "success",
  "data": {
    "items": [...],
    "total": 100,
    "page": 1,
    "page_size": 20
  }
}

// 错误响应
{
  "code": 400,
  "message": "参数错误",
  "details": {
    "field": "email",
    "error": "邮箱格式不正确"
  }
}
```

### 错误码

| 范围 | 说明 |
|------|------|
| 200 | 成功 |
| 400 | 请求参数错误 |
| 401 | 未认证 |
| 403 | 无权限 |
| 404 | 资源不存在 |
| 500 | 服务器错误 |

### 接口路径

```
POST   /api/v1/auth/login          # 登录
POST   /api/v1/auth/register       # 注册
POST   /api/v1/auth/refresh        # 刷新 token

GET    /api/v1/users               # 用户列表
GET    /api/v1/users/:id           # 用户详情
POST   /api/v1/users               # 创建用户
PUT    /api/v1/users/:id           # 更新用户
DELETE /api/v1/users/:id           # 删除用户

POST   /api/v1/upload/image        # 上传图片
```

### 认证

```
Authorization: Bearer <access_token>
```

---

## WebSocket 规范

### 连接

```js
// 前端
const socket = io('wss://example.com', {
  auth: { token: 'xxx' }
})
```

### 事件

```js
// 服务端 → 客户端
socket.emit('notification', { type: 'info', message: '新消息' })
socket.emit('data_update', { table: 'orders', action: 'create', data: {...} })

// 客户端 → 服务端
socket.emit('subscribe', { channel: 'orders' })
socket.emit('unsubscribe', { channel: 'orders' })
```

---

## 数据库规范

### 命名

- 表名: snake_case 复数 (`users`, `order_items`)
- 列名: snake_case (`created_at`, `user_id`)
- 索引: `idx_{table}_{columns}`
- 外键: `fk_{table}_{ref_table}`

### 必备字段

```sql
id SERIAL PRIMARY KEY,
created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
```

### 迁移工具

- Python: Alembic
- Node.js: Prisma

---

## 文件命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| React 组件 | PascalCase | `UserCard.jsx` |
| 工具/hooks | camelCase | `useAuth.js` |
| 样式 | kebab-case | `user-card.css` |
| Python | snake_case | `user_service.py` |
| 常量 | UPPER_SNAKE | `API_BASE_URL` |
