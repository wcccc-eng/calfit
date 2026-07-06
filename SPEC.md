# CalFit 官网 Landing Page 开发说明书 v2.0

> **项目**: CalFit 产品官网  
> **版本**: v2.0  
> **日期**: 2026-07-02  
> **状态**: 已创建骨架 + 三平台分发架构，待 Codex 补充完整  

---

## 1. 项目概述

### 1.1 产品定位
CalFit 的产品官网。用户在搜索引擎搜索"CalFit / 热量追踪 App / AI 食物识别"等关键词时，能发现此页面并直接下载 APK 安装使用。

### 1.2 核心目标
- 🔍 **搜索引擎可发现**：SEO 优化，Google / 百度可收录
- 📱 **三平台覆盖**：Android APK + iOS PWA + HarmonyOS PWA，一个页面服务所有用户
- 🏠 **产品展示面**：一句话说清 CalFit 是什么、为什么选它
- 🌐 **国内可访问**：Gitee Pages 部署，国内 CDN 快速加载
- 💰 **零成本**：不买域名、不买服务器

### 1.3 目标用户
- 搜索"食物热量计算 App"的减脂增肌人群
- 从社交媒体/朋友分享链接进入的潜在用户
- Android 用户直接下载 APK 安装
- iPhone 用户通过 PWA 添加到主屏幕（或下载 IPA）
- 华为/鸿蒙用户通过 PWA 浏览器使用

---

## 2. 技术架构

### 2.1 技术栈

| 层级 | 技术 | 说明 |
|------|------|------|
| 页面 | 纯静态 **HTML5 + CSS3** | 单页 Hero，无需框架 |
| 样式 | 手写 CSS（复用 App 设计 Token） | 无构建步骤，零依赖 |
| 图标 | 内联 **SVG**（下载图标）+ 外部 **SVG**（Logo / QR） | 矢量无损 |
| SEO | meta tags + Open Graph + JSON-LD 结构化数据 | 搜索引擎友好 |
| 部署 | **Gitee Pages**（主）+ **GitHub Pages**（镜像） | 免费双通道 |
| 分析 | 可选：百度统计 / Plausible | V1 暂不加 |

### 2.2 与 App 项目的关系

```
E:\CalFit-App\
├── src/                    ← App 源码（React + TS）
├── android/                ← Capacitor Android 项目
├── public/                 ← App PWA 静态资源
│   ├── logo-mark.svg       ← App Logo（官网复用）
│   └── icons/icon.svg      ← PWA 图标
└── landing/                ← 🆕 官网（独立目录）
    ├── index.html           ← 唯一页面
    ├── calfit.css           ← 样式
    ├── logo-mark.svg        ← 从 public/ 复制
    ├── qr-code.svg          ← APK 下载二维码（待生成）
    └── README.md            ← 部署说明
```

> **官网和 App 共享**：Logo 文件、配色 Token、字体栈。官网不依赖 App 构建产物。

### 2.3 设计 Token（与 App 完全一致）

```css
:root {
  --ink:              #101111;    /* 主文字 / 主暗色 */
  --paper:            #fafaf7;    /* 主背景 */
  --accent:           #34c759;    /* 绿色强调 */
  --ember:            #ff9500;    /* 热量橙 */
  --danger:           #ff3b30;    /* 红色 */
  --text-secondary:   rgba(16, 17, 17, 0.55);
  --text-tertiary:    rgba(16, 17, 17, 0.35);
  --radius:           24px;       /* 大圆角 */
  --radius-sm:        16px;       /* 小圆角 */
  --font:             'Aptos', 'Segoe UI Variable',
                      'Microsoft YaHei UI', 'PingFang SC',
                      'Noto Sans SC', sans-serif;
}
```

---

## 3. 页面设计

### 3.1 当前状态（v1.0 已有）

v1.0 已完成 **Hero 首屏**，包含以下区块：

```
┌──────────────────────────────────────────────┐
│                                              │
│           🔥 (Logo 96×96 圆角阴影)           │
│                                              │
│        CalFit                                │  ← h1 粗黑大字
│        AI 热量追踪 · 拍照识别食物             │  ← 副标题
│                                              │
│   三行中文描述：                              │
│   拍照 → AI 识别 → 标注热量                  │
│   目标体重 → 公式计算 → 专属目标              │
│   本地存储 · 不注册 · 不卖数据                │
│                                              │
│   三行英文描述（同上英文版）                   │
│                                              │
│   ┌────────────────────────────┐             │
│   │  ⬇ 下载 Android APK        │  ← 主 CTA   │
│   └────────────────────────────┘             │
│   最新版本 · 8.2 MB · 免费使用                │
│                                              │
│   ┌───────────┐                              │
│   │  QR Code   │  ← 占位装饰 SVG             │
│   └───────────┘                              │
│   手机扫码直接下载                            │
│                                              │
│   也支持 PWA 在线使用 → 链接到 /              │
│                                              │
│   📸拍照识食  🎯千人千面  ⚡一键添加  🔒隐私优先│  ← 功能标签
│                                              │
├──────────────────────────────────────────────┤
│  GitHub ｜ Gitee ｜ 网盘下载                  │  ← Footer
│  © 2026 CalFit. 纯本地 · 不传数据 · 免费使用  │
└──────────────────────────────────────────────┘
```

### 3.2 Codex 需要做的事

#### 任务 A：替换占位链接（P0 必须）

| 位置 | 当前 | 要改成 |
|------|------|--------|
| `index.html` 第 65 行 `href="#"` | 占位符 `#` | APK 真实下载链接 |
| `index.html` 第 102 行 `href="https://github.com"` | 示例链接 | 真实 GitHub 仓库地址 |
| `index.html` 第 104 行 `href="https://gitee.com"` | 示例链接 | 真实 Gitee 仓库地址 |
| `index.html` 第 106 行 `href="#"` 网盘下载 | 占位符 `#` | 真实网盘链接（百度/阿里） |
| `index.html` 第 70 行版本号/大小 | `8.2 MB` | 从 APK 文件属性获取真实值 |

#### 任务 B：生成真实 QR 二维码（P0 必须）

当前 `div.qr-code` 内是一个**装饰性硬编码 SVG**（不代表任何真实链接）。

Codex 需要：
1. 用 APK 真实下载链接生成二维码
2. 输出为 SVG 文件 `landing/qr-code.svg`
3. 在 `index.html` 中替换为 `<img src="qr-code.svg" />`

推荐生成方式：
```bash
# 方式一：Node.js qrcode 库
npx qrcode -o landing/qr-code.svg -t svg "https://gitee.com/xxx/calfit/releases/download/v1.0/app-debug.apk"

# 方式二：Python qrcode 库
python -c "import qrcode; qrcode.make('https://...').save('landing/qr-code.svg')"
```

#### 任务 C：页面区块扩展（P1 建议）

Hero 首屏完成后，后续可补充以下区块（Codex 需在 `index.html` 中添加对应 HTML + CSS）：

##### C1. 功能亮点区
```
┌──────────────────────────────────────────────┐
│          为什么选择 CalFit？                   │
│                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ 📸       │  │ 🎯       │  │ 🔒       │   │
│  │ 拍照识食  │  │ 千人千面  │  │ 隐私优先  │   │
│  │ 虚线标注  │  │ 专属热量  │  │ 不传数据  │   │
│  └──────────┘  └──────────┘  └──────────┘   │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ ⚡       │  │ 🔍       │  │ 🔔       │   │
│  │ 一键添加  │  │ 搜索兜底  │  │ 饭点提醒  │   │
│  │ 批量确认  │  │ 手动输入  │  │ 不落一餐  │   │
│  └──────────┘  └──────────┘  └──────────┘   │
└──────────────────────────────────────────────┘
```

##### C2. 使用流程区
```
4 步开始：

① 填写档案  →  ② 拍照识别  →  ③ 确认添加  →  ④ 追踪进度
   身高体重       食物热量        一键写入        每日汇总
```

##### C3. 下载区（增强版）
```
┌─────────────────────────────────────────────┐
│          获取 CalFit                         │
│                                             │
│  [下载 Android APK]  [PWA 在线版]            │
│  二维码              备用：GitHub / 网盘      │
└─────────────────────────────────────────────┘
```

##### C4. FAQ 区
```
常见问题：

Q: 怎么安装 APK？
A: 下载后点击文件 → 允许"未知来源"安装 → 完成。

Q: 我的照片会上传到服务器吗？
A: 不会。CalFit 100% 本地处理。照片识别后立即删除，不保存不上传。

Q: 需要注册账号吗？
A: 不需要。打开即用，没有账号系统。

Q: 热量数据准确吗？
A: AI 识别提供参考值。你也可以手动调整份量和热量。
```

#### 任务 D：SEO 完善（P2 可选）

当前已有基础 SEO（meta description、OG、JSON-LD），可进一步补充：

- [ ] 多语言 `<link rel="alternate" hreflang="en" href="...">`
- [ ] `sitemap.xml`
- [ ] `robots.txt`
- [ ] 百度站长平台验证 meta 标签
- [ ] 结构化数据补充 `aggregateRating`、`operatingSystem` 版本

---

## 4. 部署架构

### 4.1 部署流程

```
开发者推送代码到 Gitee
        ↓
Gitee Pages 自动构建
        ↓
landing/index.html → https://你的用户名.gitee.io/calfit
        ↓
用户访问 → 点击下载 → APK 从 Gitee Release 拉取
```

### 4.2 双平台镜像

| 平台 | 地址 | 国内速度 | 用途 |
|------|------|----------|------|
| Gitee Pages | `用户名.gitee.io/calfit` | 🟢 快 | 主站 |
| GitHub Pages | `用户名.github.io/calfit` | 🟡 偶慢 | 备用镜像 |

### 4.3 APK 分发三通道

| 通道 | 实现 | 说明 |
|------|------|------|
| Gitee Release | 下载按钮主链接 | 直链，国内快 |
| 二维码 | 内嵌 QR SVG | 手机扫码即下 |
| 网盘 | Footer 备用链接 | 百度网盘/阿里云盘 |

---

## 5. 文件对照表

| 文件 | 状态 | 说明 |
|------|------|------|
| `landing/index.html` | ✅ 已创建 | 完整 Hero，需替换占位链接 |
| `landing/calfit.css` | ✅ 已创建 | 响应式，移动端 + 桌面端 |
| `landing/logo-mark.svg` | ✅ 已创建 | 从 `public/` 复制 |
| `landing/README.md` | ✅ 已创建 | 部署说明 |
| `landing/qr-code.svg` | ❌ 待生成 | 真实二维码 |
| `landing/sitemap.xml` | ❌ 待创建 | SEO |
| `landing/robots.txt` | ❌ 待创建 | SEO |

---

## 6. 响应式断点

| 断点 | 行为 |
|------|------|
| ≤ 480px | Logo 缩小 80px、按钮全宽、QR 缩小 120px、padding 收窄 |
| 481–767px | 默认样式 |
| ≥ 768px | Hero 最大宽度扩展到 580px |

---

## 7. 浏览器兼容

| 浏览器 | 最低版本 |
|--------|----------|
| Chrome | 90+ |
| Safari | 15+ |
| Firefox | 90+ |
| Edge | 90+ |
| 国内浏览器（UC/QQ/华为） | 近 3 年版本 |

---

## 8. 三平台分发架构 🔥 新增

### 8.1 总览

CalFit 基于 **一套代码（React + Capacitor + PWA）**，通过不同分发方式覆盖三个移动平台：

```
                ┌─────────────────────────────────┐
                │         CalFit 代码库            │
                │    React + Capacitor + PWA       │
                └──────────────┬──────────────────┘
                               │
           ┌───────────────────┼───────────────────┐
           ▼                   ▼                   ▼
     ┌──────────┐       ┌──────────┐       ┌──────────┐
     │ Android  │       │   iOS    │       │ HarmonyOS│
     └────┬─────┘       └────┬─────┘       └────┬─────┘
          │                  │                  │
    ┌─────┴─────┐     ┌──────┴──────┐     ┌────┴─────┐
    │ APK 直接装│     │ PWA (Safari)│     │ PWA 唯一 │
    │ 8.2 MB   │     │ 添加到主屏幕 │     │ 浏览器打开│
    │ 主推方式  │     │             │     │          │
    └───────────┘     │ IPA (需Mac) │     └──────────┘
                      └─────────────┘
```

### 8.2 各平台实现详情

#### Android — ✅ 已完工

| 项目 | 状态 | 说明 |
|------|------|------|
| 原生项目 | ✅ `android/` 已初始化 | Capacitor Android 项目 |
| APK 构建 | ✅ 可构建 | `npm run android:sync` → Android Studio → Build |
| 最新 APK | ✅ 已产出 | `app-debug.apk` (8.2 MB, 2026-07-02) |
| 摄像头 | ✅ | Capacitor Camera — 拍照 + 相册 |
| 本地通知 | ✅ | Capacitor Local Notifications — 饭点提醒 |
| 分发方式 | APK 直装 | 下载 → 允许"未知来源" → 安装 |

#### iOS — ⚠️ 代码就绪，缺构建环境

| 项目 | 状态 | 说明 |
|------|------|------|
| 依赖 | ✅ `@capacitor/ios: ^8.4.1` 已安装 | 与 core 同版本 |
| 原生项目 | ❌ `ios/` 未初始化 | 需要 macOS 执行 `npm run ios:add` |
| 脚本 | ✅ 已配置 | `ios:add` / `ios:sync` / `ios:open` |
| 代码兼容 | ✅ | `useCamera.ts` / `notifications.ts` 均兼容 iOS |
| **PWA 方案** | ✅ **主推** | Safari 打开 → 分享 → 添加到主屏幕，体验接近原生 |
| IPA 方案 | ❌ 需 Mac | 需要 Xcode + Apple ID 构建签名 |
| 摄像头 | ✅ PWA 降级 | `<input capture="environment">` / 文件选择器 |
| 本地通知 | ✅ PWA 降级 | Safari Web Notification API |
| 分发方式 | PWA（主推）+ IPA（可选） | |

**iOS 不买域名不买服务器的推荐路径**：
```
用户访问官网 → 看到 iOS 引导文案
  ↓
Safari 打开 calfit.gitee.io
  ↓
点击底部「分享」按钮
  ↓
「添加到主屏幕」
  ↓
桌面出现 CalFit 图标 → 点击即用（和 App 一样）
```

#### HarmonyOS — ✅ PWA 已覆盖

| 项目 | 状态 | 说明 |
|------|------|------|
| 原生项目 | ❌ 不支持 | Capacitor 无鸿蒙平台 |
| **PWA 方案** | ✅ **唯一路径** | 鸿蒙 ArkWeb 基于 Chromium，支持 SW + manifest |
| Service Worker | ✅ | 离线缓存 |
| Web App Manifest | ✅ | 可添加到桌面 |
| 摄像头 | ✅ | `<input capture="environment">` 调用相机 |
| 通知 | ⚠️ 有限 | 取决于鸿蒙系统版本对 Web Notification 的支持 |
| 分发方式 | 浏览器打开 PWA | |

**鸿蒙用户路径**：
```
用户访问官网 → 看到鸿蒙引导文案
  ↓
浏览器打开 calfit.gitee.io
  ↓
添加到桌面 / 书签
  ↓
每次打开即用
```

### 8.3 PWA 兜底能力

三个平台共同具备的 PWA 能力：

| 能力 | Android | iOS | HarmonyOS |
|------|---------|-----|-----------|
| 添加到主屏幕 | ✅ | ✅ | ✅ |
| 离线使用 | ✅ | ✅ | ✅ |
| 摄像头调用 | ✅ (原生) | ✅ (Safari capture) | ✅ (ArkWeb capture) |
| 本地通知 | ✅ (原生) | ✅ (Web Notification) | ⚠️ |
| IndexedDB | ✅ | ✅ | ✅ |
| 图片不保存 | ✅ | ✅ | ✅ |

### 8.4 官网需要展示的三平台入口

当前 `landing/index.html` 只展示了 Android 下载按钮。需要扩充为三平台引导：

```html
<!-- 下载区改为三列 -->
<div class="download-grid">
  <!-- Android -->
  <div class="platform-card">
    🤖 Android
    [下载 APK]
    或扫描二维码
  </div>

  <!-- iOS -->
  <div class="platform-card">
     iPhone
    Safari 打开 → 分享 → 添加到主屏幕
    [打开 PWA]
  </div>

  <!-- HarmonyOS -->
  <div class="platform-card">
    🧬 鸿蒙
    浏览器打开即用
    [打开 PWA]
  </div>
</div>
```

### 8.5 Codex 待做：官网三平台改造

| 任务 | 优先级 | 说明 |
|------|--------|------|
| 将下载区从单一 Android → 三平台卡片 | P0 | 修改 `index.html` 下载区块 |
| 新增 iOS 引导文案 | P0 | "Safari 打开 → 分享 → 添加到主屏幕" |
| 新增 HarmonyOS 引导文案 | P0 | "浏览器打开即用，支持添加到桌面" |
| FAQ 新增三平台安装说明 | P1 | 每个平台单独解答 |
| iOS IPA 构建文档 | P2 | 给有 Mac 的开发者参考 |

---

## 9. 后续迭代规划

| 版本 | 内容 |
|------|------|
| v2.0 | Hero 首屏 + Android APK 下载 + 三平台引导（当前） |
| v2.1 | 功能亮点区 + 使用流程区 |
| v2.2 | FAQ + 增强下载区（三平台安装说明） |
| v2.3 | i18n 中英切换 |
| v2.4 | 深色模式 |
| v3.0 | 用户评价 / 数据展示 / 博客 |

---

## 10. 验收标准

- [ ] 桌面端浏览器打开 `landing/index.html`，所有文字可读、Logo 显示正常
- [ ] 手机浏览器打开，布局适配、按钮可点击
- [ ] 三平台下载区清晰展示（Android / iOS / HarmonyOS）
- [ ] 点击 Android 下载按钮 → 触发 APK 文件下载
- [ ] 扫描二维码 → 跳转到下载页面或直接下载
- [ ] iOS 引导："Safari 打开 → 分享 → 添加到主屏幕" 可见
- [ ] HarmonyOS 引导："浏览器打开即用" 可见
- [ ] Google / 百度搜索 "CalFit AI 热量追踪" 能搜到页面
- [ ] Gitee Pages 部署后手机访问正常
- [ ] Footer 三个链接均可达（GitHub / Gitee / 网盘）
- [ ] 无 404 资源（CSS、Logo SVG 均正确加载）
- [ ] Android PWA 可添加到主屏幕并离线使用
- [ ] iOS Safari PWA 可添加到主屏幕
- [ ] HarmonyOS 浏览器可正常打开 PWA 使用
