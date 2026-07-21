# Word-Force 强制单词

> 没有花哨的记忆方法，只有强制的重复、死记硬背。

[![Build Android APK](https://github.com/Aoi0420/Word-Force/actions/workflows/build-apk.yml/badge.svg)](https://github.com/Aoi0420/Word-Force/actions/workflows/build-apk.yml)

一款极简的记单词工具，纯前端单页应用，无需注册、无需后端、开箱即用。

在线使用：[记单词.遥遥领先.org](http://记单词.遥遥领先.org/)　|　下载 APK：[最新安装包](https://github.com/Aoi0420/Word-Force/releases/latest)

---

## 快速开始

### GitHub Pages

Fork 本仓库，在 Settings → Pages 中开启，选择 `master` 分支 `/` 根目录，访问 `https://<用户名>.github.io/Word-Force/`。

### 本地运行

```bash
python3 -m http.server 8000
# 浏览器打开 http://localhost:8000
```

### Android APK

自行构建：

```bash
git tag v0.1.0
git push origin v0.1.0
```

GitHub Actions 自动构建并发布 Release。也可在 Actions 页面手动触发构建（仅生成 Artifact）。

### USB 搬运

单 HTML 文件 + CSS，U 盘拷贝到任何电脑即可运行，无需网络、无需安装。

---

## 功能

- **八级记忆体系**：答对升级、答错降级，Lv.7 标记已掌握
- **四种题型**：英选中、中选英、拼写、填空拼写，随机打乱
- **五种学习模式**：普通 / 复习 / 学习 / 错题深渊 / 闪电战
- **炼狱模式**：6 选项 2x3 网格，限时答题，极致压迫
- **单词 PK 模式**：横屏双人分屏对战，独立抽词，限时竞速
- **预习机制**：每关前 60 秒浏览本关单词
- **题型专项筛选**：单独练习某一种或几种题型
- **虚拟键盘**：拼写模式自带 QWERTY 键盘，不触发手机输入法
- **音效反馈**：答对答错不同音高提示
- **离线可用**：不依赖网络，打开即用
- **多词库**：内置 CET-4/6、高考、中考词库，支持导入自定义 JSON 词库

---

## 开发者

### 技术栈

纯 HTML + CSS + JavaScript (ES6)，零框架、零构建、零后端、零依赖（运行时除外）。

| 技术 | 用途 |
|------|------|
| Web Audio API (OscillatorNode) | 音效生成，无音频文件 |
| Flexbox + CSS Grid | 布局，暗色主题，移动端适配 |
| 640px / 400px 两档断点 | 响应式 |
| localStorage | 数据持久化 |
| JSZip + sql.js (CDN) | Anki 词库导入 |

### 数据存储

所有学习进度存储在浏览器 `localStorage`：

| 键 | 内容 |
|----|------|
| `wf_words_{id}` | 词库 JSON（按词库 ID 独立存储） |
| `wf_data_{id}` | 单词等级位图（3-bit/word，Base64 编码） |
| `wf_meta_{id}` | 元数据（今日统计、耻辱榜） |
| `wf_settings` | 用户设置（含当前词库 ID） |

存储效率：3-bit 位图编码，每个单词 8 级状态（Lv.0–Lv.7），10 万词仅约 50KB。

### 词库格式

```json
[
  {"en":"abandon","pos":"v","zh":"遗弃、放弃"},
  {"en":"ability","pos":"n","zh":"能力、才能"}
]
```

首次启动自动加载 `words.json`。导入 JSON 时提示命名并创建为独立词库（默认 `词库-YY/MM/DD`），不会覆盖当前词库。

### 项目结构

```
Word-Force/
├── index.html              # 应用主文件（HTML + 完整 JS 引擎）
├── style.css               # 样式表
├── words.json              # 默认词库（基于中考词汇）
├── cet4.json / cet6.json   # CET 词库
├── gaokao.json             # 高考词库
├── zhongkao.json           # 中考词库
├── .github/
│   └── workflows/
│       └── build-apk.yml   # CI/CD: push tag 自动构建 APK
├── docs/
│   ├── 功能设计.md
│   └── 技术设计.md
└── .gitignore
```

### CI/CD 构建

工作流 `.github/workflows/build-apk.yml` 在 push tag `v*` 时自动执行：

1. 在关于页面注入版本号（tag 名）和构建日期（北京时间）
2. 通过 Capacitor 将 Web 应用初始化为 Android 原生项目
3. Gradle 构建 debug APK（universal 包）
4. 解析 tag 间 conventional commits 生成 Release Notes，按 feat/fix/refactor 分类
5. 创建 GitHub Release 并上传 APK

手动触发 (`workflow_dispatch`) 仅构建并上传 Artifact，不创建 Release。
