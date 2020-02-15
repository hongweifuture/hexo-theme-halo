# hexo-theme-halo

> 这是一个采用 `Material Design` 和响应式设计的 Hexo 博客主题。
>
> 基于 [hexo-theme-matery]( https://github.com/blinkfox/hexo-theme-matery ) 主题修改

 [主题演示](https://www.zhwei.cn/)

## 特性

- 简单漂亮，文章内容美观易读
- [Material Design](https://material.io/) 设计
- 博客名字动态显示，文章信息统计统页面展示
- 响应式设计，博客在桌面端、平板、手机等设备上均能很好的展现
- 首页轮播文章及每天动态切换 `Banner` 图片，首页轮播图设置为70%页面高度
- 首页`subtitle`替换打字效果，添加动态诗词自动切换，
- 瀑布流式的博客文章列表（文章无特色图片时会有 `24` 张漂亮的图片代替）
- 时间轴式的归档页，`增加简约风归档页面`
- 分类页、标签页和`标签云`同一页面显示，集中展示
- 丰富的关于我页面（包括关于我、文章统计图、我的项目、我的技能、相册等可自定义）
- 可自定义的数据的友情链接页面
- 支持文章置顶和文章打赏
- 类似于Python Django中`SLUG`用法的urlname
- 支持 `MathJax`
- `TOC` 目录
- 可设置复制文章内容时追加版权信息
- 可设置阅读文章时做密码验证
- [Gitalk](https://gitalk.github.io/)、[Gitment](https://imsun.github.io/gitment/)、[Valine](https://valine.js.org/) 和 [Disqus](https://disqus.com/) 评论模块（推荐使用 `Gitalk`）
- 集成了[不蒜子统计](http://busuanzi.ibruce.info/)、谷歌分析（`Google Analytics`）和文章字数统计等功能
- 支持在首页的音乐播放和视频播放功能
- 支持`emoji`表情，用`markdown emoji`语法书写直接生成对应的能**跳跃**的表情。
- 支持 [DaoVoice](http://www.daovoice.io/)、[Tidio](https://www.tidio.com/) 在线聊天功能。


## 下载

当你看到这里的时候，应该已经有一个自己的 [Hexo](https://hexo.io/zh-cn/) 博客了。如果还没有的话，不妨使用 `Hexo` 和 [Markdown](https://www.appinn.com/markdown/) 来写博客和文章。

点击 [这里](https://codeload.github.com/hongweifuture/hexo-theme-halo/zip/master) 下载 `master` 分支的最新稳定版的代码，解压缩后，将 `hexo-theme-halo` 的文件夹复制到你 Hexo 的 `themes` 文件夹中，并修改主题配置项即可。

当然你也可以在你的 `themes` 文件夹下使用 `git clone` 命令来下载:

```bash
git clone https://github.com/hongweifuture/hexo-theme-halo.git themes/halo
```

## 配置

### 切换主题

修改 Hexo 根目录下的 `_config.yml` 的  `theme` 的值：`theme: halo`

#### `_config.yml` 文件的其它修改建议:

- 请修改 `_config.yml` 的 `url` 的值为你的网站主 `URL`（如：`http://xxx.github.io`）。
- 建议修改两个 `per_page` 的分页条数值为 `6` 的倍数，如：`12`、`18` 等，这样文章列表在各个屏幕下都能较好的显示。
- 如果你是中文用户，则建议修改 `language` 的值为 `zh-CN`。

### 
## 创建页面

1. **直接将`themes/halo/config/source`内所有内容拷贝到`Hexo`根目录下的`source`文件夹内替换**
2. 或者自己重新创建，创建请参考下面

### 新建分类 categories 页

`categories` 页是用来展示所有分类的页面，如果在你的博客 `source` 目录下还没有 `categories/index.md` 文件，那么你就需要新建一个，命令如下：

```bash
hexo new page "categories"
```

编辑你刚刚新建的页面文件 `/source/categories/index.md`，至少需要以下内容：

```yaml
---
title: categories
date: 2020-01-01 00:00:00
type: "categories"
layout: "categories"
---
```

### 新建标签 tags 页

`tags` 页是用来展示所有标签的页面，如果在你的博客 `source` 目录下还没有 `tags/index.md` 文件，那么你就需要新建一个，命令如下：

```bash
hexo new page "tags"
```

编辑你刚刚新建的页面文件 `/source/tags/index.md`，至少需要以下内容：

```yaml
---
title: tags
date: 2020-01-01 00:00:00
type: "tags"
layout: "tags"
---
```

### 新建关于我 about 页

`about` 页是用来展示**关于我和我的博客**信息的页面，如果在你的博客 `source` 目录下还没有 `about/index.md` 文件，那么你就需要新建一个，命令如下：

```bash
hexo new page "about"
```

编辑你刚刚新建的页面文件 `/source/about/index.md`，至少需要以下内容：

```yaml
---
title: about
date: 2020-01-01 00:00:00
type: "about"
layout: "about"
---
```

### 新建留言板 contact 页（可选的）

`contact` 页是用来展示**留言板**信息的页面，如果在你的博客 `source` 目录下还没有 `contact/index.md` 文件，那么你就需要新建一个，命令如下：

```bash
hexo new page "contact"
```

编辑你刚刚新建的页面文件 `/source/contact/index.md`，至少需要以下内容：

```yaml
---
title: contact
date: 2020-01-01 00:00:00
type: "contact"
layout: "contact"
---
```

> **注**：本留言板功能依赖于第三方评论系统，请**激活**你的评论系统才有效果。并且在主题的 `_config.yml` 文件中，第 `19` 至 `21` 行的“**菜单**”配置，取消关于留言板的注释即可。

### 新建友情连接 friends 页（可选的）

`friends` 页是用来展示**友情连接**信息的页面，如果在你的博客 `source` 目录下还没有 `friends/index.md` 文件，那么你就需要新建一个，命令如下：

```bash
hexo new page "friends"
```

编辑你刚刚新建的页面文件 `/source/friends/index.md`，至少需要以下内容：

```yaml
---
title: friends
date: 2020-01-01 00:00:00
type: "friends"
layout: "friends"
---
```

同时，在你的博客 `source` 目录下新建 `_data` 目录，在 `_data` 目录中新建 `friends.json` 文件，文件内容如下所示：

```json
[{
	"avatar": "https://zhwei.cn/favicon.ico",
	"name": "HONGWEI",
	"introduction": "写如诗的内容，分享温而不沸的生活。",
	"url": "https://zhwei.cn",
	"title": "前去探索"
}, {
	"avatar": "https://avatar.csdnimg.cn/6/8/C/3_z_johnny.jpg",
	"name": "CSDN博客",
	"introduction": "Johnny's CSDN Blog 欢迎大佬们前去参观！",
	"url": "https://blog.csdn.net/z_johnny",
	"title": "前去参观"
}]
```

### 新建404页面（可选的）

`404` 页是用来展示**访问内容页面失效**信息的页面，如果在你的博客 `source` 目录下还没有 `404/index.md` 文件，那么你就需要新建一个，命令如下：

```bash
hexo new page "404"
```

编辑你刚刚新建的页面文件 `/source/404/index.md`，至少需要以下内容：

```
title: 404
date: 2020-01-01 00:00:00
type: "404"
layout: "404"
description: "你访问的页面已经去吃肉肉了~~"
```

`description`就是当访问出现`404`的时候显示的内容文字。

### 新建归档`时间轴`页面（可选的）

本主题已经默认归档页面显示`简约风`归档，如需时间轴样式，如在你的博客 `source` 目录下还没有 `timeline/index.md` 文件，那么你就需要新建一个，命令如下：

```bash
hexo new page "timeline"
```

编辑你刚刚新建的页面文件 `/source/timeline/index.md`，至少需要以下内容：

```
title: timeline
date: 2020-01-01 00:00:00
type: "timeline"
layout: "timeline"
```

当然可以两个归档风格的页面同时存在。

### 新建文章统计页面（可选的）

`data` 页是用来展示**文章统计**信息的页面，如果在你的博客 `source` 目录下还没有` data/index.md` 文件，那么你就需要新建一个，命令如下：

```bash
hexo new page "data"
```

编辑你刚刚新建的页面文件 `/source/data/index.md`，至少需要以下内容：

```
title: data
date: 2020-01-01 00:00:00
type: "data"
layout: "data"
```



## 菜单导航配置

### 配置基本菜单导航的名称、路径url和图标icon.

1.菜单导航名称可以是中文也可以是英文(如：`Index`或`主页`) 
2.图标icon 可以在[Font Awesome](https://fontawesome.com/icons) 中查找   

```yaml
menu:
  Index:
    url: /
    icon: fas fa-home
  Tags:
    url: /tags
    icon: fas fa-tags
  Categories:
    url: /categories
    icon: fas fa-bookmark
  Archives:
    url: /archives
    icon: fas fa-archive
  About:
    url: /about
    icon: fas fa-user-circle
  Friends:
    url: /friends
    icon: fas fa-address-book
```

### 二级菜单配置方法
如果你需要二级菜单则可以在原基本菜单导航的基础上如下操作     
1.在需要添加二级菜单的一级菜单下添加`children`关键字(如:`About`菜单下添加`children`)     
2.在`children`下创建二级菜单的 名称name,路径url和图标icon.      
3.注意每个二级菜单模块前要加 `-`。遵循`yaml`语法。  
4.注意缩进格式  

```yaml
menu:
  Index:
    url: /
    icon: fas fa-home
  Tags:
    url: /tags
    icon: fas fa-tags
  Categories:
    url: /categories
    icon: fas fa-bookmark
  Archives:
    url: /archives
    icon: fas fa-archive
  About:
    url: /about
    icon: fas fa-user-circle-o
  Friends:
    url: /friends
    icon: fas fa-address-book
  Medias:
    icon: fas fa-list
    children:
      - name: Musics
        url: /musics
        icon: fas fa-music
      - name: Movies
        url: /movies
        icon: fas fa-film
      - name: Books
        url: /books
        icon: fas fa-book
      - name: Galleries
        url: /galleries
        icon: fas fa-image
```

执行 `hexo clean && hexo g` 重新生成博客文件，然后就可以在文章中对应位置看到你用`emoji`语法写的表情了。

## 插件
### 代码高亮（建议安装）

由于 Hexo 自带的代码高亮主题显示不好看，所以主题中使用到了 [hexo-prism-plugin](https://github.com/ele828/hexo-prism-plugin) 的 Hexo 插件来做代码高亮，安装命令如下：

```bash
npm i -S hexo-prism-plugin
```

然后，修改 Hexo 根目录下 `_config.yml` 文件中 `highlight.enable` 的值为 `false`，并新增 `prism` 插件相关的配置，主要配置如下：

```yaml
highlight:
  enable: false

prism_plugin:
  mode: 'preprocess'    # realtime/preprocess
  theme: 'tomorrow'
  line_number: false    # default false
  custom_css:
```

### 搜索（建议安装）

本主题中还使用到了 [hexo-generator-search](https://github.com/wzpan/hexo-generator-search) 的 Hexo 插件来做内容搜索，安装命令如下：

```bash
npm install hexo-generator-search --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```yaml
search:
  path: search.xml
  field: post
```

### 文章字数统计插件（建议安装）

如果你想要在文章中显示文章字数、阅读时长信息，可以安装 [hexo-wordcount](https://github.com/willin/hexo-wordcount)插件。

安装命令如下：

```bash
npm i --save hexo-wordcount
```

然后只需在本主题下的 `_config.yml` 文件中，将各个文章字数相关的配置激活即可：

```yaml
postInfo:
  date: true
  update: false
  wordCount: false # 设置文章字数统计为 true.
  totalCount: false # 设置站点文章总字数统计为 true.
  min2read: false # 阅读时长.
  readCount: false # 阅读次数.
```

### 添加代码压缩（建议安装）

[hexo-neat]( https://github.com/rozbo/hexo-neat )插件实现压缩代码，底层是通过gulp来实现的。但是这个插件是有Bug的：

- 压缩 md 文件会使 markdown 语法的代码块消失
- 会删除全角空格

在博客站点根目录执行安装代码：

```
npm install hexo-neat --save
```

紧接着在站点根目录下的配置文件添加如下代码：

```
neat_enable: true
neat_html:
  enable: true
  exclude:
neat_css:
  enable: true
  exclude:
    - '*.min.css'
neat_js:
  enable: true
  mangle: true
  output:
  compress:
  exclude:
    - '*.min.js'
```

然后直接 hexo cl&&hexo g 就可以了，会自动压缩文件 。

补充：为了解决以上Bug，对于`halo`主题（其他主题自行解决）需要将以上默认配置修改为：

```
#hexo-neat 优化提速插件（去掉HTML、css、js的blank字符）
neat_enable: true
neat_html:
  enable: true
  exclude:
    - '**/*.md'
neat_css:
  enable: true
  exclude:
    - '**/*.min.css'
neat_js:
  enable: true
  mangle: true
  output:
  compress:
  exclude:
    - '**/*.min.js'
    - '**/**/instantpage.js'
    - '**/matery.js'
```

### 外链跳转插件 hexo-external-link（可选安装）
> [hexo-external-link](https://github.com/hvnobug/hexo-external-link)是一个跳转外链相关插件。自动为所有html文件中外链的a标签生成对应的属性。 比如 设置`target=’_blank’`, `rel=’external nofollow noopener noreferrer’ `告诉搜索引擎这是外部链接,不要将该链接计入权重。 同时自动生成外链跳转页面,默认在根目录下 go.html;

安装
```bash
# npm 安装 
npm install hexo-external-link --save 
```

配置插件
在Hexo根目录的_config.yml文件中添加如下配置。
```YAML
hexo_external_link:
  enable: true
  enable_base64_encode: true
  url_param_name: 'u'
  html_file_name: 'go.html'
  target_blank: true
  link_rel: 'external nofollow noopener noreferrer'
  domain: 'your_domain' # 如果开启了防盗链
  safety_chain: true
```
- enable：是否开启hexo_external_link插件 - 默认 false
- enable_base64_encode：是否对跳转url使用base64编码 - 默认 fasle
- url_param_name：url参数名,在跳转到外链传递给html_file_name的参数名 - 默认 ‘u’
- html_file_name：跳转到外链的页面文件路径 - 默认 'go.html'
- target_blank：是否为外链的a标签添加target='_blank' - 默认 true
- link_rel：设置外链的a标签的rel属性 - 默认 'external nofollow noopener noreferrer'
- domain：如果开启了防盗链,除了 localhost 和 domain 之外调用会跳到主页,同时也是判断链接是否为外链的依据 - 默认 window.location.host
- safety_chain：go.html 为了防止外链盗用 对域名进行的判断 - 默认 false 即关闭防盗链

### 添加 RSS 订阅支持（可选安装）

本主题中还使用到了 [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 的 Hexo 插件来做 `RSS`，安装命令如下：

```bash
npm install hexo-generator-feed --save
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

### 添加 sitemap 站点地图（可选安装）
本主题中还使用到了 [hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap) 的 Hexo 插件来做 `Sitemap`，安装命令如下：

```bash
npm install hexo-generator-sitemap --save
```

访问地址：/sitemap.xml

### 中文链接转拼音（可选安装）

如果你的文章名称是中文的，那么 Hexo 默认生成的永久链接也会有中文，这样不利于 `SEO`，且 `gitment` 评论对中文链接也不支持。我们可以用 [hexo-permalink-pinyin](https://github.com/viko16/hexo-permalink-pinyin) Hexo 插件使在生成文章时生成中文拼音的永久链接。

这里为可选安装，因为我希望使用`urlname`进行连接访问，中文链接转拼音由一个缺点就是当文章名字过长会显示十分臃肿。`urlname`的方式见下文。

安装命令如下：

```bash
npm i hexo-permalink-pinyin --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```yaml
permalink_pinyin:
  enable: true
  separator: '-' # default: '-'
```

> **注**：除了此插件外，[hexo-abbrlink](https://github.com/rozbo/hexo-abbrlink) 插件也可以生成非中文的链接。

### 添加emoji表情支持（可选安装）

本主题新增了对`emoji`表情的支持，使用到了 [hexo-filter-github-emojis](https://npm.taobao.org/package/hexo-filter-github-emojis) 的 Hexo 插件来支持 `emoji`表情的生成，把对应的`markdown emoji`语法（`::`,例如：`:smile:`）转变成会跳跃的`emoji`表情，安装命令如下：

```bash
npm install hexo-filter-github-emojis --save
```

在 Hexo 根目录下的 `_config.yml` 文件中，新增以下的配置项：

```yaml
githubEmojis:
  enable: true
  className: github-emoji
  inject: true
  styles:
  customEmojis:
```

### deploy 发布插件（可选安装）
如果你想通过`deploy`的方式进行推送`public文件夹`到托管网站，你需要安装
```bash
npm install hexo-deployer-git --save
```
当然你也可以选择不装，使用Github Actions、docker等方式


执行 `hexo clean && hexo g` 重新生成博客文件，然后在 `public` 文件夹中即可看到 `atom.xml` 文件，说明你已经安装成功了。

### 添加 [DaoVoice](http://www.daovoice.io/) 在线聊天功能（可选的）

前往 [DaoVoice](http://www.daovoice.io/) 官网注册并且获取 `app_id`，并将 `app_id` 填入主题的 `_config.yml` 文件中。

### 添加 [Tidio](https://www.tidio.com/) 在线聊天功能（可选的）

前往 [Tidio](https://www.tidio.com/) 官网注册并且获取 `Public Key`，并将 `Public Key` 填入主题的 `_config.yml` 文件中。

### 修改页脚

页脚信息可能需要做定制化修改，而且它不便于做成配置信息，所以可能需要你自己去再修改和加工。修改的地方在主题文件的 `/layout/_partial/footer.ejs` 文件中，包括站点、使用的主题、访问量等。

### 修改社交链接

在主题的 `_config.yml` 文件中，默认支持 `QQ`、`GitHub` 和邮箱等的配置，你可以在主题文件的 `/layout/_partial/social-link.ejs` 文件中，新增、修改你需要的社交链接地址，增加链接可参考如下代码：

```html
<% if (theme.socialLink.github) { %>
    <a href="<%= theme.socialLink.github %>" class="tooltipped" target="_blank" data-tooltip="访问我的GitHub" data-position="top" data-delay="50">
        <i class="fab fa-github"></i>
    </a>
<% } %>
```

其中，社交图标（如：`fa-github`）你可以在 [Font Awesome](https://fontawesome.com/icons) 中搜索找到。以下是常用社交图标的标识，供你参考：

- Facebook: `fab fa-facebook`
- Twitter: `fab fa-twitter`
- Google-plus: `fab fa-google-plus`
- Linkedin: `fab fa-linkedin`
- Tumblr: `fab fa-tumblr`
- Medium: `fab fa-medium`
- Slack: `fab fa-slack`
- Sina Weibo: `fab fa-weibo`
- Wechat: `fab fa-weixin`
- QQ: `fab fa-qq`
- Zhihu: `fab fa-zhihu`

> **注意**: 本主题中使用的 `Font Awesome` 版本为 `5.11.0`。

### 修改打赏的二维码图片

在主题文件的 `source/medias/reward` 文件中，你可以替换成你的的微信和支付宝的打赏二维码图片。

### 配置音乐播放器（可选的）

要支持音乐播放，就必须开启音乐的播放配置和音乐数据的文件。

首先，在你的博客 `source` 目录下的 `_data` 目录（没有的话就新建一个）中新建 `musics.json` 文件，文件内容如下所示：

```json
[{
	"name": "五月雨变奏电音",
	"artist": "AnimeVibe",
	"url": "http://xxx.com/music1.mp3",
	"cover": "http://xxx.com/music-cover1.png"
}, {
	"name": "Take me hand",
	"artist": "DAISHI DANCE,Cecile Corbel",
	"url": "/medias/music/music2.mp3",
	"cover": "/medias/music/cover2.png"
}, {
	"name": "Shape of You",
	"artist": "J.Fla",
	"url": "http://xxx.com/music3.mp3",
	"cover": "http://xxx.com/music-cover3.png"
}]
```

> **注**：以上 JSON 中的属性：`name`、`artist`、`url`、`cover` 分别表示音乐的名称、作者、音乐文件地址、音乐封面。

然后，在主题的 `_config.yml` 配置文件中激活配置即可：

```yaml
# 是否在首页显示音乐.
music:
  enable: true
  showTitle: false
  title: 听听音乐
  fixed: false # 是否开启吸底模式
  autoplay: false # 是否自动播放
  theme: '#42b983'
  loop: 'all' # 音频循环播放, 可选值: 'all', 'one', 'none'
  order: 'list' # 音频循环顺序, 可选值: 'list', 'random'
  preload: 'auto' # 预加载，可选值: 'none', 'metadata', 'auto'
  volume: 0.7 # 默认音量，请注意播放器会记忆用户设置，用户手动设置音量后默认音量即失效
  listFolded: false # 列表默认折叠
  listMaxHeight: # 列表最大高度
```

## 文章 Front-matter 介绍
1. **直接将`themes/halo/config/scaffolds`内所有内容拷贝到`Hexo`根目录下的`scaffolds`文件夹内替换**
2. 或者自己重新修改，修改请参考下面

### Front-matter 选项详解

`Front-matter` 选项中的所有内容均为**非必填**的。但我仍然建议至少填写 `title` 、`urlname` 和 `date` 的值。

| 配置选项   | 默认值                      | 描述                                                         |
| ---------- | --------------------------- | ------------------------------------------------------------ |
| title      | `Markdown` 的文件标题        | 文章标题，强烈建议填写此选项                                 |
| date       | 文件创建时的日期时间          | 发布时间，强烈建议填写此选项，且最好保证全局唯一             |
| author     | 根 `_config.yml` 中的 `author` | 文章作者                                                     |
| img        | `featureImages` 中的某个值   | 文章特征图，推荐使用图床(腾讯云、七牛云、又拍云等)来做图片的路径.如: `http://xxx.com/xxx.jpg` |
| top        | `true`                      | 推荐文章（文章是否置顶），如果 `top` 值为 `true`，则会作为首页推荐文章 |
| cover      | `false`                     | `v1.0.2`版本新增，表示该文章是否需要加入到首页轮播封面中 |
| coverImg   | 无                          | `v1.0.2`版本新增，表示该文章在首页轮播封面需要显示的图片路径，如果没有，则默认使用文章的特色图片 |
| password   | 无                          | 文章阅读密码，如果要对文章设置阅读验证密码的话，就可以设置 `password` 的值，该值必须是用 `SHA256` 加密后的密码，防止被他人识破。前提是在主题的 `config.yml` 中激活了 `verifyPassword` 选项 |
| urlname    | index                       | 文章访问路径，需要在`Hexo`根目录下`_config.yml`文件中使用`permalink: :urlname/`和`permalink_defaults:`<br>`urlname: index` |
| toc        | `true`                      | permalink_defaults:是否开启 TOC，可以针对某篇文章单独关闭 TOC 的功能。前提是在主题的 `config.yml` 中激活了 `toc` 选项 |
| mathjax    | `false`                     | urlname: index是否开启数学公式支持 ，本文章是否开启 `mathjax`，且需要在主题的 `_config.yml` 文件中也需要开启才行 |
| summary    | 无                          | 文章摘要，自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要 |
| categories | 无                          | 文章分类，本主题的分类表示宏观上大的分类，只建议一篇文章一个分类 |
| tags       | 无                          | 文章标签，一篇文章可以多个标签                              |
| keywords   | 文章标题                     | 文章关键字，SEO 时需要                              |
| reprintPolicy | cc_by                    | 文章转载规则， 可以是 cc_by, cc_by_nd, cc_by_sa, cc_by_nc, cc_by_nc_nd, cc_by_nc_sa, cc0, noreprint 或 pay 中的一个 |

> **注意**:
>
> 1. 如果 `img` 属性不填写的话，文章特色图会根据文章标题的 `hashcode` 的值取余，然后选取主题中对应的特色图片，从而达到让所有文章都的特色图**各有特色**。
> 2. `date` 的值尽量保证每篇文章是唯一的，因为本主题中 `Gitalk` 和 `Gitment` 识别 `id` 是通过 `date` 的值来作为唯一标识的。
> 3. 如果要对文章设置阅读验证密码的功能，不仅要在 Front-matter 中设置采用了 SHA256 加密的 password 的值，还需要在主题的 `_config.yml` 中激活了配置。有些在线的 SHA256 加密的地址，可供你使用：[开源中国在线工具](http://tool.oschina.net/encrypt?type=2)、[chahuo](http://encode.chahuo.com/)、[站长工具](http://tool.chinaz.com/tools/hash.aspx)。
> 4. 您可以在文章md文件的 front-matter 中指定 reprintPolicy 来给单个文章配置转载规则

以下为文章的 `Front-matter` 示例。

### post.md 最简示例

```yaml
---
title: hexo-theme-halo主题介绍
date: 2020-01-01 00:00:00
urlname:
---
```

### 最全示例

```yaml
---
title: hexo-theme-halo主题介绍
date: 2020-01-01 00:00:00
author: hongwei
img: /source/images/xxx.jpg
top: true
cover: true
coverImg: /images/1.jpg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: false
mathjax: false
summary: 这是你自定义的文章摘要内容，如果这个属性有值，文章卡片摘要就显示这段文字，否则程序会自动截取文章的部分内容作为摘要
categories: Markdown
tags:
  - Typora
  - Markdown
update:
urlname:
---
```
## 自定制修改

在本主题的 `_config.yml` 中可以修改部分自定义信息，有以下几个部分：

- 菜单
- 我的梦想
- 首页的音乐播放器和视频播放器配置
- 是否显示推荐文章名称和按钮配置
- `favicon` 和 `Logo`
- 个人信息
- TOC 目录
- 文章打赏信息
- 复制文章内容时追加版权信息
- MathJax
- 文章字数统计、阅读时长
- 点击页面的'爱心'效果
- 我的项目
- 我的技能
- 我的相册
- `Gitalk`、`Gitment`、`Valine` 和 `disqus` 评论配置
- [不蒜子统计](http://busuanzi.ibruce.info/)和谷歌分析（`Google Analytics`）
- 默认特色图的集合。当文章没有设置特色图时，本主题会根据文章标题的 `hashcode` 值取余，来选择展示对应的特色图

**我认为个人博客应该都有自己的风格和特色**。如果本主题中的诸多功能和主题色彩你不满意，可以在主题中自定义修改，很多更自由的功能和细节点的修改难以在主题的 `_config.yml` 中完成，需要修改源代码才来完成。以下列出了可能对你有用的地方：

### 修改主题颜色

在主题文件的 `/source/css/matery.css` 文件中，搜索 `.bg-color` 来修改背景颜色：

```css
/* 整体背景颜色，包括导航、移动端的导航、页尾、标签页等的背景颜色. */
.bg-color {
    background-image: linear-gradient(to right, #4cbf30 0%, #0f9d58 100%);
}

@-webkit-keyframes rainbow {
   /* 动态切换背景颜色. */
}

@keyframes rainbow {
    /* 动态切换背景颜色. */
}
```

### 修改 banner 图和文章特色图

你可以直接在 `/source/medias/banner` 文件夹中更换你喜欢的 `banner` 图片，主题代码中是每天动态切换一张，只需 `7` 张即可。如果你会 `JavaScript` 代码，可以修改成你自己喜欢切换逻辑，如：随机切换等，`banner` 切换的代码位置在 `/layout/_partial/bg-cover-content.ejs` 文件的 `<script></script>` 代码中：

```javascript
$('.bg-cover').css('background-image', 'url(/medias/banner/' + new Date().getDay() + '.jpg)');
```

在 `/source/medias/featureimages` 文件夹中默认有 24 张特色图片，你可以再增加或者减少，并需要在 `_config.yml` 做同步修改。


## 修改文章访问路径 urlname

在`Hexo`根目录`_config.yaml`中添加以下配置

```
# permalink: :year/:month/:day/:title/
permalink: :urlname/
permalink_defaults:
  urlname: index
```

## 全站 CDN
> CDN的全称是`Content Delivery Network`，即内容分发网络。CDN是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。——百度百科

放在Github的资源在国内加载速度比较慢，因此需要使用CDN加速来优化网站打开速度，jsDelivr + Github便是免费且好用的CDN，非常适合博客网站使用。

用法：
```http
https://cdn.jsdelivr.net/gh/你的用户名/你的仓库名@发布的版本号/文件路径

如：
http://cdn.jsdelivr.net/gh/hongweifuture/hongweifuture.github.io/medias/featureimages/12.jpg
```

注意：版本号不是必需的，是为了区分新旧资源，如果不使用版本号，将会直接引用最新资源，如果需要版本，请创建`releases`然后按照格式添加

当然最直接的办法就是使用 `username/username.github.io/`

# 常见问题
## 下载主题后不能预览
请根据主题配置文件选择需要安装的插件，或者在配置文件中关闭对应的功能