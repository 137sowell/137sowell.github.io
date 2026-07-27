# 个人网站

Hugo + [Blowfish](https://blowfish.page/) 主题，推送到 `main` 分支后由 GitHub Actions 自动构建并发布到
<https://137sowell.github.io/>。

## 日常使用

### 本地预览

```bash
hugo server -D
```

浏览器打开 <http://localhost:1313>，改完文件会自动刷新。`-D` 表示连草稿一起显示。

### 写一篇日志

在 `content/posts/` 下新建一个文件夹，里面放 `index.md`：

```
content/posts/2026-去了趟海边/
├── index.md
├── featured.jpg        ← 文件名带 feature/cover 的会被当作封面
└── snap-01.jpg         ← 正文里要插的照片
```

`index.md` 开头这几行是必须的：

```yaml
---
title: "标题"
date: 2026-07-27
tags: ["随笔"]
---
```

也可以用命令生成骨架：`hugo new content posts/我的新日志/index.md`

### 新建一个相册

在 `content/gallery/` 下新建文件夹，放 `index.md` 和照片，正文里写一行短代码即可：

```
{{< photos >}}
```

照片超过 4 张时会折叠成一叠（封面 + 后面露出两张的边 + 「展开 N」按钮），点一下铺开成 2×2 网格。

| 写法 | 效果 |
| --- | --- |
| `{{< photos >}}` | 本文件夹下所有图片，超过 4 张折叠成一叠 |
| `{{< photos fold="8" >}}` | 超过 8 张才折叠 |
| `{{< photos fold="2" >}}` | 超过 2 张就折叠 |
| `{{< photos fold="999" >}}` | 永不折叠，直接铺开 |
| `{{< photos match="trip-*.jpg" >}}` | 只取匹配的图片（正文里混着别的图时用） |
| `{{< photos cols="3" >}}` | 展开后每行 3 张（默认 2） |

展开后点任意一张看大图，左右方向键或手机滑动翻页，Esc 关闭。
这个短代码在 `layouts/shortcodes/photos.html`，卡片叠放的角度、模糊程度、格子大小都在里面的 `<style>` 里改。

照片直接放原图就行，Hugo 会自动生成缩略图并压缩，不用手动裁。

## 想改哪里

| 想改的东西 | 改哪个文件 |
| --- | --- |
| 站名、你的名字、简介、社交链接 | `config/_default/languages.zh-cn.toml` |
| 顶部菜单、页脚菜单 | `config/_default/menus.zh-cn.toml` |
| 首页大图、配色、深浅色、各种开关 | `config/_default/params.toml` |
| 首页那段介绍文字 | `content/_index.md` |
| 「关于」页 | `content/about/index.md` |
| 首页大图 / 头像图片 | 换掉 `assets/img/hero.jpg`、`assets/img/avatar.jpg` |

几个常用开关（都在 `params.toml`）：

- `defaultAppearance = "light"` — 默认亮色；`autoSwitchAppearance = false` 表示不跟随系统深色模式。
  右上角的月亮图标始终可以手动切换，选择会记在浏览器里
- `homepage.layout = "background"` — 首页大图固定在页面背后，内容滚过去时逐渐模糊（`layoutBackgroundBlur`）。
  换成 `hero` 就是图片装在顶部圆角卡片里，另外还有 `profile` / `card` / `page`
- `colorScheme` — 配色，可选 `blowfish` / `ocean` / `forest` / `autumn` / `noir` / `slate` 等
- `article.heroStyle` — 文章页封面样式，改成 `background` 就和首页一样是固定背景 + 滚动模糊

## 发布

```bash
git add -A && git commit -m "更新" && git push
```

推上去后去仓库的 Actions 页面看进度，一两分钟后网站就更新了。

> 首次部署需要在 GitHub 仓库 **Settings → Pages → Build and deployment → Source** 里选 **GitHub Actions**。

## 关于主题

主题是直接放在 `themes/blowfish/` 里的完整副本（不是子模块），所以构建不依赖网络，克隆下来就能跑。
要升级主题：去 <https://github.com/nunocoracao/blowfish> 下载新版，替换整个 `themes/blowfish/` 文件夹即可
（自己的配置和内容都在外面，不会被覆盖）。
