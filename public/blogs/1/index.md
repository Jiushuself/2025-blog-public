# 2025 Blog 项目功能详解与开发指南

> 本项目是一个基于 Next.js 和 GitHub API 的现代化个人博客系统，支持前端直接管理内容、拖拽式布局、在线写作等功能。

## 一、项目概览

### 技术栈
- **框架**: Next.js 16 (App Router)
- **UI 框架**: React 19
- **样式**: Tailwind CSS 4
- **状态管理**: Zustand
- **动画**: Motion One
- **Markdown 渲染**: Marked + Shiki (代码高亮)
- **GitHub 集成**: GitHub App + JWT 认证
- **部署**: Vercel / Cloudflare Pages

### 核心特性
- ✅ 拖拽式首页布局编辑
- ✅ 在线写作与预览 (支持 Markdown)
- ✅ GitHub 自动同步 (通过 GitHub App)
- ✅ 图片管理与压缩
- ✅ 分类系统
- ✅ RSS 与 Sitemap 自动生成
- ✅ 响应式设计 (桌面端/移动端适配)
- ✅ 主题颜色配置
- ✅ 缓读功能 (阅读进度追踪)

---

## 二、项目结构详解

```
2025-blog-public/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── (home)/            # 首页模块
│   │   │   ├── page.tsx       # 首页主组件
│   │   │   ├── hi-card.tsx    # 问候卡片 (可编辑)
│   │   │   ├── art-card.tsx   # 艺术展示卡片
│   │   │   ├── clock-card.tsx # 时钟卡片
│   │   │   ├── calendar-card.tsx # 日历卡片
│   │   │   ├── music-card.tsx # 音乐卡片
│   │   │   ├── social-buttons.tsx # 社交链接按钮
│   │   │   ├── share-card.tsx # 分享卡片
│   │   │   ├── aritcle-card.tsx # 文章列表卡片
│   │   │   ├── write-buttons.tsx # 写作入口按钮
│   │   │   ├── like-position.tsx # 点赞位置
│   │   │   ├── hat-card.tsx  # 帽子卡片 (彩蛋)
│   │   │   ├── config-dialog/ # 配置弹窗
│   │   │   ├── stores/        # 状态管理
│   │   │   └── services/      # 业务逻辑
│   │   ├── blog/              # 博客文章列表
│   │   │   ├── page.tsx       # 文章列表页
│   │   │   ├── [id]/          # 文章详情页 (动态路由)
│   │   │   ├── components/    # 博客相关组件
│   │   │   └── services/      # 博客服务
│   │   ├── write/             # 写作编辑器
│   │   │   ├── page.tsx       # 写作页面
│   │   │   ├── [slug]/        # 编辑已有文章
│   │   │   ├── components/    # 编辑器组件 (编辑器/侧边栏/预览/操作)
│   │   │   ├── stores/        # 写作状态管理
│   │   │   └── services/      # 发布/保存服务
│   │   ├── pictures/          # 图片管理页
│   │   ├── bloggers/          # 博主信息页
│   │   ├── about/             # 关于页面
│   │   ├── projects/          # 项目展示页
│   │   ├── music/             # 音乐页面
│   │   ├── clock/             # 时钟页面
│   │   ├── image-toolbox/     # 图片工具箱
│   │   ├── share/             # 分享页面
│   │   ├── rss.xml/           # RSS 生成
│   │   ├── sitemap.ts         # Sitemap 生成
│   │   └── layout.tsx         # 全局布局
│   │
│   ├── components/            # 公共组件
│   │   ├── card.tsx           # 基础卡片组件
│   │   ├── nav-card.tsx       # 导航卡片 (全局)
│   │   ├── blog-preview.tsx   # 博客预览
│   │   ├── blog-sidebar.tsx   # 博客侧边栏
│   │   ├── blog-toc.tsx       # 文章目录
│   │   ├── color-picker.tsx   # 颜色选择器
│   │   ├── color-picker-panel.tsx # 颜色面板
│   │   ├── code-block.tsx     # 代码块组件
│   │   ├── dialog-modal.tsx   # 模态框
│   │   ├── editable-star-rating.tsx # 可编辑评分
│   │   ├── like-button.tsx    # 点赞按钮
│   │   ├── markdown-image.tsx # Markdown 图片
│   │   ├── select.tsx         # 下拉选择
│   │   ├── star-rating.tsx    # 星级评分
│   │   ├── scroll-top-button.tsx # 返回顶部
│   │   ├── wip.tsx            # 开发中组件
│   │   └── liquid-grass/      # 液态草动画组件
│   │
│   ├── config/                # 配置文件
│   │   ├── site-content.json  # 网站内容配置 (主题/社交/元数据)
│   │   ├── card-styles.json   # 卡片样式配置 (位置/尺寸/启用状态)
│   │   └── card-styles-default.json # 默认样式备份
│   │
│   ├── layout/                # 布局组件
│   │   ├── index.tsx          # 主布局组件
│   │   ├── header.tsx         # 头部
│   │   ├── footer.tsx         # 底部
│   │   ├── head.tsx           # Head 元数据
│   │   └── backgrounds/       # 背景动画
│   │
│   ├── lib/                   # 工具库
│   │   ├── github-client.ts   # GitHub API 客户端 (核心)
│   │   ├── auth.ts            # 认证相关
│   │   ├── blog-index.ts      # 博客索引生成
│   │   ├── color.ts           # 颜色处理工具
│   │   ├── file-utils.ts      # 文件处理工具
│   │   ├── load-blog.ts       # 加载博客文章
│   │   ├── markdown-renderer.ts # Markdown 渲染器
│   │   ├── utils.ts           # 通用工具
│   │   └── log.ts             # 日志工具
│   │
│   ├── hooks/                 # 自定义 Hooks
│   │   ├── use-auth.ts        # 认证状态
│   │   ├── use-blog-index.ts  # 博客索引
│   │   ├── use-categories.ts  # 分类管理
│   │   ├── use-center.ts      # 居中计算
│   │   ├── use-markdown-render.tsx # Markdown 渲染 Hook
│   │   ├── use-read-articles.ts # 阅读记录
│   │   └── use-size.ts        # 响应式尺寸
│   │
│   ├── styles/                # 样式文件
│   │   └── globals.css        # 全局样式
│   │
│   └── svgs/                  # SVG 图标
│
├── public/                    # 静态资源
│   ├── images/                # 图片资源
│   │   ├── art/              # 艺术图片
│   │   ├── blogger/          # 博主图片
│   │   ├── hats/             # 帽子图片
│   │   ├── pictures/         # 用户上传图片
│   │   ├── share/            # 分享图片
│   │   ├── avatar.png        # 头像
│   │   └── cursor.svg        # 自定义光标
│   └── blogs/                 # 博客文章缓存 (存储在 public 目录)
│
├── scripts/                   # 脚本工具
│   └── gen-svgs-index.js      # SVG 索引生成
│
├── package.json               # 项目依赖
├── next.config.ts             # Next.js 配置
├── tsconfig.json              # TypeScript 配置
├── postcss.config.mjs         # PostCSS 配置
├── wrangler.toml              # Cloudflare 配置
└── README.md                  # 项目说明
```

---

## 三、核心功能模块详解

### 1. GitHub 集成系统 (核心)

**文件路径**: `src/lib/github-client.ts`

**功能说明**:
- 通过 GitHub App 进行认证
- JWT Token 生成与管理
- GitHub API 封装 (读取/写入/更新)
- 文件内容的 Base64 编解码
- 错误处理 (401/422)

**如何修改配置**:
```typescript
// src/consts.ts
export const GITHUB_CONFIG = {
  OWNER: process.env.NEXT_PUBLIC_GITHUB_OWNER || 'yysuni',      // 仓库所有者
  REPO: process.env.NEXT_PUBLIC_GITHUB_REPO || '2025-blog-public', // 仓库名
  BRANCH: process.env.NEXT_PUBLIC_GITHUB_BRANCH || 'main',       // 分支
  APP_ID: process.env.NEXT_PUBLIC_GITHUB_APP_ID || '-',          // GitHub App ID
  ENCRYPT_KEY: process.env.NEXT_PUBLIC_GITHUB_ENCRYPT_KEY || 'wudishiduomejimo', // 加密密钥
} as const
```

**部署时需要配置的环境变量**:
- `NEXT_PUBLIC_GITHUB_OWNER`: 你的 GitHub 用户名
- `NEXT_PUBLIC_GITHUB_REPO`: 你的仓库名
- `NEXT_PUBLIC_GITHUB_APP_ID`: GitHub App 的 App ID
- `NEXT_PUBLIC_GITHUB_ENCRYPT_KEY`: 用于加密的密钥 (可选)

### 2. 首页布局系统

**文件路径**: `src/app/(home)/`

**核心特性**:
- **拖拽编辑**: 支持在首页按住卡片拖拽调整位置
- **实时保存**: 可保存拖拽后的布局到本地存储
- **配置开关**: 每个卡片可以单独启用/禁用
- **响应式**: 桌面端显示完整布局，移动端简化

**可配置卡片**:
1. **Art Card** - 艺术展示卡片 (`art-card.tsx`)
2. **Hi Card** - 问候卡片 (`hi-card.tsx`) - 可修改头像、问候语
3. **Clock Card** - 实时时钟 (`clock-card.tsx`)
4. **Calendar Card** - 日历 (`calendar-card.tsx`)
5. **Music Card** - 音乐播放器 (`music-card.tsx`)
6. **Social Buttons** - 社交链接 (`social-buttons.tsx`)
7. **Share Card** - 分享卡片 (`share-card.tsx`)
8. **Article Card** - 最近文章 (`aritcle-card.tsx`)
9. **Write Buttons** - 写作按钮 (`write-buttons.tsx`)
10. **Like Position** - 点赞位置 (`like-position.tsx`)
11. **Hat Card** - 帽子彩蛋 (`hat-card.tsx`)

**如何修改首页内容**:

**方式1: 通过配置弹窗 (推荐)**
- 按 `Ctrl + L` 或 `Cmd + ,` 打开配置弹窗
- 可修改主题颜色、社交链接、显示开关等

**方式2: 修改配置文件**
```json
// src/config/site-content.json - 修改网站元数据和主题
{
  "meta": {
    "title": "你的博客标题",
    "description": "博客描述",
    "username": "你的名字"
  },
  "theme": {
    "colorBrand": "#35bfab",     // 主品牌色
    "colorPrimary": "#334f52",   // 主色调
    "colorBg": "#eeeeee",        // 背景色
    "colorCard": "#ffffff66"     // 卡片背景
  },
  "socialButtons": [
    {
      "type": "github",          // 类型: github/juejin/xiaohongshu/tiktok/email
      "value": "https://github.com/你的用户名",
      "label": "",
      "order": 1
    }
  ]
}
```

```json
// src/config/card-styles.json - 修改卡片样式和位置
{
  "hiCard": {
    "width": 360,      // 宽度
    "height": 288,     // 高度
    "order": 1,        // 层级
    "offsetX": null,   // X轴偏移 (null表示自动居中)
    "offsetY": null,   // Y轴偏移
    "enabled": true    // 是否启用
  }
}
```

**方式3: 修改组件代码**
```tsx
// src/app/(home)/hi-card.tsx - 修改问候卡片内容
export default function HiCard() {
  // 可修改: 头像路径、问候语、用户名显示等
  <img src='/images/avatar.png' ... />
  <h1 className='font-averia mt-3 text-2xl'>
    {greeting} <br /> I'm <span className='text-linear text-[32px]'>{username}</span>
  </h1>
}
```

**方式4: 拖拽编辑**
1. 进入首页
2. 点击右上角的"编辑"按钮或按 `Ctrl + L`
3. 拖拽卡片调整位置
4. 点击"保存偏移"按钮
5. **注意**: 这只保存到前端本地存储，需要提交到 GitHub 才能永久保存

### 3. 写作系统

**文件路径**: `src/app/write/`

**核心功能**:
- **Markdown 编辑器**: 实时写作与预览
- **图片管理**: 上传图片后自动插入 Markdown
- **元数据配置**: 标题、分类、标签、发布时间
- **预览模式**: 查看最终渲染效果
- **发布/保存**: 保存到 GitHub 仓库

**如何写作**:

**创建新文章**:
1. 访问 `/write` 页面
2. 填写文章信息:
   - **标题**: 文章标题
   - **Slug**: URL 路径 (如: `my-first-post`)
   - **分类**: 选择或创建分类
   - **发布时间**: 默认为当前时间
   - **摘要**: 简短描述
   - **封面图**: URL 或本地上传
3. 在编辑器中写作 Markdown
4. **图片上传**: 点击 `+` 按钮上传图片，然后拖拽到编辑器中
5. 点击右上角"预览"查看效果
6. 点击"发布"按钮保存到 GitHub

**编辑已有文章**:
1. 在博客列表页找到文章
2. 点击文章进入详情页
3. 点击右上角"编辑"按钮
4. 修改内容后重新发布

**支持的 Markdown 语法**:
- 标题: `# H1`, `## H2`, `### H3`
- 粗体: `**text**`
- 斜体: `*text*`
- 链接: `[text](url)`
- 图片: `![alt](url)`
- 代码: `` `code` ``
- 代码块: ```` ```lang\ncode\n``` ````
- 列表: `- item` or `1. item`
- 引用: `> quote`
- 表格: 使用 `|` 分隔

**Markdown 渲染器**:
- 使用 `marked` 解析 Markdown
- 使用 `shiki` 进行代码语法高亮
- 自定义组件渲染图片(`markdown-image.tsx`)
- 支持渲染 HTML (通过 `html-react-parser`)

### 4. 博客展示系统

**文件路径**: `src/app/blog/`

**功能**:
- **文章列表**: 按时间/分类/标签展示
- **文章详情**: Markdown 渲染与展示
- **目录生成**: 自动生成 TOC
- **阅读进度**: 缓读功能记录阅读状态
- **编辑模式**: 认证用户可直接编辑
- **分类管理**: 动态分类系统

**展示模式**:
- **按年**: `year` - 默认模式
- **按月**: `month`
- **按周**: `week`
- **按日**: `day`
- **按分类**: `category`

**如何修改博客列表展示**:
```tsx
// src/app/blog/page.tsx - 主要逻辑在第 41 行
const [displayMode, setDisplayMode] = useState<DisplayMode>('year') // 默认模式
```

**文章详情页组件**:
- `blog-preview.tsx` - 文章预览
- `blog-sidebar.tsx` - 侧边栏 (作者信息等)
- `blog-toc.tsx` - 文章目录

### 5. 图片管理系统

**文件路径**: `public/images/`

**图片类型**:
- `/images/art/` - 艺术卡片展示图片
- `/images/avatar.png` - 头像
- `/images/pictures/` - 用户上传图片
- `/images/hats/` - 帽子彩蛋图片

**如何管理图片**:

**方式1: 静态资源 (推荐)**
1. 将图片放入 `public/images/` 对应目录
2. 在代码中直接使用相对路径: `src="/images/xxx.png"`

**方式2: GitHub 存储**
1. 在写作页面上传图片
2. 图片会自动插入到编辑器
3. 发布后图片会保存到 GitHub 仓库
4. 通过 CDN 访问

**如何修改头像**:
```tsx
// src/app/(home)/hi-card.tsx 第 33 行
<img src='/images/avatar.png' ... />
// 替换 public/images/avatar.png 文件
```

**如何修改艺术卡片图片**:
```json
// src/config/site-content.json 第 25-33 行
"artImages": [
  {
    "id": "cat",
    "url": "/images/art/cat.png"  // 修改此路径
  }
]
// 同时修改 currentArtImageId 来切换显示
```

### 6. 主题与样式系统

**文件路径**: `src/app/layout.tsx` (主题) + `src/styles/globals.css` (基础样式)

**主题配置** (`src/config/site-content.json`):
```json
"theme": {
  "colorBrand": "#35bfab",       // 品牌色 (按钮、重点)
  "colorPrimary": "#334f52",     // 主要文字
  "colorSecondary": "#7b888e",   // 次要文字
  "colorBrandSecondary": "#1fc9e7", // 辅助品牌色
  "colorBg": "#eeeeee",          // 背景色
  "colorBorder": "#ffffff",      // 边框色
  "colorCard": "#ffffff66",      // 卡片背景 (带透明度)
  "colorArticle": "#ffffffcc"    // 文章背景
}
```

**背景颜色配置**:
```json
"backgroundColors": [
  "#EDDD62", "#9EE7D1", "#84D68A",
  "#EDDD62", "#88E6E5", "#a7f3d0"
]
// 这些颜色用于动态背景的气泡动画
```

**如何只修改主题颜色** - **最简单的方式**:
1. 按 `Ctrl + L` 打开配置弹窗
2. 点击颜色选择器修改颜色
3. 点击保存

**CSS 变量系统**:
主题颜色会自动转换为 CSS 变量:
```css
:root {
  --color-brand: #35bfab;        /* 从 JS 注入到 style 属性 */
  --color-primary: #334f52;
  /* ... 其他变量 */
}
```

### 7. 认证与权限系统

**文件路径**: `src/hooks/use-auth.ts` + `src/lib/auth.ts`

**功能**:
- **private key 管理**: 用于 GitHub App 认证
- **会话缓存**: sessionStorage 存储
- **权限检查**: 401/403 错误处理
- **编辑权限**: 只有认证用户才能编辑/发布

**如何设置认证**:

**方式1: 通过 UI (推荐)**
1. 访问任意页面
2. 点击右上角"编辑"按钮
3. 在弹窗中输入 Private Key (Base64 格式)
4. 保存到浏览器缓存

**方式2: 修改配置**
```json
// src/config/site-content.json
{
  "isCachePem": true,  // 是否缓存私钥
  "hideEditButton": false  // 是否隐藏编辑按钮
}
```

**Private Key 格式**:
- 必须是 GitHub App 创建时下载的 `.pem` 文件内容
- 会自动转换为 Base64 存储
- 存储在 sessionStorage

### 8. 动画系统

**技术**: `Motion One` (轻量级动画库)

**主要动画**:
- **入场动画**: 卡片逐个淡入
- **拖拽动画**: 卡片拖拽时的平滑过渡
- **悬停动画**: 按钮/卡片 hover 效果
- **点击动画**: 按钮点击反馈

**配置位置**:
```tsx
// src/consts.ts
export const INIT_DELAY = 0.3      // 初始延迟
export const ANIMATION_DELAY = 0.1 // 卡片间延迟

// src/app/(home)/page.tsx
<motion.button
  whileHover={{ scale: 1.05 }}
  whileTap={{ scale: 0.95 }}
>
```

### 9. 插件与工具

**SVG 图标生成**:
```bash
npm run svg  # 自动生成 src/svgs/index.ts
```

**代码检查**:
```bash
npm run build  # Next.js 会自动检查类型和代码质量
```

**GH Actions / CI**:
- 项目配置了自动部署
- 提交到 main 分支会触发部署

---

## 四、常见修改场景指南

### 场景 1: 修改网站标题和描述

**修改位置**:
```json
// src/config/site-content.json
{
  "meta": {
    "title": "你的新标题",
    "description": "你的新描述",
    "username": "你的名字"
  }
}
```

**生效方式**: 重新部署

---

### 场景 2: 修改社交链接

**修改位置**:
```json
// src/config/site-content.json
{
  "socialButtons": [
    {
      "id": "github",
      "type": "github",  // 类型: github/juejin/xiaohongshu/tiktok/email
      "value": "https://github.com/你的用户名",
      "label": "",       // 空则显示默认图标
      "order": 1         // 排序
    },
    {
      "type": "email",
      "value": "your@email.com",
      "label": "",
      "order": 2
    }
  ]
}
```

**支持的类型**:
- `github` - GitHub 图标
- `juejin` - 掘金图标
- `xiaohongshu` - 小红书图标
- `tiktok` - 抖音图标
- `email` - 邮箱图标
- 其他 - 显示文字标签

---

### 场景 3: 隐藏某个首页卡片

**方法1: 配置文件**
```json
// src/config/card-styles.json
{
  "musicCard": {
    "enabled": false  // 设置为 false 隐藏
  }
}
```

**方法2: 配置弹窗**
1. 按 `Ctrl + L`
2. 取消勾选对应卡片
3. 保存

---

### 场景 4: 移除 Liquid Grass 动画

**修改文件**: `src/layout/index.tsx`

**删除以下两行代码** (第 4 行和第 54 行):
```tsx
// 第 4 行 - 删除导入
import BlurredBubblesBackground from './backgrounds/blurred-bubbles'

// 第 54 行 - 删除背景组件
<BlurredBubblesBackground colors={siteContent.backgroundColors} regenerateKey={regenerateKey} />
```

---

### 场景 5: 修改首页 Card 内容

**示例**: 修改 HiCard 的问候语

**文件**: `src/app/(home)/hi-card.tsx`

```tsx
function getGreeting() {
  const hour = new Date().getHours()

  // 可修改这里的问候语
  if (hour >= 6 && hour < 12) {
    return '早上好'  // 修改为中文
  } else if (hour >= 12 && hour < 18) {
    return '下午好'
  } else {
    return '晚上好'
  }
}

// 第 34-36 行 - 修改文本
<h1 className='font-averia mt-3 text-2xl'>
  {greeting} <br /> 我是 <span className='text-linear text-[32px]'>{username}</span>, 很高兴见到你!
</h1>
```

**其他 Card 修改位置**:
- `art-card.tsx` - 艺术卡片
- `clock-card.tsx` - 时钟卡片
- `share-card.tsx` - 分享卡片
- `social-buttons.tsx` - 社交按钮

---

### 场景 6: 自定义主题颜色

**方法1: 配置弹窗 (推荐)**
1. 按 `Ctrl + L`
2. 点击颜色选择器
3. 实时预览效果
4. 保存

**方法2: 配置文件**
```json
// src/config/site-content.json
{
  "theme": {
    "colorBrand": "#ff6b6b",        // 活力红
    "colorPrimary": "#2d3436",      // 深灰
    "colorBg": "#f8f9fa",           // 米白
    "colorCard": "#ffffff"          // 纯白
  },
  "backgroundColors": [
    "#ff6b6b", "#feca57", "#48dbfb", "#1dd1a1"  // 鲜艳背景色
  ]
}
```

**颜色建议**:
- 品牌色: 用于按钮、链接 (推荐亮色)
- 主色: 用于主要文字 (推荐深色)
- 背景色: 用于背景 (推荐浅色或白色)

---

### 场景 7: 添加新页面

**步骤**:

1. **创建页面文件**
```tsx
// src/app/my-new-page/page.tsx
'use client'

export default function MyNewPage() {
  return (
    <div className="container mx-auto px-4 py-20">
      <h1>我的新页面</h1>
      <p>这里写内容</p>
    </div>
  )
}
```

2. **添加导航链接** (可选)
   - 编辑 `src/components/nav-card.tsx` 添加新链接

3. **部署生效**

---

### 场景 8: 修改博客文章列表样式

**文件**: `src/app/blog/page.tsx`

**修改显示模式**:
```tsx
// 第 41 行
const [displayMode, setDisplayMode] = useState<DisplayMode>('year') // 改为 'month' 等
```

**修改文章卡片样式**:
- 可在 `page.tsx` 中搜索 `<motion.div` 修改渲染逻辑
- 或修改 `src/components/blog-preview.tsx`

---

### 场景 9: 管理文章分类

**方法1: 在写作时添加**
- 写作页面有分类输入框
- 输入新分类名自动创建

**方法2: 手动管理**
```tsx
// 分类信息存储在每篇文章的 front matter 中
---
title: "文章标题"
category: "分类名"  // 修改这里
tags: ["标签1", "标签2"]
---
```

**分类列表**: 会自动从所有文章中提取，无需单独配置

---

### 场景 10: 修改工具栏/按钮样式

**编辑器按钮**: `src/app/write/components/actions.tsx`
**社交按钮**: `src/app/(home)/social-buttons.tsx`
**导航卡片**: `src/components/nav-card.tsx`
**通用按钮**: 使用 Tailwind CSS 类修改

**示例**: 修改按钮圆角
```tsx
// 找到目标组件
<button className="... rounded-xl ...">  // 改为 rounded-2xl 等
```

---

### 场景 11: 修改图片上传路径

**默认路径**: `public/images/pictures/`

**修改上传逻辑**: `src/app/write/stores/write-store.ts`

**相关函数**:
```typescript
addFiles: (files: FileList) => Promise<void>
addUrlImage: (url: string) => void
```

**如果想自定义存储**:
1. 修改 `write-store.ts` 的文件处理逻辑
2. 修改 `src/lib/github-client.ts` 的文件上传 API

---

### 场景 12: 修改文章 URL 结构

**默认**: `/blog/[slug]`

**修改位置**: `src/app/blog/[id]/page.tsx` (动态路由)

**如改为 `/blog/[year]/[slug]`**:
```tsx
// 新增目录结构
src/app/blog/[year]/
  └── [slug]/
      └── page.tsx

// 修改链接生成逻辑 (在 blog page.tsx)
<Link href={`/blog/${year}/${slug}`}>...</Link>
```

---

### 场景 13: 禁用 RSS / Sitemap

**RSS**: 删除 `src/app/rss.xml/route.ts`

**Sitemap**:
```tsx
// src/app/sitemap.ts - 返回空数组
export default async function sitemap() {
  return []
}
```

---

### 场景 14: 修改缓读功能

**文件**: `src/hooks/use-read-articles.ts`

**缓存位置**: `localStorage` (浏览器存储)

**清除缓存**:
```tsx
// 在浏览器控制台执行
localStorage.removeItem('readArticles')
```

**修改缓存逻辑**:
```tsx
// use-read-articles.ts 第 17 行
const readArticles = JSON.parse(localStorage.getItem('readArticles') ?? '[]')
```

---

### 场景 15: 修改代码高亮主题

**文件**: `src/lib/markdown-renderer.ts`

**Shiki 配置**:
```typescript
const highlightedCode = shiki.codeToHtml(code, {
  lang: language,
  theme: 'github-dark'  // 修改主题: github-light, github-dark, material-theme 等
})
```

**可用主题**:
- `github-light`
- `github-dark`
- `material-theme`
- `monokai`
- `nord`
- ... 查看 Shiki 文档

---

### 场景 16: 修改响应式断点

**文件**: `src/hooks/use-size.ts`

**默认断点**:
```typescript
// use-size.ts 定义了 maxSM 判断逻辑
// 通常基于 window.innerWidth < 768

// 在组件中使用
const { maxSM } = useSize()

// 条件渲染
{!maxSM && <DesktopComponent />}
{maxSM && <MobileComponent />}
```

**修改断点**: 需要修改 `use-size.ts` 中的判断逻辑

---

### 场景 17: 添加评论系统

**推荐方案**: 使用现有服务
1. **Giscus** - 基于 GitHub Discussions
2. **Waline** - 隐私友好
3. **Valine** - 轻量级
4. **Disqus** - 功能丰富

**集成步骤**:
```tsx
// src/app/blog/[id]/page.tsx
import Comments from '@/components/comments'

export default function BlogPost({ params }) {
  return (
    <article>
      {/* 文章内容 */}
      <Comments slug={params.id} />
    </article>
  )
}
```

**自定义评论组件**: `src/components/comments.tsx`

---

### 场景 18: 修改构建和部署配置

**Next.js 配置**: `next.config.ts`
```typescript
const nextConfig: NextConfig = {
  // 可修改: 编译选项、插件等
}
```

**Cloudflare 配置**: `wrangler.toml`
```toml
name = "2025-blog"
compatibility_date = "2024-12-15"
```

**部署命令**:
```bash
# 本地开发
npm run dev

# 构建
npm run build

# Vercel 自动部署 (推荐)
# 推送代码到 GitHub，连接 Vercel 自动部署

# Cloudflare 手动部署
npm run build:cf
npm run deploy
```

---

### 场景 19: 修改 Open Graph / SEO

**文件**: `src/app/layout.tsx`

```typescript
export const metadata: Metadata = {
  title: '博客标题',
  description: '博客描述',
  openGraph: {
    title: '分享标题',
    description: '分享描述',
    images: ['/images/og-image.png']  // 分享图片
  },
  twitter: {
    title: 'Twitter 标题',
    description: 'Twitter 描述',
    card: 'summary_large_image'
  }
}
```

**单页面修改**:
```tsx
// 在页面内部定义
export const metadata: Metadata = {
  title: '特定页面标题'
}
```

---

### 场景 20: 故障排查

**问题1: 部署失败**
- 检查 `package.json` 依赖
- 检查 TypeScript 错误 (`npm run build`)
- 检查环境变量

**问题2: GitHub 连接失败**
- 检查 `GITHUB_CONFIG` 配置
- 确认 GitHub App 权限 (需要 repo write 权限)
- 检查 Private Key 格式
- 查看浏览器控制台错误

**问题3: 样式不生效**
- 检查 Tailwind 类名是否正确
- 清除浏览器缓存
- 检查 `tailwind.config` (本项目使用 v4，配置在 `postcss.config.mjs`)

**问题4: 图片不显示**
- 检查路径是否正确 (相对于 `public` 目录)
- 确认图片文件存在
- 检查大小写 (Linux 区分大小写)

**问题5: 编辑功能不起作用**
- 确认已登录 GitHub App
- 检查 Private Key 是否过期
- 查看网络请求是否成功

---

## 五、高级定制

### 1. 添加自定义动画
```tsx
// src/layout/backgrounds/ 添加新背景组件
// 在 src/layout/index.tsx 中导入使用
```

### 2. 自定义 Markdown 渲染
```tsx
// src/lib/markdown-renderer.ts
// 修改 renderer 配置
```

### 3. 添加 API 路由
```tsx
// src/app/api/ 目录
// 例如: src/app/api/likes/route.ts
```

### 4. 使用数据库 (替代 GitHub)
1. 替换 `src/lib/github-client.ts` 相关逻辑
2. 修改 `src/app/write/services/` 中的 API 调用
3. 添加数据库连接 (Prisma 等)

### 5. 国际化 (i18n)
1. 添加 `next-intl` 依赖
2. 创建 `locales` 目录存放翻译
3. 修改所有文本为 Key
4. 配置中间件

---

## 六、性能优化建议

### 1. 图片优化
- 使用 WebP 格式
- 压缩图片到 < 200KB
- 使用 Next.js Image 组件 (本项目使用原生 img 以简化)

### 2. 缓存策略
```tsx
// src/lib/github-client.ts
// 可添加 localStorage 缓存 API 响应
```

### 3. 懒加载
```tsx
// 大型组件使用 dynamic import
import dynamic from 'next/dynamic'
const HeavyComponent = dynamic(() => import('@/components/heavy'))
```

### 4. 减少 bundle 体积
- 移除未使用的依赖
- 按需导入组件
- 使用 `swc` (已配置)

### 5. SEO 优化
- 确保每个页面有唯一 title/description
- 添加结构化数据 (Schema.org)
- 生成 `robots.txt`

---

## 七、安全建议

### 1. Private Key 保护
- ❌ 不要上传到 GitHub
- ✅ 使用环境变量存储
- ✅ 使用 Vercel/Cloudflare Secrets

### 2. API 限流
- GitHub API 有速率限制 (60 次/小时)
- 项目已有简单错误处理
- 如需更多，可添加缓存层

### 3. XSS 防护
- 使用了 `html-react-parser`
- 项目已转义用户输入
- 自定义 Markdown 渲染器

---

## 八、常见问题 FAQ

**Q: 如何删除我的测试文章?**
A: 修改 `src/consts.ts` (文档说提供删除功能，需要在 UI 中找到)

**Q: 如何修改端口?**
A: 修改 `package.json` 的 `dev` 脚本: `next dev --turbopack -p 2025`

**Q: 如何修改博客数量显示?**
A: `src/app/blog/page.tsx` 中的搜索逻辑，或修改 `useBlogIndex` hook

**Q: 如何禁用缓读?**
A: 删除 `use-read-articles.ts` 中的 localStorage 逻辑

**Q: 如何添加音乐播放器?**
A: 已有 `music-card.tsx`，可修改为自定义播放列表

**Q: 如何修改 Cursor 图标?**
A: 替换 `public/images/cursor.svg` 文件

**Q: 如何添加 Page Transitions?**
A: 在 `src/app/layout.tsx` 的 `children` 外层添加 `<AnimatePresence>`

---

## 九、备份与迁移

### 备份配置
```
src/config/site-content.json
src/config/card-styles.json
src/consts.ts
```

### 迁移步骤
1. 备份上述配置文件
2. Fork 本项目
3. 修改配置为你的内容
4. 创建新的 GitHub App
5. 设置环境变量
6. 重新部署

---

## 十、社区与支持

- **文档**: https://www.yysuni.com/blog/readme
- **Issue**: GitHub Issues
- **QQ 群**: https://qm.qq.com/q/spdpenr4k2
- **微信群**: 查看 README 中的二维码

---

## 总结

这个博客项目的核心优势:
1. **无后端**: 纯前端 + GitHub 存储
2. **可视化编辑**: 拖拽布局 + 配置弹窗
3. **开箱即用**: 配置好 GitHub App 即可使用
4. **高度可定制**: 从主题到布局都可以修改代码
5. **数据安全**: 内容存储在你的 GitHub 仓库

**最关键的几个配置文件**:
- `src/consts.ts` - GitHub 配置
- `src/config/site-content.json` - 网站内容
- `src/config/card-styles.json` - 布局样式
- `package.json` - 依赖和脚本

**最常用的操作**:
- 按 `Ctrl + L` 打开配置
- 写作发布文章
- 拖拽调整布局
- 修改颜色主题

**遇到问题优先看**:
1. 本文件的对应章节
2. 浏览器控制台错误
3. GitHub Actions 日志
4. 项目 README.md

祝你使用愉快! 🚀