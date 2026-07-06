# CalFit 官网部署说明

## 文件清单

```
landing/
├── index.html        ← 官网首页（Hero 首屏）
├── calfit.css        ← 响应式样式（复用 CalFit 设计 Token）
├── logo-mark.svg     ← App Logo（火焰 C 字标）
├── qr-code.svg       ← (待生成) APK 下载二维码
└── README.md         ← 本文件
```

## 部署步骤

### 1. 上传 APK 到 Gitee Release

1. 在 Gitee 创建 `calfit` 仓库（需开源以启用 Pages）
2. 仓库设置 → Releases → 创建标签 → 上传 `android/app/build/outputs/apk/debug/app-debug.apk`
3. 复制 APK 直链

### 2. 更新下载链接

编辑 `index.html`，找到：

```html
<a class="btn-primary" href="#" id="download-link">
```

将 `href="#"` 替换为 Gitee Release APK 直链。例如：

```html
<a class="btn-primary" href="https://gitee.com/你的用户名/calfit/releases/download/v1.0/app-debug.apk">
```

### 3. 生成二维码

用 Gitee Release APK 直链生成二维码 SVG，替换 `index.html` 中 `div.qr-code` 内的 SVG。

在线生成工具：https://qr-code.io 或本地 `qrcode` 命令行。

### 4. 开启 Gitee Pages

1. Gitee 仓库 → 服务 → Gitee Pages
2. 部署目录选择 `landing`
3. 点击「更新」
4. 等几分钟后访问 `https://你的用户名.gitee.io/calfit`

### 5. GitHub Pages 镜像（可选）

1. GitHub 创建 `calfit` 仓库
2. Settings → Pages → Source: GitHub Actions 或 Deploy from branch (`landing/` 目录)
3. 访问 `https://你的用户名.github.io/calfit`

## 部署架构

```
用户访问 calfit.gitee.io
        ↓
   Gitee Pages (国内 CDN)
        ↓
   landing/index.html
        ├── APK 下载 → Gitee Release / 网盘直链
        ├── 二维码 → 同 Gitee Release 链接
        └── PWA → calfit.gitee.io/app (CalFit PWA)
```

## 待补充

- [ ] 替换 `footer` 中 `github.com` 和 `gitee.com` 为实际仓库地址
- [ ] 替换 `footer` 中「网盘下载」为实际网盘链接
- [ ] 上传 APK 后更新 `.apk-meta` 中的版本号和大小
- [ ] 后续可扩展：功能亮点区、使用流程图、FAQ、用户评价
