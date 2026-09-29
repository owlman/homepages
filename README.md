# MyHome — www.owlman.cn

> 凌杰（网名 owlman）的个人主页 —— 技术作家、译者与 AI Agent 实践者。
> 汇聚博客、出版作品、社交账号等外链，作为个人网络存在的统一入口。

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)
[![GitHub Pages](https://img.shields.io/badge/deploy-GitHub%20Pages-222222?logo=githubpages)](https://www.owlman.cn/)
[![Tech](https://img.shields.io/badge/stack-HTML%20%2B%20CSS%20%2B%20JS-orange)](#技术栈)
[![No build step](https://img.shields.io/badge/build-none-success)](#快速开始)

> 最后更新：2026-09-29

## 预览

线上版本：[https://www.owlman.cn](https://www.owlman.cn)

![首页预览](img/og-image.png)

## 特色

- **文人风格**：标题用 Noto Serif SC 衬线字体、正文用黑体，强调"书卷气"
- **深色模式为默认**：首次访问即深色，可切换主题并写入本地存储
- **响应式布局**：桌面端主-侧栏，移动端单列
- **完整 SEO**：OG / Twitter Card / JSON-LD（1 Person + 15 Books + BlogPosting）
- **零构建步骤**：纯静态 HTML + CSS + 原生 JavaScript，没有 webpack / vite / 构建工具链
- **离线可运行**：Bootstrap、字体、图标全部本地化，无任何外部 CDN 依赖
- **PWA 支持**：`site.webmanifest` + 多尺寸 favicon / apple-touch-icon
- **博客一键同步**：`python tools/fetch_posts.py` 从博客园首页抓取文章列表

## 技术栈

- **HTML5 + CSS3 + 原生 JavaScript**（无构建步骤）
- **Bootstrap 5.3.3**（本地化到 `vendor/bootstrap/`）
- **Noto Serif SC** 衬线字体（子集化本地 woff2，覆盖 633 个 CJK + 52 个 Latin 字符，存于 `vendor/fonts/`）
- **内联 SVG sprite**（无图标字体）
- **GitHub Pages** + `CNAME` 自定义域名 `www.owlman.cn`

## 目录结构

```
homepages/
├── index.htm              # 主页面（head + SVG sprite + 主体）
├── books.json             # 15 部作品（5 原创 + 10 翻译）
├── posts.json             # 10 篇博客文章
├── feed.xml               # Atom feed（由 posts.json 生成）
├── sitemap.xml            # 站点地图（由 books + posts 生成）
├── site.webmanifest       # PWA 配置
├── robots.txt
├── CNAME                  # www.owlman.cn
├── LICENSE                # MIT
├── README.md              # 本文件
├── MAINTENANCE.md         # 详细维护文档（设计约定、易踩坑、数据获取等）
├── css/
│   └── style.css          # 全部自定义样式（含深色模式覆写）
├── js/
│   └── main.js            # 主题切换 / 数据加载 / 滚动动画 / Tab 路由
├── data/
│   └── social-links.json  # 社交图标数据（驱动 Hero + 联系区两处展示）
├── img/
│   ├── me.png             # 头像
│   ├── og-image.png       # OG / README 预览图
│   ├── icon-192.png       # PWA 图标
│   ├── icon-512.png       # PWA 图标
│   ├── apple-touch-icon.png
│   ├── favicon-16.png / favicon-32.png
│   └── books/             # 15 本封面（WebP，width 400px）
├── vendor/
│   ├── bootstrap/         # bootstrap.min.css + bootstrap.bundle.min.js
│   └── fonts/             # noto-serif-sc-{400,700}.woff2
└── tools/
    ├── fetch_posts.py     # 抓博客园 → posts.json
    ├── play_owlman.py     # 综合校验（4 步）
    ├── build_feed.py      # 生成 feed.xml
    ├── build_sitemap.py   # 生成 sitemap.xml
    ├── build_jsonld.py    # 生成 JSON-LD Books 片段
    ├── subset_font.py     # 下载 + 子集化 Noto Serif SC
    ├── check.sh           # 预提交校验
    └── install-hook.sh    # 安装 pre-push git 钩子
```

## 快速开始

### 本地预览

```bash
# 方式 1：直接打开
start index.htm           # Windows
open index.htm            # macOS

# 方式 2：起一个静态服务（推荐，便于测试相对路径 / AJAX / Fetch）
python -m http.server 8000
# 然后访问 http://localhost:8000
```

### 开发依赖（可选）

```bash
pip install -r requirements-dev.txt    # 装 jsonschema
pip install requests                   # fetch_posts.py 依赖
```

### 安装 pre-push 校验钩子

clone 出新工作区后手动安装，让 `git push` 前自动跑 schema / 链接校验：

```bash
bash tools/install-hook.sh
```

## 内容维护

| 任务 | 入口 |
| --- | --- |
| 同步博客园最新文章 | `python tools/fetch_posts.py` |
| 新增 / 修改一本书 | 编辑 `books.json` + 封面 WebP + `index.htm` JSON-LD |
| 修改个人简介 / 技能 / 荣誉 | 直接编辑 `index.htm` 的 `#about` section |
| 修改社交链接 | 编辑 `data/social-links.json` |
| 修改联系邮箱 | 编辑 `index.htm` 联系区文案 |
| 重建 feed.xml | `python tools/build_feed.py` |
| 重建 sitemap.xml | `python tools/build_sitemap.py` |

字段语义、Tab 归属、易踩坑（豆瓣图床防盗链 / 深色模式默认 / 社交顺序等）详见 **[MAINTENANCE.md](./MAINTENANCE.md)**。

## 工具脚本

| 脚本 | 作用 |
| --- | --- |
| `tools/fetch_posts.py` | 从博客园 `cnblogs.com/owlman/` 抓首页 10 篇，写 `posts.json` |
| `tools/play_owlman.py` | 综合校验：JSON Schema / 文件存在性 / sitemap 一致性 / 线上 Playwright 抓取 |
| `tools/build_feed.py` | 从 `posts.json` 生成 `feed.xml`（Atom） |
| `tools/build_sitemap.py` | 从 `books.json` + `posts.json` 合成 `sitemap.xml` |
| `tools/build_jsonld.py` | 从 `books.json` 生成 `index.htm` 的 Books JSON-LD 片段 |
| `tools/subset_font.py` | 下载并子集化 Noto Serif SC（woff2） |
| `tools/check.sh` | 预提交校验（schema + 文件存在性 + 一致性） |
| `tools/install-hook.sh` | 安装 pre-push 钩子（调用 `check.sh`） |

常用命令：

```bash
python tools/fetch_posts.py                    # 抓取首页 10 篇，直接覆盖 posts.json
python tools/fetch_posts.py --pages 2          # 抓前 2 页（按发布时间并集去重）
python tools/fetch_posts.py --summary          # 同时抓取每篇正文摘要（较慢，~10-15s）
python tools/fetch_posts.py --dry-run          # 只打印 JSON 到 stdout，不写文件

python tools/play_owlman.py --fetch            # 先 fetch_posts 再做综合校验
python tools/play_owlman.py --skip-live        # 只跑本地校验，不开浏览器

python tools/subset_font.py --dry-run          # 只看字符统计，不下载
```

## 部署

- GitHub Pages（`gh-pages` 分支），`git push` 后自动部署
- 自定义域名 `www.owlman.cn`（通过 `CNAME` 文件绑定，不要删除）

```bash
git push origin gh-pages
```

## 设计约定（简版）

| 区块 | 字体 / 字号 |
| --- | --- |
| 区块标题 h2（`content-heading`） | 宋体 1.5rem |
| 子标题 h3/h4（`sub-heading`） | 宋体 1.1rem |
| 正文 | 黑体，行高 1.8，`max-width: 70ch` 限宽 |

**颜色变量**（`css/style.css` 的 `:root`，深色 `[data-bs-theme="dark"]` 覆写）：

- `--tw-bg` / `--tw-surface`：背景
- `--tw-text` / `--tw-muted`：正文 / 弱化
- `--tw-accent` / `--tw-accent-light`：强调色（棕色系）
- `--tw-heading`：标题色

**交互**：入场动画、Hero 错峰、主题切换 0.4s 过渡、阅读进度条、回到顶部、Tab hash 路由。

完整设计规范与配色变量见 **[MAINTENANCE.md](./MAINTENANCE.md#关键设计约定)**。

## 已知限制

- **Bootstrap 227 KB 未裁剪**：曾尝试 PurgeCSS，但 Windows 路径兼容性问题放弃；浏览器缓存后影响不大
- **字体会随内容漂移**：新增文章或修改 HTML 引入新 CJK 字符时，需重跑 `tools/subset_font.py` 更新字体
- **feed.xml / sitemap.xml 需手动重建**：抓取文章或新增书籍后，分别跑 `build_feed.py` / `build_sitemap.py`
- **文章摘要需额外请求**：`fetch_posts.py --summary` 会逐篇访问博客页面，10 篇约 10-15 秒

## 协议

本项目以 [MIT License](./LICENSE) 协议开源 —— Copyright (c) 2019-2026 owlman。

使用项目代码（包括样式、布局、脚本）时请保留版权声明。

## 联系

- E-mail: [jie.owl2008@gmail.com](mailto:jie.owl2008@gmail.com)
- 微博: [@凌杰](https://weibo.com/owlman)
- X (Twitter): [@lingjieowl](https://twitter.com/lingjieowl)
- 博客园: [owlman](https://www.cnblogs.com/owlman)
- GitHub: [owlman](https://github.com/owlman)
- 码云: [owlman](https://gitee.com/owlman)
- 豆瓣: [owlman](https://www.douban.com/people/owlman/)
- Facebook: [jie.owlman](https://www.facebook.com/jie.owlman)

## 致谢

- [Bootstrap 5](https://getbootstrap.com/) — MIT
- [Simple Icons](https://simpleicons.org/) — CC0
- [Noto Serif SC](https://fonts.google.com/noto/specimen/Noto+Serif+SC) — SIL OFL 1.1
- [博客园](https://www.cnblogs.com/) — 内容来源