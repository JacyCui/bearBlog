---
title: 个人博客搭建教程
description: 使用hexo框架，基于github托管，netlify生成，使用clouldflare加速来搭建自己的个人博客
abbrlink: 4248
cover: https://s2.loli.net/2022/09/05/pRG12wyKLYbN7ki.jpg
date: 2022-08-14 13:57:22
keywards: hexo github netlify clouldflare
tag: 实用教程
---

# 引言

这是一个个人博客搭建的教程，会手把手教读者从零开始搭建自己的个人博客。这个图文教程重点聚焦于实践操作的不部分，下面这个视频和本教程是配套的，不过它的重点会更聚焦于理论一些。

<iframe src="//player.bilibili.com/player.html?aid=729827798&bvid=BV1qD4y1z783&cid=811525188&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" height="500px" width="100%"> </iframe>


## 写博客的好处

在正式开始之前，我们先简单谈谈写博客的好处。所谓“学然后知不足，教然后知困”，写博客的过程其实是一个知识输出的过程，在很多的知识输入之后，将自己的理解与收获写成博客输出，一方面能够帮助自己总结回顾，加深思考，训练思维与写作能力，另一方面也能够帮助到其他正在学习相关知识的人。

除此之外，自己写的博客，日后很有可能成为重要的参考材料，就好像放在锦囊里的妙计一样，说不定哪天需要但是已经记不清的时候，自己曾经写的博客就能够发挥作用。

## 使用的工具及系统环境

这个教程使用的个人博客框架是hexo，博客文件拖管于github，博客网站用netlify生成，国内访问采用cloudflare进行CDN加速。使用这个解决方案的原因是不需要云服务器，也不需要备案，只需要有一个域名即可。

下面，我们来一步一步介绍我们即将使用的工具及其基本用法。

我个人的系统环境是macOS，后续的教程都基于macOS的终端及其包管理软件`homebrew`进行。
![macOS-and-homebrew](https://s2.loli.net/2022/09/05/f5spL29qYDBydZt.png)


其他的类unix系统安装过程也是类似的（只是包管理软件不同），windows系统请使用wsl或者linux虚拟机。

> 注：其实可以直接在windows上装，但是会麻烦不少，本教程适用于类unix的环境。



# hexo博客框架

## 介绍

![hexo](https://s2.loli.net/2022/09/05/7TNLUO32vQBPHpb.png)

`hexo`是一个基于nodejs的静态博客网站生成器，作者是来自台湾的`Tommy Chen`，为许多技术博客的博主所青睐，主要有如下的一些优点：

- 支持`Markdown`语法，编辑简单，排版优美；
- 能够快速生成静态html文件；
- 部署容易，接口简单；
- 兼容于各大主流操作系统；
- 社区主题、插件很多，遇到问题的时候能查到的参考材料也很多。

## 环境配置

搭建hexo首先需要有[nodejs](https://nodejs.org/zh-cn/)的环境，可以从官网直接下载。
![nodejs](https://s2.loli.net/2022/09/05/2xKvaPTDoyZcFbp.png)

当然，你也可以直接通过终端下的包管理软件一键安装，比如说我的环境是macOS，使用`brew`进行包管理，可以通过如下命令安装。

```bash
brew install node # homebrew安装nodejs
```

> Linux类似，用包管理软件即可。Windows的话需要从官网下载安装，安装完成后建议用git-bash（在安装git的时候应该已经同时安装了，如果没有的话可以去[官网](https://git-scm.com/)下载）来进行后续的操作，因为windows和unix的命令体系有差别。

最后查看版本信息以确认环境配置信息：

```bash
node -v # 查看node版本信息
npm -v # 查看npm版本信息
```

能够正常查看到版本信息，说明环境配置就已经完成了。

这里还需要提一下的是`npm`默认的官网源可能会比较慢，如果想要后续的下载速度快一些，可以通过下面的方式将源设置为淘宝源。

```bash
npm config get registry # 查看原来的源
npm config set registry https://registry.npm.taobao.org # 修改为淘宝源
npm config get registry # 查看现在的源
```

## 生成博客

### 安装

有了`npm`包管理软件，安装`hexo`就很方便了，只需要一行命令：

```bash
npm install hexo-cli -g # 全局安装hexo命令行工具
```

其中`-g`参数表示全局安装，没有这个参数就只在当前目录下安装，建议全局安装。

### 初始化

运行命令：

```bash
hexo init "你的博客目录名称" # 目录名称不含空格的时候双引号可以省略
```

得到如下的反馈信息：

```bash
INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git
INFO  Install dependencies
# 一些可能的中间信息
INFO  Start blogging with Hexo!
```

然后进入博客目录：

```bash
cd "博客目录"
```

安装博客需要的其他支持：

```bash
npm install # 安装的依赖项在package.json文件的dependencies字段中可以看到
```

### 博客项目目录结构介绍

查看目录结构：

```bash
tree -L 1
```

结果如下：

```bash
.
├── _config.landscape.yml
├── _config.yml
├── node_modules
├── package-lock.json
├── package.json
├── scaffolds
├── source
└── themes
```

各部分的含义：

- `_config.yml`
    - 为全局配置文件，网站的很多信息都在这里配置，比如说网站名称，副标题，描述，作者，语言，主题等等。具体可以参考官方文档：https://hexo.io/zh-cn/docs/configuration.html。
- `scaffolds`
    - 骨架文件，是生成新页面或者新博客的模版。可以根据需求编辑，当`hexo`生成新博客的时候，会用这里面的模版进行初始化。
- `source`
    - 这个文件夹下面存放的是网站的`markdown`源文件，里面有一个`_post`文件夹，所有的`.md`博客文件都会存放在这个文件夹下。现在，你应该能看到里面有一个`hello-world.md`文件。
- `themes`
    - 网站主题目录，`hexo`有非常丰富的主题支持，主题目录会存放在这个目录下面。
    - 我们后续会以默认主题来演示，更多的主题参见：https://hexo.io/themes/

### 生成新文章

```bash
hexo new post "test" # 会在 source/_posts/ 目录下生成文件 ‘test.md’，打开编辑
hexo generate        # 生成静态HTML文件到 /public 文件夹中
hexo server          # 本地运行server服务预览，打开 http://localhost:4000 即可预览你的博客
```

本地预览效果：

![preview](https://s2.loli.net/2022/09/05/jvikZ3Yu5mqBxPl.png)

这是`hexo`的默认主题，更多的主题可以从官网下载。

更详细的`hexo`命令可以查看文档：https://hexo.io/zh-cn/docs/commands

### 添加建站脚本

为了后续`netlify`建站方便，我们可以在`package.json`里面添加一个命令：

```json
{
    // ......
    "scripts": {
        "build": "hexo generate",
        "clean": "hexo clean",
        "deploy": "hexo deploy",
        "server": "hexo server",
        "netlify": "npm run clean && npm run build" // 这一行为新加
    },
    // ......
}
```

### 博客配置

由于我们这个教程的重点不是如何编写自己的博客，而是怎么把博客搭建起来，所以`hexo`的细节用法以及各种主题的设置我们就不展开细说了，官网有非常详细的文档和教程可供参考。这里简单提一下`_config.yml`的各个字段的含义：

```yml
# Site
title: Hexo  # 网站标题
subtitle:    # 网站副标题
description: # 网站描述
author: John Doe  # 作者
language:    # 语言
timezone:    # 网站时区, Hexo默认使用您电脑的时区

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child'
## and root as '/child/'
url: http://yoursite.com   # 你的站点Url
root: /                    # 站点的根目录
permalink: :year/:month/:day/:title/   # 文章的 永久链接 格式   
permalink_defaults:        # 永久链接中各部分的默认值

# Directory   
source_dir: source     # 资源文件夹，这个文件夹用来存放内容
public_dir: public     # 公共文件夹，这个文件夹用于存放生成的站点文件。
tag_dir: tags          # 标签文件夹     
archive_dir: archives  # 归档文件夹
category_dir: categories     # 分类文件夹
code_dir: downloads/code     # Include code 文件夹
i18n_dir: :lang              # 国际化（i18n）文件夹
skip_render:                 # 跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。    

# Writing
new_post_name: :title.md  # 新文章的文件名称
default_layout: post      # 预设布局
titlecase: false          # 把标题转换为 title case
external_link: true       # 在新标签中打开链接
filename_case: 0          # 把文件名称转换为 (1) 小写或 (2) 大写
render_drafts: false      # 是否显示草稿
post_asset_folder: false  # 是否启动 Asset 文件夹
relative_link: false      # 把链接改为与根目录的相对位址    
future: true              # 显示未来的文章
highlight:                # 内容中代码块的设置    
  enable: true            # 开启代码块高亮
  line_number: true       # 显示行数
  auto_detect: false      # 如果未指定语言，则启用自动检测
  tab_replace:            # 用 n 个空格替换 tabs；如果值为空，则不会替换 tabs

# Category & Tag
default_category: uncategorized
category_map:       # 分类别名
tag_map:            # 标签别名

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD     # 日期格式
time_format: HH:mm:ss       # 时间格式    

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10           # 分页数量
pagination_dir: page   # 分页目录

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape   # 主题名称

# Deployment
## Docs: https://hexo.io/docs/deployment.html
#  部署部分的设置
deploy:     
  type: '' # 类型，常用的git 
```


# Github项目文件托管

<img src="https://s2.loli.net/2022/09/05/bXDOiVtTygAH1CM.jpg" alt="git-github" style="zoom:50%;" />

这一步非常简单，`git`和`github`的基本用法我就不赘述了。创建本地仓库，然后推送到远端服务器即可：

```bash
cd "博客目录"
git init
git add .
git commit -m "my blog first commit"
git remote add origin "远端github仓库地址"
git branch -M main
git push -u origin main
```



# Netlify建站

## 介绍

Netlify是一个国外的免费的提供静态网站部署服务的平台，能够将托管 GitHub，GitLab 等上的静态网站部署上线。至于我们为什么不使用`github`自带的`gitpage`，原因很简单，访问速度慢。此外，Netlify还有很多别的功能支持，这里不作剧透，可以自行探索。

## 建站步骤

1. 首先注册并登陆[Netlify](https://www.netlify.com/)
    - 这一步需要能够科学上网，因为这是一个国外的网站
    - 我们的博客在开启cloundflare的CDN加速之前，也只能通过科学上网的方式访问
2. 新建站点：
    - ![create-site](https://s2.loli.net/2022/09/05/SJvGYlIzZVm7iP5.png)
3. 连接`github`：
    - ![connect-github](https://s2.loli.net/2022/09/05/oSa6BOtIQ8WkZX1.png)
4. 选择刚刚上传的博客项目：
    - ![select-project](https://s2.loli.net/2022/09/05/e2sGfOYSAFPylVB.png)
5. 一切默认，除了构建命令改成我们之前设置的`npm run netlify`：
    - ![site-config](https://s2.loli.net/2022/09/05/p3P2NJaQzuIZnYs.png)
    > 这里BaseDirectory为空表示项目目录是仓库目录的根目录。
6. 构建完成后我们就能够看到一个URL，打开网址就是我们的个人博客了
    - ![url-generate](https://s2.loli.net/2022/09/05/B2qmolnIgDH5uNt.png)

可以根据提示进行进一步的设置，比如说设置一下二级域名（即`netlify.app`之前的域名）。

在下面的演示中，我设置的`netlify`二级域名为`blogbearsir`，也就是说，我的个人博客站点的域名为`blogbearsir.netlify.app`。

不过现在，我们的个人博客已经算是搭建完成了。下面需要解决的就是配置域名和访问慢的问题了。

## 配置域名

配置域名的前提自然是要购买域名了，从任意域名服务商处购买一个域名。
![domian-purchase](https://s2.loli.net/2022/09/05/DFHYzywifpJTxqj.png)

然后设置域名解析，类型为`CNAME`（DNS知识点参见计算机网络相关教程），内容为`xxxxx.netlify.app`，其中`xxxxx`为你自己设置的个性二级域名。
![domain-resolve](https://s2.loli.net/2022/09/05/5OVcw9ypvRKQePJ.png)

设置完毕之后需要等待一段时间，因为DNS服务器需要一段时间来进行同步。

然后，我们还需要回到netlify中配置一下自己的用户域名，这样的话可以在国外获得netlify本身的CDN支持。
1. 在netlify设置用户域名。
    - ![set-custom-domain](https://s2.loli.net/2022/09/05/MDjxbIcWBEoLURA.png)
2. 进行相关的配置，由于我们的域名本身已经配置了解析，这里会显示出来，不用再额外添加记录，只需要一路默认即可。
    - ![add-record](https://s2.loli.net/2022/09/05/cqwL9xF8Eov6yVa.png)
    - ![activate-dns](https://s2.loli.net/2022/09/05/RTLcjynQYXbW9vI.png)
3. 设置一下netlify本身的对于国外CDN的支持。
    - ![netlify-cdn](https://s2.loli.net/2022/09/05/8v3ROjQc2WY9q7T.png)


之后，我们就可以通过自己配置的域名访问自己的个人博客，比如说我的博客地址是 https://blog.cuijiacai.com 。

> 这里`https`访问需要在`netlify`中配置，否则应该只能`http`访问。
> ![https-config](https://s2.loli.net/2022/09/05/f3q8hPbG5vsImeY.png)
> 需要注意一下的是，此刻的https配置过程中的dns验证已经可以通过，但是证书检查会失败，等到后面clouldflare加速配置完成之后，这个问题 就可以解决了。所以暂时应该只能http访问。

但是，此刻我们的博客访问依然需要科学上网，因为我们还没有国内的CDN的支持，下面，我们来解决这个问题。

# ClouldFlare加速

## 介绍

Netlify 虽然已经提供了 CDN 加速，但在使用过程中发现国内访问还是比较慢，Cloudflare 相对于国内的七牛云、阿里云等云服务商的 CDN 速度会慢一些，但是它有免费版本，而且最重要的是域名不用备案。

## 加速步骤

1. 注册[Clouldflare](https://www.cloudflare.com/zh-cn/)并登陆
2. 添加站点
    - ![add-site](https://s2.loli.net/2022/09/05/rqNObP5dzE6GY83.png)
    - ![config-site](https://s2.loli.net/2022/09/05/Dk3Y4BrltQeCOHI.png)
3. 选择免费套餐
    - ![choose-project](https://s2.loli.net/2022/09/05/SrhEAvmGZeqn8Co.png)
4. 添加 DNS 记录
    - 一般情况下 Cloudflare 会检测出来几条 DNS 记录，类型大多数是A，或者AAAA，由于我们是转发，所以应该是 CNAME 类型才对。有必要的话可能得手动配置一下。
    - ![update-record](https://s2.loli.net/2022/09/05/fSsAGV5JCeZuF1w.png)
5. 更改名称服务器
    - 这个步骤Cloudflare会提供一个在线的教程，主要步骤是在你的域名服务商那里修改 dns 解析服务器为 cloudflare 提供的地址，修改完成后点击完成。
    - ![modify-server](https://s2.loli.net/2022/09/05/vd2WxXGbJHmgAey.png)
    - 以阿里云为例，设置的步骤如下:
        1. 进入域名的配置界面
            - ![dns-manage](https://s2.loli.net/2022/09/05/ZfLiNUejRsCyhG3.png)
        2. 将域名服务器从阿里云的默认服务器改成clouldflare的服务器
            - ![change-server](https://s2.loli.net/2022/09/05/juxWl7i9QaeLTGK.png)
    - 配置完成后，clouldflare会有邮件通知(一般不会等太久)
        ![mail-notice](https://s2.loli.net/2022/09/05/JbBvp18Trne37kC.png)

## 配置https

在clouldflare配置完成之后，我们可以回到netlify去配置一下https访问。

1. 先确认一下dns解析:
    - ![verify-dns](https://s2.loli.net/2022/09/05/r6iHKWLktnRap1j.png)
2. 然后自动安装证书:
    - ![certify](https://s2.loli.net/2022/09/05/KvDupBFh8b9CScN.png)
3. 最后看到如下的界面，就说明https配置完成了:
    - ![https-config](https://s2.loli.net/2022/09/05/f3q8hPbG5vsImeY.png)

# 测试站点

等待一段时间之后，我们可以试着用自己的浏览器去访问自己配置的域名地址，如果在不科学上网的情况下能够正常看到如下的默认页面，则我们的个人博客就配置成功了。

![default-page](https://s2.loli.net/2022/09/05/HESMpeXbUFT3rsR.png)

本机能够正常访问之后，我们可以用[拨测](https://www.boce.com/)来检测一下域名解析与访问的速度。

![speed](https://s2.loli.net/2022/09/05/7C496Ruof1rBknM.png)

到此为止，我们的个人博客就彻底搭建完成啦。后续我们只需要修改博客的配置文件和博客本身的markdown源文件，然后push到github上，netlify会自动帮我们运行当初配置的建站脚本，然后将生成在public文件夹中的静态网页部署出去。

关于hexo博客的[写作方法](https://hexo.io/zh-cn/docs/writing)与各种好看的[主题的配置](https://hexo.io/themes/)可以查看官方的文档，多尝试多摸索，配置自己的个性页面吧。






