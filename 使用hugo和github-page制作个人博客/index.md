# 使用Hugo和GitHub Page制作个人博客


## 安装Hugo {#安装hugo}

[官方文档](https://gohugo.io/getting-started/quick-start/)， `ArchLinux` 系统为例中打开终端，输入命令

```shell
sudo pacman -S hugo
```


### 在 `doom-emacs` 中安装 `ox-hugo` {#在-doom-emacs-中安装-ox-hugo}

在 `doom` 配置文件 `init.el` 中, 取消 `org` 一行的注释, 在下面添加 hugo 支持即 可


#### ox-hugo 的使用与配置 {#ox-hugo-的使用与配置}

写一篇 `org` 文章的时候按快捷键 `C-c+C-e` 就会呼出交互式的导出选项了, 你也可以 `M-x org-export-dispatch`, 导出选项中有一 项是 `Export to Hugo-compatible Markdown`, 就是我们要用的.

这时候, 如果我们按 `H` 再按 `h` 即可将 `org` 文件导出为 `Markdown` 文件了.


## 初始化 `hugo` (创建一个新的网站) {#初始化-hugo--创建一个新的网站}

`Hugo` 提供了一个 `new` 命令来创建一个新的网站, 执行下面代码

```shell
hugo new site blog
cd blog
```

```shell
tree -L 1 ~/blog
```


## 安装hugo 主题(以 LoveIt 为例) {#安装hugo-主题--以-loveit-为例}

`LoveIt` 的主题仓库是: <https://github.com/dillonzq/LoveIt>

初始化项目目录为 `git` 仓库, 并且把主题仓库作为网站目录的子模块

```shell
git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```


## 创建文章 {#创建文章}

使用 `ox-hugo` 插件, 将 `org` 文档转换成 `Markdown` 文档

使用 `Emacs` 的模板工具 `yasnippet` 快速生成

```snippet
# -*- mode: snippet -*-
# name: hugo_blog
# key: <hugo
# --
#+OPTIONS: author:nil ^:{}
#+hugo_front_matter_format: yaml
#+HUGO_BASE_DIR: ../
#+HUGO_SECTION: posts/`(format-time-string "%Y/%m")`
#+DATE: `(format-time-string "[%Y-%m-%d %a %H:%M]")`
#+HUGO_CUSTOM_FRONT_MATTER: :toc true
#+HUGO_AUTO_SET_LASTMOD: t
#+HUGO_TAGS: $1
#+HUGO_CATEGORIES: $2
#+HUGO_DRAFT: false
#+TITLE: $3

$0
```

头部文件说明

-   OPTION: 这里我们在 `config.toml` 中如果已经配置了, 就可以把作者给关掉
-   HUGO_FRONT_MATTER_FORMAT: `ox-hugo` 将 `org` 文件中的头文件转换成 `Markdown` 文件的 头部参数时的指定格式, 这里我们指定的是 `YAML` 格式
-   HUGO_BASE_DIR: 我们博客的根目录的位置, 这里我们的位置是上一层目录 ../
-   HUGO_SECTION: 生成 `Markdown` 文件的位置, 这里的路径是相对博客根目录下的 `content` 文件夹的, 所以我们这里生成的 `Markdown` 文件就去了 `content/post/2022/04` 这个 文件夹下
-   HUGO_CUSTOM_FRONT_MATTER: `Markdown` 文件头部参数设置, 我们这里把目录 `toc` 打 开
-   HUGO_AUTO_SET_LASTMOD: 最后修改时间, `t` 表示自动生成
-   HUGO_TAGS/CATEGORIES: 标签和类别
-   HUGO_DRAFT: 默认情况下所有文章和页面均作为草稿创建, 我们让文章立即可以渲染


## 本地预览 {#本地预览}

本地启动网站:

```shell
hugo serve -D
```

如果渲染成功, 这时候命令行会返回一个预览网址, 复制到游览器打开即可预览你的文章.

一般为: <http://localhost:1313>


## FAQ {#faq}


### page not found {#page-not-found}

如果预览出现 `page not found` ,那应该是主题配置出错了，网上的大部分教程，都会告诉你把主题配置在 `config.toml` 文件，对比了官方文档才发现，应该是配置在 `hugo.toml` 文件里。

如果修改后仍出现该问题，建议按照官方文档操作一遍。

