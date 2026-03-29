# 项目 AI 知识基础

## 项目简介

本项目是一个基于 Jekyll 静态网站生成器的个人博客网站，使用 Chirpy 主题。网站包含多个类别的内容，包括 AI 技术、技术哲学、代码重构等主题的文章。

## 技术栈

- **静态网站生成器**: Jekyll
- **前端技术**:
  - HTML5
  - CSS3/SCSS
  - JavaScript
  - Rollup (JavaScript 打包工具)
- **构建工具**:
  - Node.js
  - npm
- **版本控制**: Git
- **主题**: Chirpy

## 目录结构

```
├── _posts/             # 博客文章目录
│   ├── AI/             # AI 相关文章
│   ├── philosophy/     # 哲学相关文章
│   └── refactoring/    # 代码重构相关文章
├── _layouts/           # 页面布局模板
├── _includes/          # 可重用的页面组件
├── _data/              # 数据文件
│   ├── locales/        # 多语言支持
│   └── origin/         # 原始数据
├── _tabs/              # 导航标签页
├── assets/             # 静态资源
│   ├── css/            # 样式文件
│   ├── js/             # JavaScript 文件
│   │   ├── dist/       # 打包后的 JS 文件
│   │   └── data/       # 数据文件
│   ├── img/            # 图片文件
│   └── example/        # 示例代码
├── docs/               # 文档目录
├── tools/              # 工具脚本
├── _config.yml         # Jekyll 配置文件
├── index.html          # 网站首页
├── package.json        # npm 配置文件
└── README.md           # 项目说明文件
```

## 主要功能

- 响应式设计，支持多种设备
- 多语言支持
- 文章分类和标签系统
- 搜索功能
- 代码高亮显示
- 社交媒体分享
- 评论系统集成
- PWA 支持

## 内容分类

- **AI 技术**: 人工智能相关的技术文章和教程
- **技术哲学**: 关于技术发展和应用的思考
- **代码重构**: 代码质量改进和重构技巧

## 开发流程

1. 安装依赖: `npm install`
2. 本地开发: `npm run dev` 或 `bundle exec jekyll serve`
3. 构建生产版本: `npm run build` 或 `bundle exec jekyll build`

## 部署方式

项目可部署到 GitHub Pages、Netlify、Vercel 等静态网站托管服务。