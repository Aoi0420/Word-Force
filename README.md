# Word-Force 强制单词

> 没有花哨的记忆方法，只有强制的重复、死记硬背。

一款极简的记单词工具，纯前端单页应用，无需注册、无需后端、开箱即用。

## 特性

- **八级记忆体系 Lv.0~Lv.7** — 答对升级、答错降级，Lv.7 已掌握
- **四种题型** — 英选中、中选英、拼写、填空拼写，随机打乱
- **五种学习模式** — 普通 / 复习(Lv.5+) / 学习(Lv.3-) / 错题深渊 / 闪电战
- **☠️ 炼狱模式** — 6 选项 2×3 网格，全部计时 -2s，极致限时
- **⚔️ 单词 PK 模式** — 横屏双人分屏对战，独立抽词，限时竞速
- **预习机制** — 每关开始前 60s 浏览本关单词，可跳过
- **题型专项筛选** — 可单独练习某一种或几种题型
- **虚拟键盘** — 拼写模式自带 QWERTY 键盘，不触发手机输入法
- **音效反馈** — Web Audio API 生成，答对上行音、答错下行音
- **暗色军事风 UI** — 移动端适配、触摸优化
- **离线可用** — 单 HTML 文件 + 外部 CSS
- **二进制存储** — 3-bit 位图编码，10 万词仅 ~50KB

## 快速开始

### 方式一：GitHub Pages

1. Fork 或 push 到你的 GitHub 仓库
2. 在仓库 Settings → Pages 中开启，选择 `master` 分支 `/` 根目录
3. 访问 `https://<用户名>.github.io/Word-Force/`

### 方式二：本地直接打开

```bash
python3 -m http.server 8000
# 浏览器打开 http://localhost:8000
```

### 方式三：USB 搬运

适合被防火墙封锁的环境——单 HTML 文件 + CSS，U盘拷贝到任何电脑即可运行。

## 词库格式

导入 JSON 文件，格式如下：

```json
[
  {"en":"abandon","pos":"v","zh":"遗弃、放弃"},
  {"en":"ability","pos":"n","zh":"能力、才能"}
]
```

首次启动会自动加载项目根目录的 `words.json`（预设词库）。词库页面可切换其他内置词库（CET-4、CET-6、高考、中考），每个词库的学习进度独立保存。

## 数据存储

所有学习进度存储在浏览器 `localStorage` 中：

| 键 | 内容 |
|----|------|
| `wf_words_{id}` | 词库 JSON（按词库 ID 独立存储） |
| `wf_data_{id}` | 单词等级位图（Base64 编码） |
| `wf_meta_{id}` | 元数据（今日统计、耻辱榜） |
| `wf_settings` | 用户设置（含当前词库 ID） |

## 技术栈

纯 HTML + CSS + JavaScript (ES6)，零框架、零构建、零后端。

- 位图编码：3-bit/word，每个单词 8 级状态
- 音效：Web Audio API（OscillatorNode）
- 布局：Flexbox + CSS Grid，暗色主题
- 响应式：640px / 400px 两档断点
- CSS 已外置为独立文件（`style.css`）

## 项目结构

```
Word-Force/
├── index.html    # 应用主文件（HTML + JS）
├── style.css     # 样式表
├── words.json    # 默认词库（预设，基于中考词汇修改）
├── cet4.json     # CET-4 四级词库
├── cet6.json     # CET-6 六级词库
├── gaokao.json   # 高考词库
├── zhongkao.json # 中考词库
├── docs/
│   ├── 功能设计.md
│   └── 技术设计.md
└── .gitignore
```
