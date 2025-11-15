# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个基于WXT框架的浏览器扩展项目，核心功能是在用户点击扩展图标时动态注入内容脚本。项目使用TypeScript开发，支持Chrome和Firefox浏览器。

## 架构特点

- **动态内容脚本注入**: 使用`registration: "runtime"`模式，通过background script监听扩展图标点击事件，动态执行内容脚本
- **Shadow DOM UI**: 通过`createShadowRootUi`创建隔离的UI环境，避免与页面样式冲突
- **最小权限原则**: 仅请求`activeTab`和`scripting`权限，在用户交互时才获取当前标签页访问权限

## 项目结构

```
entrypoints/
├── background.ts          # 后台脚本，处理扩展图标点击和内容脚本注入
├── content/
│   ├── index.ts          # 内容脚本入口，创建Shadow DOM UI
│   └── style.css         # 内容脚本样式
utils/
└── matches.ts            # 定义内容脚本匹配的网址模式
```

## 常用开发命令

### 开发模式
```bash
# Chrome开发模式
pnpm dev

# Firefox开发模式
pnpm dev:firefox
```

### 构建命令
```bash
# 构建Chrome版本
pnpm build

# 构建Firefox版本
pnpm build:firefox
```

### 打包命令
```bash
# 打包Chrome扩展
pnpm zip

# 打包Firefox扩展
pnpm zip:firefox
```

### 类型检查
```bash
# TypeScript类型检查（不生成文件）
pnpm compile
```

## 关键配置说明

- **内容脚本匹配**: 在`utils/matches.ts`中定义，当前设置为`*://*.google.com/*`
- **Web可访问资源**: 在`wxt.config.ts`中配置CSS文件访问权限
- **启动URL**: 开发模式自动打开`https://google.com`进行测试

## 核心工作流程

1. 用户点击扩展图标
2. Background script监听点击事件，验证当前URL是否匹配
3. 使用`browser.scripting.executeScript`动态注入内容脚本
4. 内容脚本创建Shadow DOM UI并显示内容
5. 可选择返回结果给background script

## 扩展开发注意事项

- 修改`utils/matches.ts`中的URL模式来改变内容脚本生效的网站
- 内容脚本使用`cssInjectionMode: "ui"`确保样式注入到Shadow DOM中
- Background script会记录内容脚本的返回值到控制台
- 开发时需要重新加载扩展来应用background script的更改