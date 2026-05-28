# CLAUDE.md — Project guide for AI assistants

个人站,基于 [yaoyao-liu/minimal-light](https://github.com/yaoyao-liu/minimal-light) Jekyll 主题做了重度定制。视觉风格参考 [junliwang.tech](https://junliwang.tech/):白底、衬线字、单栏居中、顶部 sticky 导航。

## 部署

GitHub Pages,仓库名 `thesky30.github.io`。`git push origin main` 即触发自动构建,几分钟后线上更新。`CNAME` 文件在仓库根目录;若要绑自定义域名,把域名写入 `CNAME` 并在 `_config.yml` 把 `url` 改成对应域名。

## 目录结构

```
.
├── _config.yml            # 站点配置(标题、链接、导航项、SEO 等)
├── index.md               # 首页内容(About / Blog / Projects 三个区块)
├── blog.md                # /blog/ 列表页
├── _data/
│   ├── projects.yml       # Projects 区块数据源
│   ├── works.yml          # Models & Databases 区块数据源
│   └── categories.yml     # Blog 大/小分类体系(/blog/ 顶部 chip 的来源)
├── _posts/                # 博客文章目录,文件名格式: YYYY-MM-DD-title.md
├── _layouts/
│   ├── default.html       # 共享外壳:顶部 nav + 居中单栏 + 脚注
│   ├── homepage.html      # 首页(extends default):Hero + {{ content }}
│   └── post.html          # 博文详情页(extends default)
├── _includes/             # 主题原有的 publications.md / services.md(未使用)
├── _sass/
│   └── minimal-light.scss # 主样式,自定义 nav / hero / 列表样式在文件末尾
├── assets/
│   ├── css/               # 编译后的 CSS、字体定义
│   ├── img/               # avatar、favicon 等图片
│   ├── files/             # CV 等可下载文件
│   └── js/                # favicon 切换等小脚本
├── Gemfile                # Ruby gem 依赖(本地预览用 Jekyll 4.x)
└── CNAME                  # 自定义域名(空文件时使用 *.github.io)
```

## 常用操作

### 本地预览

```bash
bundle exec jekyll serve --livereload
# 浏览器打开 http://localhost:4000
```

如果新开终端跑这条报 `command not found: bundle`,先 `source ~/.zshrc`。

第一次拉到新机器:`bundle install` 安装依赖。

### 加一篇博客

在 `_posts/` 下新建文件,文件名必须是 `YYYY-MM-DD-slug.md`。frontmatter 模板:

```markdown
---
layout: post
title: "文章标题"
date: 2026-05-27
description: "一句话摘要(会出现在首页和列表页的预览里)"
category: 投研           # 大分类(单值),如 投研 / 量化 / AI / 游记
tags: [公司研究]          # 小分类列表,可多个;只在选中对应大分类时显示
mathjax: true            # 仅当文章需要数学公式时设为 true,否则可省略
---

正文 Markdown……
```

- `mathjax: true` 时该页会异步加载 MathJax;不需要数学的文章别开,省一点带宽。
- 代码块用三个反引号 + 语言名,Rouge 自动高亮。
- 图片放 `assets/img/posts/<post-slug>/xxx.png` 引用,保持目录整洁。

**两层 chip 过滤(`/blog/` 顶部)**
- **chip 体系由 `_data/categories.yml` 定义**(不是从文章反推)。要加/删/改大分类或小分类、调顺序,都改这个文件。文章数自动统计。
- 文章 frontmatter 里写 `category: 投研`(大分类,单值)+ `tags: [公司研究]`(小分类,列表)。
- 第一行是大分类 chip(深色按钮);**选中某个大分类后**,它在 categories.yml 里定义的小分类才会在第二行显示(带左侧细线缩进,圆角细线 chip)。
- 同名小分类要归到哪个大分类下,完全看 categories.yml 怎么写(例如把 `调研札记` 只写在 `投研` 的 subs 里,它就只属于投研)。
- 选大分类会自动重置小分类。再次点击当前激活的 chip = 取消选择。
- URL 用 hash 携带状态:`/blog/#cat=投研&sub=公司研究`,可分享、可前进/后退。
- 注意:HTML `hidden` 属性会被 `display:flex/grid` 覆盖,所以 SCSS 里有一条 `[hidden]{display:none!important}` 保证过滤能真正藏元素 —— 不要删。

### 加一个项目

编辑 `_data/projects.yml`,追加一个 `- title: ...` 块。字段:

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `title` | ✅ | 项目名 |
| `role` | ❌ | "你的角色 @ 团队",显示在标题下面,灰色小字 |
| `desc` | ❌ | 1-2 行描述 |
| `links` | ❌ | 列表,每项 `{ label, href }`。常见 label:Demo / Code / PDF / Blog |

顺序就是显示顺序。删项目直接删掉对应的块。

### 加一个模型 / 数据库

编辑 `_data/works.yml`,追加一个 `- title: ...` 块。字段:

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `title` | ✅ | 模型/数据库的名字。会自动链接到 `links` 数组的第一个 URL |
| `type` | ❌ | 右上小徽章文字,如 `Model` / `Database` / `Dataset` / `Tool` / `Benchmark`。留空则不显示 |
| `desc` | ❌ | 1-2 行描述(intro) |
| `links` | ❌ | 列表,每项 `{ label, href }`。**第一个会成为标题的主链接**,所有链接也都列在描述下方。常见 label:HuggingFace / Paper / Code / Site / Docs / Demo |

显示顺序 = YAML 中的顺序。删条目直接删块。

### 修改顶部导航栏

`_config.yml` 里的 `nav_links` 字段。每项 `{ label, href }`。href 支持:
- 站内锚点:`/#about` —— 在首页时直接平滑滚动,在其他页面时回到首页对应位置
- 站内路径:`/blog/`
- 外链:`https://...`

样式在 `_sass/minimal-light.scss` 文件末尾的 `.topnav` 相关规则。

### 修改个人信息(头像、链接、邮箱等)

`_config.yml` 顶部 `# Basic Information` 区块。所有带 `"TODO: ..."` 占位符的字段都需要替换。把不需要的社交链接字段设为空字符串 `""`,对应图标就不会显示。

**中文社交特殊字段**:
- `xiaohongshu`:小红书主页 URL。设了就在 hero 图标行显示一个红圆 `小`,点击外链
- `wechat_qr`:公众号 QR 码**图片路径**(如 `./assets/img/wechat_qr.png`),不是 URL。设了就显示微信图标,点击下方弹出 QR 浮层(再次点击或 Esc 关闭)。准备好 QR 截图(建议 ≥ 320×320 PNG)放到 `assets/img/` 即可

### 头像 / Favicon

- 头像:`assets/img/avatar.png`,建议 600x600 以上正方形,会被裁成圆形显示
- Favicon:`assets/img/favicon.png`(浅色)+ `favicon-dark.png`(深色),通常 192x192

## 设计约定

- 颜色:主色 `#043361`(深蓝),链接 hover 用 `#069`;深色模式用 `rgb(62, 183, 240)`。改色统一在 `_sass/minimal-light.scss` 改。
- 字体:Serif(衬线),由 `assets/css/font.css` 提供。换成无衬线把 `_config.yml` 的 `font` 改成 `"Sans Serif"`。
- 单栏宽度:`760px`。导航宽度 `820px`(略宽,显得不挤)。
- Sticky nav 高度 `56px`,所有锚点用 `scroll-margin-top` 偏移避免被遮。

## 本地与线上差异

- 本地用 Gemfile 里的 `jekyll ~> 4.3`(Ruby 4.x 兼容)
- GitHub Pages 用 `remote_theme` 拉主题,实际构建用的是 GH Pages 的 Jekyll 3.10。我们的自定义文件(layouts / sass / index.md / _posts)会覆盖远端主题的同名文件,所以两边渲染结果应一致。
- 如果未来某天主题更新破坏兼容性,可以把 `_config.yml` 里 `remote_theme` 那行删掉,让 Pages 用本地完整文件构建。
