# thesky30.github.io

个人站点 + 博客。基于 [yaoyao-liu/minimal-light](https://github.com/yaoyao-liu/minimal-light) Jekyll 主题深度定制,视觉风格参考 [junliwang.tech](https://junliwang.tech/)。

## 本地开发

需要 Ruby 3.1+(推荐用 Homebrew 安装的版本,不要用 macOS 系统自带的 2.6):

```bash
bundle install                       # 首次拉项目时跑一次
bundle exec jekyll serve --livereload
# 打开 http://localhost:4000
```

## 部署

托管在 GitHub Pages,`git push` 到 `main` 即自动构建发布。无需手动操作。

## 怎么改

- **个人信息 / 头像 / 社交链接**:编辑 `_config.yml`
- **首页文案(About / Blog / Projects)**:编辑 `index.md`
- **加博客**:在 `_posts/` 下新建 `YYYY-MM-DD-title.md`
- **加项目**:编辑 `_data/projects.yml`
- **顶部导航栏**:`_config.yml` 的 `nav_links`

更多细节见 [`CLAUDE.md`](./CLAUDE.md)。

## License

主题部分继承自 [Minimal Light](https://github.com/yaoyao-liu/minimal-light) 的 MIT 协议;站内文章 / 项目数据等内容版权归我所有。
