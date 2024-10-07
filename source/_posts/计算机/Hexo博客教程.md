---
title: Hexo博客教程
date: 2022-04-18 15:57:37
img: https://cdn.pixabay.com/photo/2022/03/29/21/04/eggs-7100211_960_720.jpg # 该文章图片
excerpt: 在windows下利用Hexo对博客系统的建立，并利用Github进行云托管，并设置了一种主题供参考 # 文章描述（摘要）
top: false #是否该篇文章显示在首页的置顶栏目中
toc: true # 将该值设为false，则该篇文章不显示右侧目录
tocOpen: true #将该值设为false，则该篇文章右侧目录默认收缩
onlyTitle: false #文章详情页头部是否只显示标题，不显示日期等信息
comments: true #将该值设为false，则该篇文章不显示评论
share: true #将该值设为false，则该篇文章不显示分享按钮
copyright: true # 将该值设为false，则该篇文章不显示版权声明
donate:  false #将该值设为false，则该篇文章不显示打赏按钮
prismjs: default #default, coy, dark, funky, okaidia, solarizedlight, tomorrow, twilight
categories: 计算机
tags: [Hexo, 博客, 前端] #一篇文章可以多个标签
mathjax:  true #mathjax公式
---

# Hexo 博客教程

## 一、Windows本地安装Hexo

官网教程：<https://hexo.io/zh-cn/docs/>

首先需要先在电脑上安装[Node.js](https://nodejs.org/zh-cn/)，[Git](https://git-scm.com/)

### 安装Hexo

```bash
> npm install -g hexo-cli
```

### 使用Hexo命令

1. `npx hexo  <command>`
2. 将 Hexo 所在的目录下的` node_modules `添加到环境变量之中即可直接使用`hexo <command>`

### 初始化文件夹

```bash
> npx hexo init <folder>
> cd <folder>
> npm install
```

**_config.yml**
网站的 [配置](https://hexo.io/zh-cn/docs/configuration) 信息，您可以在此配置大部分的参数。

**package.json**
应用程序的信息。

**scaffolds**
[模版](https://hexo.io/zh-cn/docs/writing) 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

**source**
资源文件夹是存放用户资源的地方。

**themes**
[主题](https://hexo.io/zh-cn/docs/themes) 文件夹。Hexo 会根据主题来生成静态页面。

### Hexo新建文章

```bash
> npx hexo new [layout] <title>
```

新建一篇文章。如果没有设置 `layout` 的话，默认使用 [\_config.yml](https://hexo.io/zh-cn/docs/configuration) 中的 `default_layout` 参数代替。如果标题包含空格的话，请使用引号括起来。

```bash
> npx hexo new "post title with whitespace"
```

| 参数              | 描述                                          |
| ----------------- | --------------------------------------------- |
| `-p`, `--path`    | 自定义新文章的路径                            |
| `-r`, `--replace` | 如果存在同名文章，将其替换                    |
| `-s`, `--slug`    | 文章的 Slug，作为新文章的文件名和发布后的 URL |

默认情况下，Hexo 会使用文章的标题来决定文章文件的路径。对于独立页面来说，Hexo 会创建一个以标题为名字的目录，并在目录中放置一个 `index.md` 文件。你可以使用 `--path` 参数来覆盖上述行为、自行决定文件的目录：

```bash
> npx hexo new page --path about/me "About me"
```

以上命令会创建一个 `source/about/me.md` 文件，同时 Front Matter 中的 title 为 `"About me"`

注意！title 是必须指定的！如果你这么做并不能达到你的目的：

```bash
> npx hexo new page --path about/me
```

此时 Hexo 会创建 `source/_posts/about/me.md`，同时 `me.md` 的 Front Matter 中的 title 为 `"page"`。这是因为在上述命令中，hexo-cli 将 `page` 视为指定文章的标题、并采用默认的 `layout`。

### 生成静态文件

```bash
> npx hexo generate
```

| 选项                  | 描述                                                                                                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-d`, `--deploy`      | 文件生成后立即部署网站                                                                                                                                              |
| `-w`, `--watch`       | 监视文件变动                                                                                                                                                        |
| `-b`, `--bail`        | 生成过程中如果发生任何未处理的异常则抛出异常                                                                                                                        |
| `-f`, `--force`       | 强制重新生成文件  Hexo 引入了差分机制，如果 `public` 目录存在，那么 `hexo g` 只会重新生成改动的文件。使用该参数的效果接近 `hexo clean && hexo generate` |
| `-c`, `--concurrency` | 最大同时生成文件的数量，默认无限制                                                                                                                                  |

该命令可以简写为

```bash
> npx hexo g
```

### 启动本地服务

```bash
> npx hexo server
```

对Hexo的内容进行预览。
启动服务器。默认情况下，访问网址为： `http://localhost:4000/`。

| 选项             | 描述                           |
| ---------------- | ------------------------------ |
| `-p`, `--port`   | 重设端口                       |
| `-s`, `--static` | 只使用静态文件                 |
| `-l`, `--log`    | 启动日记记录，使用覆盖记录格式 |

### 将 Hexo 部署到 GitHub Pages

>生成密钥

```bash
> git config --global user.name "Name"
> git config --global user.email "Email"
```

Name与Email为github注册的用户名和邮箱，然后生成密钥

```bash
> ssh-keygen -t rsa -C "Email"
```

之后全部回车确认到底。然后，在用户文件夹下的`.ssh`文件夹中找到`id_rsa.pub`文件，打开复制里面的全部内容。
在浏览器中，登录你的github账号打开Settings，在右侧中找到SSH and GPG keys选项并打开，选择New SSH key，填写好标题，把复制的内容全部粘贴上去。

>本地连接github

```bash
> ssh -T git@github.com
```

该命令无法连接问题，错误：**ssh: connect to host github.com port 22: Connection refused**
在用户文件夹中找到`.ssh`文件夹，在里面新建一个`config`文件，里面填写

```bash
Host github.com  
User xxxxx@xx.com   //注册github的邮箱
Hostname ssh.github.com  
PreferredAuthentications publickey  
IdentityFile ~/.ssh/id_rsa  //填写你的id_rsa的绝对路径
Port 443
```

再一次运行命令，一直确定，得到下面结果就说明连接成功

```bash
Hi ******! You've successfully authenticated, but GitHub does not provide shell access.
```

>修改Hexo配置文件

在初始化的博客系统的根目录下，找到`_config.yml`文件，找到配置的最后一行

```bash
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
    type: ''
```

修改为

```bash
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
    type: git
    repository: git@github.com:****/****.github.io.git  #你的仓库以ssh格式下载的地址
    branch: master
```

### 部署网站

```bash
> npx hexo deploy
```

| 参数               | 描述                     |
| ------------------ | ------------------------ |
| `-g`, `--generate` | 部署之前预先生成静态文件 |

该命令可以简写为：

```bash
> npx hexo d
```

发布失败，错误：**ERROR Deployer not found: git**

```bash
> npm install --save hexo-deployer-git
```

安装后继续发布

> 使用自己的域名

在Hexo目录下的public文件夹中，新建CNAME文件,输入你的域名就可以了，否则每次上传发布都需要重新绑定域名。

### 后续维护

> 查看当前hexo版本

```bash
> npx hexo version
```

> 升级hexo

```bash
> npm i hexo-cli -g
```

升级完成后查看版本，是否更新成功

> 检查系统中的插件是否有升级的

```bash
> npm install -g npm-check
> npm-check
```

> 升级插件

```bash
> npm install -g npm-upgrade
> npm-upgrade
> npm update -g
> npm update --save
```

>删除缓存：如果主题没有变化，只有内容有变化可以忽略此步骤

```bash
> npx hexo clean
```

> 生成静态网页

```bash
> npx hexo g
```

> 上传/发布

```bash
> npx hexo d
```

## 二、使用Hexo主题并配置文件

本文安装主题为[hexo-theme-bamboo](https://github.com/yuang01/hexo-theme-bamboo)
最新文档，请[点击gitee查看](https://yuang01.gitee.io/)

### 主题下载

首先需要安装好 hexo博客系统，进入`themes`文件夹下使用 `Git clone` 命令来下载主题:

github安装

```bash
> git clone https://github.com/yuang01/hexo-theme-bamboo.git
```

gitee安装

```bash
> git clone https://gitee.com/yuang01/hexo-theme-bamboo.git
```

修改hexo本地博客系统文件夹下的配置文件`_config.yml`，把主题改为`hexo-theme-bamboo`，通过主题文件夹下的`config.yml`对主题进行配置。

### 配置文件

下载 完成后，需要修改配置才能启用主题。
修改根目录下的 `_config.yml` 的 `theme` 的值：`theme: hexo-theme-bamboo`

#### 代码高亮

首先，保证Hexo为最新版。在根目录的配置文件`_config.yml`中，将`highlight`的`enable`设置为false，然后将`prismjs`的`enable`设置为true
当你使用的是`prismjs`作为代码高亮的话,你还可以在单独的文章中设置代码高亮主题，这样可以实现不同的页面，有不同的代码高亮主题：

```yaml
title: Hexo主题--Bamboo介绍
date: 2021-01-5 23:28
swiper: true
swiperImg: '/medias/11.jpg'
img: '/medias/7.jpg'
categories: 前端
tags: [Hexo, hexo-theme-bamboo]
top: true
prismjs: dark # 设置该篇文章的代码高亮主题为dark
```

#### 内容搜索

安装[hexo-generator-search](https://github.com/wzpan/hexo-generator-search) 的 Hexo 插件来做内容搜索：

```bash
> npm install hexo-generator-search --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```yaml
search:
  path: search.xml
  field: post
```

在主题文件夹下的`_config.yml`文件中设置`search`为true或者false控制显示隐藏

#### 新建分类页面

`categories` 页是用来展示所有分类的页面，也就是导航上的分类页面，如果在你的博客 `source` 目录下还没有 `categories/index.md` 文件，那么你就需要手动或者使用命令新建一个，命令如下：

```bash
> npx hexo new page "categories"
```

编辑你刚刚新建的页面文件 `/source/categories/index.md`，**至少需要以下内容**：

```yaml
---
title: categories
date: 2020-09-14 15:30:30
type: "categories"
layout: "categories"
---
```

#### 新建标签页面

`tags` 页是用来展示所有标签的页面，如果在你的博客 `source` 目录下还没有 `tags/index.md` 文件，那么你就需要新建一个，命令如下：

```bash
> npx hexo new page "tags"
```

编辑你刚刚新建的页面文件 `/source/tags/index.md`，**至少需要以下内容**：

```yaml
---
title: tags
date: 2020-09-14 15:30:30
type: "tags"
layout: "tags"
---
```

#### 新建关于我页面

`about` 页是用来展示**关于我和我的博客**信息的页面，如果在你的博客 `source` 目录下还没有 `about/index.md` 文件，那么你就需要新建一个，命令如下：

```bash
> npx hexo new page "about"
```

编辑你刚刚新建的页面文件 `/source/about/index.md`，**至少需要以下内容**：

```yaml
---
title: about
date: 2020-09-14 15:30:30
type: "about"
layout: "about"
---
```

然后可以在本主题下的`_config.yml`文件下，对关于我页面信息相应的进行更改。

#### 中文链接转拼音

如果你的文章名称是中文的，那么 Hexo 默认生成的永久链接也会有中文，这样不利于 `SEO`，且 `gitment` 评论对中文链接也不支持。我们可以用 [hexo-permalink-pinyin](https://github.com/viko16/hexo-permalink-pinyin) Hexo 插件使在生成文章时生成中文拼音的永久链接。  
安装命令如下：

```bash
> npm i hexo-permalink-pinyin --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```yaml
permalink_pinyin:
  enable: true
  separator: '-' # default: '-'
```

#### 添加RSS订阅

本主题中还使用到了 [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 的 Hexo 插件来做 `RSS`，安装命令如下：

```bash
> npm install hexo-generator-feed --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```yaml
feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '
  order_by: -date
```

#### Front-matter

这个指的是你在你的文章页面里面写的参数，针对的是这一篇文章，例如

```yaml
---
title: Hexo主题--Bamboo介绍
date: 2020-09-14 14:06
swiper: true # 是否将文章放入轮播图中
swiperImg: '/medias/1.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: '/medias/1.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: 前端
tags: [Hexo, hexo-theme-bamboo]
top: true
---
```

`Front-matter` 选项中的所有内容均为**非必填**的。但我仍然建议至少填写 `title` 和 `date` 的值。

| 配置选项        | 默认值                | 描述                                                                                                                                                          |
| --------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| title           | `Markdown` 的文件标题 | 文章标题，强烈建议填写此选项                                                                                                                                  |
| date            | 文件创建时的日期时间  | 发布时间，强烈建议填写此选项，且最好保证全局唯一                                                                                                              |
| swiper          | false                 | 表示该文章是否需要加入到首页轮播封面中                                                                                                                        |
| swiperImg       | 无                    | 表示该文章在首页轮播封面需要显示的图片路径，如果没有，则默认使用文章的特色图片                                                                                |
| swiperDesc      | 无                    | 表示该文章在首页轮播封面需要显示的文字描述（摘要），如果没有，则使用`excerpt`，如果`excerpt`也没有，则取文章内容                                              |
| img             | 无                    | 文章特征图，该文章显示的图片，没有则默认使用文章的特色图片                                                                                                    |
| excerpt         | 无                    | 文章描述（摘要），该文章在首页的描述文字，如果没有，则取`swiperDesc`,如果`swiperDesc`也没有，则取文章内容（优先取`<!-- more -->`上面的内容）                  |
| top             | false                 | 将该值设为true，则将该篇文章显示在首页的置顶栏目中                                                                                                            |
| toc             | true                  | 将该值设为false，则该篇文章不显示右侧目录                                                                                                                     |
| tocOpen         | true                  | 将该值设为false，则该篇文章右侧目录默认收缩                                                                                                                   |
| onlyTitle       | false                 | 文章详情页头部是否只显示标题，不显示日期等信息                                                                                                                |
| comments        | true                  | 将该值设为false，则该篇文章不显示评论                                                                                                                         |
| share           | true                  | 将该值设为false，则该篇文章不显示分享按钮                                                                                                                     |
| copyright       | true                  | 将该值设为false，则该篇文章不显示版权声明                                                                                                                     |
| donate          | true                  | 将该值设为false，则该篇文章不显示打赏按钮                                                                                                                     |
| bgImg           | \-                    | 单独为这篇文章设置背景图片或者背景颜色，可以是数组，数组里面放图片链接，可以是字符串，字符串里面是颜色值，空值则背景颜色透明                                  |
| bgImgTransition | fade                  | 该篇文章的bgImg设置为数组,该值表示背景图片切换的动画, 有三种值（fade, scale, translate-fade）                                                                 |
| bgImgDelay      | 180000(三分钟)        | 该篇文章的bgImg设置为数组,该值表示背景图片切换的延迟时间,                                                                                                     |
| categories      | 无                    | 文章分类，本主题的分类表示宏观上大的分类，只建议一篇文章一个分类                                                                                              |
| prismjs         | 无                    | 如果使用的是hexo自带的prismjs代码高亮，通过设置该值为该篇文章设置不同的代码高亮主题（default, coy, dark, funky, okaidia, solarizedlight, tomorrow, twilight） |
| tags            | 无                    | 文章标签，一篇文章可以多个标签                                                                                                                                |
| mathjax         | false                 | mathjax公式                                                                                                                                                   |
| imgTop          | true                  | 设置为`false`则文章和自定义页面顶部不要图片                                                                                                                   |
