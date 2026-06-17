# 腿替App - 跑腿服务平台

## 项目简介
腿替是一款专业的跑腿服务App，提供完整的需求发布、价格计算、实时追踪、信用管理等功能。

## 核心功能

### 1. 跑腿需求发布系统
- 多种需求类型选择（打印材料、取外卖、搬东西等）
- 详细信息填写（物品描述、取送地址、时间要求等）
- 需求确认与提交流程

### 2. 智能价格匹配系统
- 基于距离和时间的动态定价
- 详细价格明细展示
- 价格调整预览功能

### 3. 实时位置显示模块
- 类似外卖软件的双方距离实时显示
- 地图可视化功能
- 距离更新动画和倒计时

### 4. 品牌视觉识别系统
- 3套不同风格的Logo设计方案
- 完整的使用规范

### 5. 信用积分管理系统
- 信用分显示和评级
- 超时处罚规则展示
- 信用分不足时的功能限制

## 技术栈
- uni-app
- Vue 3
- TypeScript
- SCSS
- Vite

## 开发命令
```bash
# 安装依赖
npm install

# 开发模式
npm run dev:h5

# 构建H5
npm run build:h5

# 构建微信小程序
npm run build:mp-weixin

# 部署到GitHub Pages
npm run deploy:gh-pages
```

## 部署信息

### GitHub Pages
- 仓库地址: https://github.com/lhyzs-hub/foodorder
- 部署分支: gh-pages
- 访问地址: https://lhyzs-hub.github.io/foodorder

### 部署步骤
1. 构建项目: `npm run build:h5`
2. 复制静态资源到构建目录
3. 创建并切换到gh-pages分支
4. 提交并推送到远程仓库
5. GitHub Pages 自动部署

## 目录结构
```
.
├── src/                  # 源代码
│   ├── pages/           # 页面文件
│   ├── components/      # 组件
│   ├── styles/          # 样式文件
│   └── App.vue          # 根组件
├── static/              # 静态资源
│   └── icons/          # 图标文件
├── .github/             # GitHub配置
│   └── workflows/      # CI/CD工作流
└── dist/                # 构建输出
```

## 修复日志

### v1.0.1
- 修复了"打印材料"按钮无响应问题
- 为导航栏tabBar添加了对应的图标资源
- 配置GitHub Pages自动部署

## 许可证
MIT License
