## 引言

人脑有限，对于同一个问题，也许当时花了许久时间解决了，然而过了一段时间，只留下一个印象，当再次需要解决时，还是需要较长时间来寻找以前的资料。因此，在这里建立个人博客，一是为了整理记录，让自己省心；二是知识的分享，如果这里的信息恰好能够帮助你，我同样也会感到非常开心。

其实在一年前我就尝试过使用 Hexo 和 Github Page 来搭建个人博客，但是当时主要是觉得很炫，并没有耐心把知识记录下来，因此，仅仅是搭了个框架，全站仅有一篇 Hello World 。近来重新捡起的时候，几乎所有的资料还是得重新寻找，按照好几篇教程来搭建这篇博客，因此才有了前面的感慨。在这里，我将会把自己的折腾过程记录下来，并且将会随着自己博客设置的改变而不断更新...

<!--more-->

## 环境部署

首先，我的环境为：`node: V11.2.0; npm: V6.4.1 git: V2.18.0; hexo: V3.8.0`

### 安装 Node.js

下载最新版 [Node.js](https://nodejs.org/en/download/) .

安装选项默认即可。

安装好之后，摁 `Win+R` 打开命令提示符，输入 `node -v` 和 `npm -v` ，如果出现版本号，那么就安装好了。

### 安装 Git

为了把本地的网页文件上传至网上(github, coding 等)托管，我们需要用到分布式版本控制工具—— [Git](https://git-scm.com/downloads) .

安装选项还是默认，注意最后一步添加路径时选择 `Use Git from the Windows Command Prompt` ，这样我们可以直接在命令提示符里打开 git 了。

安装完成后在命令提示符中输入 `git --version` 验证是否安装成功。

### 注册 Github 账号

打开 <https://github.com> ，新建一个项目，输入自己的项目名字，后面一定要加 `.github.io` 后缀，最好是你的 `Github 账号名(唯一)+.github.io` ，如下图所示。

![github](https://gitee.com/kivenc/chaos/raw/master/upload_images/github.png)

由于我已经用自己的名字注册过了，所以这里显示不可用。之后就可以通过 `https://yourname.github.io` 访问你建好的网站了。

### 注册 Coding 账号

由于 github 的访问速度较慢，因此我将文件代码同时部署至 coding 上，打开 [腾讯云开发者平台](https://dev.tencent.com/) ，新建一个项目，项目名称可同样设为 `yourname` ，如下图所示。

![coding](https://gitee.com/kivenc/chaos/raw/master/upload_images/coding.png)

之后第一次上传代码后，在代码下拉菜单中有个 `Pages 服务` ，开启 `静态 Pages 应用` ，即可通过 `https://yourname.coding.me` 访问你的网站内容。

![pages](https://gitee.com/kivenc/chaos/raw/master/upload_images/pages.png)

### 安装 Hexo

在合适的地方新建一个文件夹，用来存放自己的博客文件，如 `blog` ，然后进入 `blog` 文件夹右击 `Git Bash Here` ，打开 git 的控制台窗口，之后我们所有的操作都在 git 控制台中进行。

执行如何命令安装 Hexo：

```bash
sudo npm install -g hexo
```

安装完后输入 `hexo -v` 验证是否安装成功。

然后初始化我们的网站，执行 init 命令初始化 hexo，命令：

```bash
hexo init
```

至此，主要安装工作已经完成！ blog 就是你的博客根目录，所有操作都在里面进行。

生成静态页面命令：

```bash
hexo generate
# 或者 hexo g
```

启动本地服务，进行文章预览调试，本地启动命令：

```bash
hexo server
# 或者 hexo s
```

然后在浏览器打开 <http://localhost:4000/> ，就可以看到我们的原始博客了，摁 `ctrl+c` 可以关闭本地服务器。

### 连接 Github、Coding 与本地

首先，安装 `hexo-deployer-git` 插件，右键打开 git bash，然后输入下面命令：

```bash
npm install hexo-deployer-git --save
```

接着，添加 `SSH key` ，命令如下：

```bash
git config --global user.name "<your_name>"
git config --global user.email "<your_email>"
ssh-keygen -t rsa -C "<your_email>"
```

其中 `<youe_name>` 和 `<your_email>` 根据你注册 github 的信息自行更改。

然后，打开 [github](https://github.com) ，在 `settings` 中点击 `SSH and GPG keys` ，新建一个 SSH，名字随意，比如 blog。复制密钥文件内容(路径形如`C:\Users\Administrator\.ssh\id_rsa.pub`)，粘贴至新建的 SSH 中。

测试是否添加成功，在命令行依次输入以下命令，返回 “You’ve successfully authenticated” 即成功。

```bash
ssh -T git@github.com
yes
```

同理，打开 [腾讯开发者平台](https://dev.tencent.com) ，在 `setting` 中点击 `SSH 公钥` ，新建一个 SSH，将之前的密钥内容添加进去。测试命令为：

```bash
ssh -T git@git.coding.net
yes
```

最后，修改 `_config.yml` (在站点目录下)。在文件末尾修改为：

```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
- type: git
  repo: git@github.com:<Github账号名称>/<Github账号名称>.github.io.git
  branch: master
- type: git
  repo: git@git.dev.tencent.com:<Coding账号名称>/<Coding账号名称>.git
  branch: master
```

至此，环境部署已经完成。

## 写文章、发布文章

在博客根目录下右键打开 git，输入以下命令，新建一篇文章：

```bash
hexo new post "article_title"
# 或者 hexo n "article_title"
```

然后在 `..\blog\source\_posts` 目录中会发现你的文章文件，编写完 Markdown 文件后，输入 `hexo g` 生成静态网页，输入 `hexo s` 进行本地预览效果，最后部署至 github、coding 上，命令如下：

```bash
hexo deploy
# 或者 hexo d
```

过一会打开你的 `https://yourname.github.io` 或 `https://yourname.coding.me` 就能看到你发布的文章了。

### 常用命令

```bash
hexo n "postName" # 新建文章，文章路径为 source/_posts
hexo new draft "draftName"  # 新建草稿，不会发布至你的网站，文章路径为 source/_drafts
hexo new page "pageName"  # 新建页面，文章路径为 source
hexo publish draft "draftName"  # 将草稿进行发布
hexo clean  # 清除缓存，建议每次部署时先执行该命令，再生成静态页面
hexo g  # 生成静态页面至 public 目录
hexo s  # 开启预览访问端口(默认端口 4000，‘ctrl+c’ 关闭 server)
hexo d  # 将文件进行部署
hexo help # 查看帮助
```

## 绑定域名

还没有进行该步骤。

计划为：购买域名后，将国内流量解析至 `https://kivenckl.coding.me` ，而境外流量解析至 `https://kivenckl.github.io` 。

## 修改主题

选择自己喜欢的主题，详见： <https://github.com/search?q=hexo-theme>

应用主题：

- 下载主题
- 将下载好的主题文件夹，粘贴到站点目录的 `themes` 下
- 更改站点配置文件 `_config.yml` 的 `theme` 字段，为主题文件夹的名称

例如，我选择的主题为 `NexT` ：

```yml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

## 个性化设置

### 添加标签页面

生成 `分类` 页并添加 type 属性，进入博客所在文件夹，打开命令行，执行命令：

```bash
hexo new page categories
```

成功之后，找到 `/source/categories/index.md` 文件，打开添加 `type: “categories”` ：

```markdown
---
title: categories
date: 2018-12-02 10:56:16
type: "categories"
comments: false
---
```

注：`comments: false` 可选

同理，生成 `标签` 页并添加 type 属性。

```bash
hexo new page tags
```

成功之后，找到 `/source/tags/index.md` 文件，打开添加 `type: “tags”` ：

```markdown
---
title: tags
date: 2018-12-02 10:55:56
type: "tags"
comments: false
---
```

注：`comments: false` 可选

之后，就可以给文章添加分类和标签属性了。例如：

```markdown
---
title: Markdown 中 LaTeX 数学公式命令
mathjax: true
abbrlink: 9a79e44d
date: 2018-11-27 13:46:17
categories: 笔记
tags:
    - Markdown
    - LaTeX
comments: true
---
```

点击首页的 `标签` 或者是 `分类` 就可以看到对应标签下或分类下的所有文章。

### 设置文章模板

每次新建文章都会调用文章模板，其位于 `scaffolds/post.md` 文件，因此，我们可以对其进行必要的修改，提供便利。例如，我的修改如下：

```markdown
---
title: {{ title }}
date: {{ date }}
categories:
tags:
comments: true
mathjax: true
---
```

其中 `comments` 用于控制是否开启评论，`mathjax` 用于控制是否公式的加载，对于不需要渲染公式的文章，关闭 mathjax 可提高页面的加载速度。

## SEO优化

### 何为 SEO

> 搜索引擎优化（英语：search engine optimization，缩写为 SEO），是一种通过了解搜索引擎的运作规则来调整网站，以及提高目的网站在有关搜索引擎内排名的方式。由于不少研究发现，搜索引擎的用户往往只会留意搜索结果最前面的几个条目，所以不少网站都希望通过各种形式来影响搜索引擎的排序，让自己的网站可以有优秀的搜索排名。当中尤以各种依靠广告维生的网站为甚。
> 所谓 “针对搜索引擎作最优化的处理”，是指为了要让网站更容易被搜索引擎接受。搜索引擎会将网站彼此间的内容做一些相关性的数据比对，然后再由浏览器将这些内容以最快速且接近最完整的方式，呈现给搜索者。搜索引擎优化就是通过搜索引擎的规则进行优化，为用户打造更好的用户体验，最终的目的就是做好用户体验。
> 对于任何一个网站来说，要想在网站推广中获取成功，搜索引擎优化都是至为关键的一项任务。同时，随着搜索引擎不断变换它们的搜索排名算法规则，每次算法上的改变都会让一些排名很好的网站在一夜之间名落孙山，而失去排名的直接后果就是失去了网站固有的可观访问流量。所以每次搜索引擎算演法的改变都会在网站之中引起不小的骚动和焦虑。可以说，搜索引擎优化是一个愈来愈复杂的任务。——[维基百科](https://zh.wikipedia.org/wiki/%E6%90%9C%E5%B0%8B%E5%BC%95%E6%93%8E%E6%9C%80%E4%BD%B3%E5%8C%96)

### 给站点添加 sitemap

首先需要安装两个插件来生成 sitemap 文件，前一个是传统的 sitemap，后一个是百度的 sitemap。

```bash
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```

然后需要修改站点配置文件 `_config.yml` ，配置 `sitemap` 插件。

```yml
Plugins: # 在该区域添加两个插件名称
  - hexo-generator-sitemap
  - hexo-generator-baidu-sitemap

# Sitemap
sitemap:
  path: sitemap.xml

# baidusitemap
baidusitemap:
  path: baidusitemap.xml
```

安装完成后执行 `hexo g` 即会在站点 `public` 目录下生成 `sitemap.xml` 和 `baidusitemap.xml` 。

### 添加 robots.txt

在站点 `source` 文件夹下新建 `robots.txt` 文件，文件内容如下：

```text
User-agent: *
Allow: /
Allow: /archives/
Allow: /categories
Allow: /tags/

Disallow: /vendors/
Disallow: /js/
Disallow: /css/
Disallow: /fonts/
Disallow: /vendors/
Disallow: /fancybox/

Sitemap: https://kivenckl.github.io/sitemap.xml
Sitemap: https://kivenckl.github.io/baidusitemap.xml
Sitemap: http://kivenckl.coding.me/sitemap.xml
Sitemap: http://kivenckl.coding.me/baidusitemap.xml
```

`Allow` 字段的值即为允许搜索引擎爬取的内容，可以对应到主题配置文件中的 menu 目录配置，如果菜单栏还有其他选项都可以按照格式自行添加。

需要将下方的域名改成自己的域名。

### 将网站链接提交至 Google

打开 [Google Search Console](https://www.google.com/webmasters/) ，添加博客地址。这里，我将 `https://kivenckl.github.io` 和 `https://kivenckl.coding.me` 两个域名都提交至 Google。

对于 NexT 主题而言，站点验证较为简单，选择 `HTML 标记` 的方法进行站点验证，打开主题配置文件 `_config.yml` ，找到 `google_site_verification` 字段，将标记代码中 `content` 部分代入，重新生成即可验证成功，对于 Baidu、Bing 验证同理。

然后提交 `robots.txt` 以及站点地图 `sitemap.xml` 、`baidusitemap.xml` ，验证无误即可。

之后就可以 google 搜索一下你的关键词和博客 title 测试一下了。

将网站链接提交至 Baidu 同理，由于百度不收录 `https://kivenckl.github.io` ，因此，我只将 `https://kivenckl.coding.me` 提交至百度。

最后，记得修改主题配置文件 `_config.yml` 中 `baidu_push` 字段，用于 Baidu 主动推送链接。

```yml
# Enable baidu push so that the blog will push the url to baidu automatically which is very helpful for SEO
baidu_push: true
```

### 修改文章链接

Hexo 默认的文章链接形式为 `domain/year/month/day/postname` ，当我们把文章源文件名改掉之后，链接也会改变，很不友好，并且四级目录，不利于 SEO。

因此，使用 `hexo-abbrlink` 插件，生成文章的永久链接，后期无论怎么修改也不会改变该链接。

```bash
npm install hexo-abbrlink --save
```

在站点配置文件 `_config.yml` 中修改：

```yaml
permalink: post/:abbrlink.html
abbrlink:
  alg: crc32 # 算法：crc16(default) and crc32
  rep: hex   # 进制：dec(default) and hex
```

可选择模式有：

- crc16 & hex
- crc16 & dec
- crc32 & hex
- crc32 & dec

## 寻找图床

当向文章中添加图片时，如果图片来源于网络，那么还比较好办，直接引用那个链接即可，不过也有问题，那就是如果那个链接挂了那么你的图片也就无法显示。另外如果你的图片来源于本地，那么更麻烦了。一种做法是使用第三方服务器，比如七牛，当需要插入图片时，先把图片上传到七牛的服务器然后再使用，我觉得很麻烦。这里选择另外一种方法。

首先修改 `_config.yml` (在站点目录下) 中 `post_asset_folder` 字段：

```yaml
# post_asset_folder: false
post_asset_folder: true
```

当设置该字段为 `true` 时，在建立文件时，Hexo 会自动建立一个与文章同名的文件夹，你就可以把与该文章相关的所有资源都放到那个文件夹，这么一来，你就可以很方便的使用资源。例如，文章 `post` 需要插入图片 `test.png` 时，就可以使用 `![](post/test.png)` 。

问题是这样在本地显示没有问题，但是发布之后就无法显示，使用 `hexo-asset-image` 插件来解决。

在博客根目录右击打开 `git bash` ，执行以下命令：

```bash
npm install https://github.com/CodeFalling/hexo-asset-image --save
```

重新生成之后就可以在你自己的网页上正常显示了。

> 对于因为 SEO 优化，使用 `abbrlink` 插件修改过文章链接的朋友而言，这种方法还需要进一步修改一下。由于原来的 `permalink: :year/:month/:day/:title/` 变成了 `permalink: post/:abbrlink.html` 。打开博客根目录下 `node_modules\hexo-asset-image\index.js` ，增加一行命令，如下所示：
>
> ```javascript
>   var config = hexo.config;
>   if(config.post_asset_folder){
>     var link = data.permalink;
>     link = link.replace('.html', '/');  //新增加，针对修改后的 permalink
>   var beginPos = getPosition(link, '/', 3) + 1;
> ```
>
> 之后就可以正常显示了，仅供参考。对于修改成其他链接形式的朋友也有一定的参考意义。

## 公式支持

首先 Hexo 的自带的 Markdown 引擎并不支持 $\LaTeX$ 公式，但是 MathJax 支持，因此首先要启用 MathJax 才能渲染 $\LaTeX$ 公式。如果你已经安装了 [NexT](http://theme-next.iissnan.com/) 主题，开启 MathJax 支持非常容易，在最新版的 NexT 主题的 `_config.yml` 文件里，找到 MathJax 相关部分，配置如下：

```yaml
math:
  enable: true

  # Default(true) will load mathjax/katex script on demand
  # That is it only render those page who has 'mathjax: true' in Front Matter.
  # If you set it to false, it will load mathjax/katex srcipt EVERY PAGE.
  per_page: true

  engine: mathjax
  #engine: katex
```

将 `per_page` 设为 `true` ，那么每篇文章都可以选择是否加载 MathJax ，从而提高不需要 MathJax 的文章的加载速度，文章的设置参见 [设置文章模板](#设置文章模板) 。

由于 Hexo 的渲染引擎 “hexo-renderer-marked” 与 MathJax 存在部分语义冲突，因此直接使用 $\LaTeX$ 语法将会出错。解决方案这里提供两种。

### 更换 kramed 引擎

更换 Hexo 的 Markdown 渲染引擎， [hexo-renderer-kramed](https://github.com/sun11/hexo-renderer-kramed) 引擎是在默认的渲染引擎 [hexo-renderer-marked](https://github.com/hexojs/hexo-renderer-marked) 的基础上修改了一些 bug，两者比较接近，也比较轻量级，替换命令如下：

```bash
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```

更换引擎后行间公式可以正确渲染了，但是行内公式渲染还是有问题。

接下来到博客根目录下，找到 `node_modules\kramed\lib\rules\inline.js` ，把 `escape` 变量和 `em` 变量的值做相应的修改：

```javascript
//  escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
  escape: /^\\([`*\[\]()#$+\-.!_>])/

//  em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
  em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/
```

### 更换 pandoc 引擎

[hexo-renderer-pandoc](https://github.com/wzpan/hexo-renderer-pandoc) 引擎十分靠谱，使用该 renderer 之前请确保你已经安装了 [Pandoc](https://github.com/jgm/pandoc/releases) ，然后卸载之前的 renderer ，安装 pandoc renderer：

```bash
npm uninstall hexo-renderer-marked --save
# 如果之前是 kramed 引擎，则 npm uninstall hexo-renderer-kramed --save
npm install hexo-renderer-pandoc --save
```

注意，如果使用该引擎，那么书写 Markdown 时需要遵循 [Pandoc 对 Markdown 的规定](https://pandoc.org/MANUAL.html#pandocs-markdown) 。

有一些比较明显的需要注意的事项：正常的文字后面如果跟的是 [`list`](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#lists), [`table`](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#tables) 或者 [`quotation`](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#blockquotes)，文字后面需要空一行，如果不空行，这些环境将不能被 Pandoc renderer 正常渲染。

另外，文中的 URL 使用 Pandoc 渲染以后是普通的文本格式，不能点击，可以通过用 `<>` 包围 URL 的方式把 URL 变成可点击的 URL。

> 在我使用该引擎时，发生无法渲染的错误，与 Pandoc 的 `smart` 参数有关，具体不是很清楚是什么原因，在博客根目录中打开 `node_modules\hexo-renderer-pandoc\index.js` ，把 `args` 变量进行修改：
>
> ```javascript
> // var args = [ '-f', 'markdown-smart', '-t', 'html-smart', math]
>   var args = [ '-f', 'markdown', '-t', 'html', math]
> ```
>
> 之后就可以了，仅供参考。

关于如何在 Markdown 中使用 $\LaTeX$ 公式，请参考 [Markdown 中 LaTeX 数学公式命令](https://kivenckl.github.io/post/9a79e44d.html) 。

## 参考文献

1. [【持续更新】最全 Hexo 博客搭建 + 主题优化 + 插件配置 + 常用操作 + 错误分析](https://www.simon96.online/2018/10/12/hexo-tutorial/)
2. [超详细 Hexo+Github 博客搭建小白教程](https://godweiyang.com/2018/04/13/hexo-blog/)
3. [在 Hexo 中渲染 MathJax 数学公式](https://www.jianshu.com/p/7ab21c7f0674)
4. [Hexo 书写 LaTeX 公式时的一些问题及解决方法](https://jdhao.github.io/2017/10/06/hexo-markdown-latex-equation/)
