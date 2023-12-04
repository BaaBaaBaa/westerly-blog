---
title: hexo博客搭建超详细过程
typora-root-url: hexoBuild
date: 2023-11-24 16:02:31
tags:
---

# 写在前面

从我入行以来，一直想搭建一个自己博客来记录自己的想法和遇到的问题。最近失业以后时间多了起来，花了大概一周的时间，把这个博客搭起来了，于是想写一篇文章来记录搭建的详细流程作为本站的一篇文章，这对于我而言也是一个新的开始！



# 准备

搭建之前，我们需要提前有以下准备：

- 安装了node环境
- 注册了github账号
- 安装了git
- 购买了自己的域名（最好有）	
- 安装了markdown编辑器（我使用的是typora）



# Hexo初体验

Hexo是可以一个解析md文件，生成静态网页的博客框架，支持多种主题，而且支持部署到github page。我们先来体验一下。



在cmd中输入命令全局安装Hexo

```shell
npm install -g hexo-cli
```

安装完成后，输入命令

```shell
hexo -v
```

如果跟下图一样出现版本号，表示安装成功。

![image-20231124162529375](./image-20231124162529375.png)



在我们的工作目录打开cmd，执行`hexo init`创建项目

```shell
hexo init hexo-blog-demo(项目名)
```

![image-20231124200447068](./image-20231124200447068.png)

接下来进入到该项目下，较新版本的 hexo 在执行 `hexo init` 时会自行安装好依赖，因此不需要再额外执行`npm i`

```shell
hexo s
或者
hexo serve
```

打开 http://localhost:4000 后，就会看到你的Hexo博客跑起来了！

> 更多命令可以 [点击查看](https://hexo.io/zh-cn/docs/commands)。

# github page配置

Hexo有个命令`hexo g`或者`hexo genarate`，会直接遍历项目中的静态文件来生成静态网页。将这些静态网页挂载到github page就可以直接访问，而不需要另外的服务器了。接下来我们来配置一下。

## 创建仓库

访问github，new 一个 repository。

> 科学上网的重要性，包括后续有可能出现ssh连接超时等问题时，都可以试试挂个代理

注意仓库名一定要是`[用户名].github.io`，一定要这个格式，一定要是这个格式，一定要是这个格式，重要的事情说三遍！

然后仓库一定要是`pubilc`，仓库一定要是`pubilc`，仓库一定要是`pubilc`，这个也说三遍！

![image-20231124202636255](./image-20231124202636255.png)

## 挂载到github page

首先安装 hexo-deployer-git

```shell
npm install hexo-deployer-git --save
```

然后找到项目_config.yml文件，在最下面添加以下代码，代码中的【用户名】填你自己的github用户名

```yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repository: git@github.com:【用户名】/【用户名】.github.io.git
  branch: main
```

> 这个文件是Hexo的配置文件，更多详细的配置可以 [点击查看](https://hexo.io/zh-cn/docs/configuration)。

修改保存以后，直接部署到github试一下吧

```shell
hexo generate // 生成静态文件
hexo deploy // 部署到github
```

第二个命令执行完成出现`Deploy done`字样时说明成功了。等待一会，再进入https://【用户名】.github.io就可以看到博客已经挂载成功了。

> 拓展：其实到这会发现，github上管理的是hexo g生成的静态文件，类似dist。而我们hexo init新建的项目没有通过github管理，是手动hexo generate &&  hexo deploy部署上去的。所以我们可以再新建一个项目存放hexo新建的项目，并通过CICD来执行前面的指令，这样就只需要提交代码就能自动生成静态文件并部署到github page。

# Vercel部署

Vercel是一个云服务平台，特别适合我们Hexo项目部署，还能提供全球CDN加速。

Vercel自带CICD，授权github项目给Vercel后，我们使用`hexo deploy`以后提交到github，Vercel就会帮我们自动部署，操作简单，可谓是非常之优雅。

![优雅](./优雅.webp)

[点击这里进入Vercel](https://vercel.com/)，注册账号并登录。

![image-20231125234021997](./image-20231125234021997.png)

使用github账号关联，在新建项目的时候可以直接托管该github账号的项目。

## 导入项目

接下来创建项目并关联对应的项目，并关联【用户名】.github.io项目

![image-20231125234253370](./image-20231125234253370.png)



![image-20231125235505317](./image-20231125235505317.png)

![image-20231125235841531](./image-20231125235841531.png)

点击deploy，等待一会，出现下面的界面。

![image-20231126000012845](./image-20231126000012845.png)

## 绑定域名

接下来配置到自己的域名，点击domains配置。

![image-20231126000157214](./image-20231126000157214.png)

输入购买的域名点击add。

![image-20231126000721235](./image-20231126000721235.png)

提示我们在购买域名的云平台去添加解析记录。

![image-20231126000829839](./image-20231126000829839.png)

我使用的是阿里云上购买的域名，接下来进入阿里云的`域名解析`。

![image-20231126001315843](./image-20231126001315843.png)

按照要求vercel上的填写两条记录，填写完成后即可。

> 添加完成以后，可以使用阿里云的dns检测工具，其他的要做的就是等待了。

# butterfly主题应用

Hexo默认的样式并不好看，接下来换个主题。我使用的是[butterfly](https://github.com/jerryc127/hexo-theme-butterfly)。更多主题可以[点击查看](https://hexo.io/themes/)。

1.在项目目录执行

```shell
npm i hexo-theme-butterfly // butterfly主题
npm install hexo-renderer-pug hexo-renderer-stylus --save // butterfly主题需要的依赖包
```

2.修改`_config.yml`

```yaml
theme: butterfly
```

3.将`node_modules`中的`_config.yml`复制出来到项目的根目录，并重命名为`_config.butterfly.yml`方便后续魔改。`_config.butterfly.yml`生效的优先级会高于`_config.yml`

![image-20231124221147987](./image-20231124221147987.png)

# 额外的插件

应用了一些插件，修改了一些样式和配置，这里就不展开讲啦。

Live2d看板娘：https://github.com/EYHN/hexo-helper-live2d/blob/HEAD/README.zh-CN.md

首页分类：https://zfe.space/post/hexo-magnet.html

首页轮播图：https://github.com/Akilarlxh/hexo-butterfly-swiper

# 写文章

至此，整个博客就完成了，可以写文章了。

文章通过`hexo new`生成

```shell
$ hexo new [layout] <title>
```

![image-20231124223544444](./image-20231124223544444.png)

执行命令后会在`source/posts`目录下生成一个单独的md文件。

## Front-matter

Front-matter 是md文件最上方以 `---` 分隔的区域，用于指定个别文件的变量，以下是官网罗列出的参数。

| 参数              | 描述                                                         | 默认值                                                       |
| :---------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `layout`          | 布局                                                         | [`config.default_layout`](https://hexo.io/zh-cn/docs/configuration#文章) |
| `title`           | 标题                                                         | 文章的文件名                                                 |
| `date`            | 建立日期                                                     | 文件建立日期                                                 |
| `updated`         | 更新日期                                                     | 文件更新日期                                                 |
| `comments`        | 开启文章的评论功能                                           | `true`                                                       |
| `tags`            | 标签（不适用于分页）                                         |                                                              |
| `categories`      | 分类（不适用于分页）                                         |                                                              |
| `permalink`       | 覆盖文章的永久链接，永久链接应该以 `/` 或 `.html` 结尾       | `null`                                                       |
| `excerpt`         | 纯文本的页面摘要。使用 [该插件](https://hexo.io/zh-cn/docs/tag-plugins#文章摘要和截断) 来格式化文本 |                                                              |
| `disableNunjucks` | 启用时禁用 Nunjucks 标签 `{{ }}`/`{% %}` 和 [标签插件](https://hexo.io/zh-cn/docs/tag-plugins) 的渲染功能 | false                                                        |
| `lang`            | 设置语言以覆盖 [自动检测](https://hexo.io/zh-cn/docs/internationalization#路径) | 继承自 `_config.yml`                                         |
| `published`       | 文章是否发布                                                 | 对于 `_posts` 下的文章为 `true`，对于 `_draft` 下的文章为 `false` |

这些参数在不同的主题，有不一样的体现，甚至不同的主题有自己的参数。

## 引入图片

source目录是存放资源的地方，引用图片也是直接以这个目录作为根目录。我们一般会在source目录下新建一个images目录用于单独存放图片来引用。但是文章内引用的图片很多，归类混乱不易维护。

官方提供了一个方案如下图，算了你还是别看官方解释了，我给你翻译翻译，就是post_asset_folder配置打开以后，新建文章时，会同时在文章的存放目录生成一个同名的文件夹，用来存放资源。

![image-20231124224809423](./image-20231124224809423.png)

咋一看好像没什么用，但是结合typora的话，就有很大的便利了。

我们在_config.yml`文件中修改以下配置

```
post_asset_folder: true
marked:
  prependRoot: true
  postAsset: true
```

再在typora中修改图片复制的设置

![image-20231124225356273](./image-20231124225356273.png)

再到`scaffolds`目录下的`post.md`添加一条`Front-matter`。

ps：这个`post.md`是创建文章时的模板，写入的内容会出现在每一篇新创建的文章里。

```md
typora-root-url: {{ title }}
```

![image-20231124230353479](./image-20231124230353479.png)

完成这三步后，在用typora写文章时，复制图片（包括截图工具截取、从网页复制或者直接复制本地文件）会直接复制到当前编辑的这个md文件同级的文件夹下，而且也不需要手动修改图片路径就能在网站正常显示，非常之舒服。

# 说在最后

感谢能看到这里，本博客会持续优化，内容也会继续更新。
