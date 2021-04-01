---
title: "jekyll+GitHubPages搭建个人博客"
date: 2019-07-15
categories: 博客
tags:
 - 静态网站
 - jekyll
---

## 配置环境

### 简介

[jekyll](https://jekyllrb.com/docs/)是一个简单的免费的，生成静态网页的工具，不需要数据库支持。但是可以配合第三方服务,例如Disqus。最关键的是jekyll可以免费部署在Github上，而且可以绑定自己的域名。jekyll让我们专注于文章本身，而不是html。
<!--more-->

```
提示：【配置环境】主要是为了在本地开发调试，比如当你使用别人的主题并做出了修改，可以立即在本地看到改变后的结果，当你发表博文时也可

以先在本地预览。但【配置环境】并不是必须的，因为Github Pages与jekyll深度结合，你可以将修改后的结果直接push到

username.github.io仓库，然后访问https://username.github.io,也可以看到你刚才修改过的结果，只是这样没有先本地预览，再push到

github快捷。对于新手来说配置环境会遇到很多问题，所以如果你遇到问题而又解决不了，不妨跳过配置环境的步骤，直接将修改push到github，

然后继续修改，继续push，直到自己满意为止，等你了解到一定程度再回过头配置环境，会更容易的解决问题，因为有很多人不是被编程本身难倒，

而是配置环境时过不去，毕竟万事开头难。
```

### 安装Ruby开发环境和jekyll

jekyll是由Ruby脚本语言编写的，所以在运行jekyll之前，需要先安装Ruby运行环境。

- 下载[RubyInstaller for Windows](https://rubyinstaller.org/downloads/),下载Ruby+Devkit的版本，减少后面报错的机会

![](http://image.xiaocaiji.site/blog/Snipaste_2020-06-15_09-23-24.jpg)

- 然后运行`ridk install`以添加必需的开发工具
- 打开cmd命令窗口，执行`gem install jekyll bundler`安装jekyll和bundler
- 查看是否正确安装了jekyll：`jekyll -v`

这里有一个jekyll官方提供的[Windows安装jekyll](https://jekyllrb.com/docs/installation/windows/)的过程，可供参考

### 生成本地站点并运行

- 切换到某个目录：在cmd命令行窗口中，用cd命令切换到一个目录，比如我切换到桌面（Logan是我的用户名）

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-15_10-59-01.jpg)

- 使用默认模板新建项目：使用`jekyll new 项目名`命令创建一个项目，比如我创建了一个名为blog的项目，创建好后是一个文件夹，位置在桌面

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-15_11-04-32.jpg)

​          这是文件夹里的内容

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-15_11-05-58.jpg)

- 生成静态站点：`cd blog`切换到blog目录下，运行`jekyll serve`生成静态站点，此时可能会报错，错误类型为`Bundler::GemNotFound`，比如我这里遇到的错误如下：

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-15_11-14-24.jpg)

根据提示，错误是缺少名为tzinfo的gem导致的（如果是缺少其他gem，解决方法是一样的，把gem名字换一下就行），使用`gem list`查看是否下载了此gem，如果查询结果里没有此gem，使用`gem install tzinfo`安装此gem

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-15_11-19-40.jpg)

安装成功后在此执行`jekyll serve`，我这里继续报错了，错误提示和上面的相同，打开blog目录下Gemfile文件

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-15_11-30-05.jpg)

这里的意思是，我们创建的blog项目需要依赖名为`tzinfo`的gem，且版本>=1.2,<2.0（`~>`的意义请看[这里](https://tosbourn.com/what-is-the-gemfile/)），而我们刚刚安装的版本为2.0版本，所以将Gemfile文件里的版本修改为我们下载的版本，下图为修后的内容

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-15_11-40-16.jpg)

保存之后，再次运行`jekyll serve`，得到下面结果，说明我们已经成功生成了站点

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-15_11-42-41.jpg)

我们打开blog文件夹，里面多了一个名为_site的文件夹，这里面就是jekyll帮我们生成的整个静态站点内容

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-15_11-44-41.jpg)

- 本地运行：在浏览器输入`http://127.0.0.1:4000/`,就可以看见jekyll生成的默认站点的内容了

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-15_11-49-15.jpg)

在配置环境的过程中，很有可能遇到其他问题，尝试复制错误信息，用搜索引擎搜索，看是否已有解决方法，如果你试了很多次还是不行，请直接在我的项目的[Issues](https://github.com/sslogan666/sslogan666.github.io/issues)里提问，或者按我开头说的提示走，跳过配置环境这一步，不要被配置环境难倒。

## 使用模板

### 获取模板

在前面，我们使用jekyll默认模板创建了一个静态站点，离我们心目中一个漂亮的博客差距还很大，对于我们新手来说，最好的选择是选择一个前辈写好的博客模板，然后加上我们自己的修改，变成我们自己心目中的博客，等我们完全了解了整个运行过程，这时候如果想自己重新写一个模板，会更容易，现在要做的是站在前辈的肩膀上。如果你喜欢我的博客的样子，可以先下载我的源码，再修改成你喜欢的样子。如果你使用过git可以用下面命令clone我的源码到你本地的仓库

```bash
git clone https://github.com/sslogan666/sslogan666.github.io.git
```

还可以直接[下载](https://codeload.github.com/sslogan666/sslogan666.github.io/zip/master)源码压缩包，之后解压到一个目录里。

### 本地运行博客

然后打开cmd命令行，用cd命令切换到解压项目目录，执行`jekyll serve`，此时很有可能会再次遇到`Bundler::GemNotFound`的错误，可以按照前面解决错误的方式尝试解决，还可以执行`bundle install`命令，此命令可以将Gemfile文件中列出的所有依赖的gem全部安装，然后再次执行`jekyll serve`命令，可能还会遇到`Gem::LoadError`，根据错误提示，执行`bundle exec jekyll serve`便不再报错。运行成功之后，在浏览器输入`http://127.0.0.1:4000/`便可以看到我的网站。

### 发布博客到github

**如果你没有把环境配置成功，不要气馁**，先在自己的github上创建一个`uername.github.io`名称的仓库，将username修改为你的github的用户名，然后将刚才下载解压后的项目文件夹名称改为`uername.github.io`，然后执行下面命令，将本地代码push到github,然后打开浏览器访问`https://username.github.io`，首次访问，可能需要10分钟左右后，才能访问到，喝杯茶，耐心等待😁

```bash
$ git add --all
$ git commit -m "my first post"
$ git push origin master
```

至此一个好看的博客实现了，所有人都可以用电脑、手机访问你发布博客，但博客的内容全部都是我的，下一步就要将这个博客修改为你自己的博客。

## 个性化

### jekyll目录结构

个性化就是要对文件内容作出修改，使得博客外观发生变化，在修改文件内容之前，必须对每个文件的作用有一点了解，这样才能有争对性的做出修改，而不是盲目试探。下面是我的项目的目录结构

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-16_10-58-29.jpg)

这是jekyll官方给出的项目目录结构，你可以点击[这里](https://jekyllrb.com/docs/structure/)查看更多信息

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-16_11-01-47.jpg)

我们的目录结构和官方给出的不是完全一样的，这很正常。下面说明一下几个重要文件及文件夹的作用（按照我的项目目录）

- **_posts**​：这个目录下存放的是所有的文章，后缀名为**.md**，这是用markdown语法写出来的文章，每个文件名都是`年-月-日-名字​`的格式
- **index.html**：博客的主页
- **_layouts**：存放了不同页面的布局，就是每个页面怎么显示，怎样排版，在每篇文章中只需引用对应的布局，文章就会按照规定的样式显示出来，这样一来我们就可以将重心放到文章内容上，而不再为文章最终的显示效果而操心，而且只需要写好一个布局，很多文章都可以重复利用，达到一样的显示效果，这极大提高了效率
- **_includes**：存放页面布局的各个部分，是对布局的进一步分割，将页面分割为页面头部、页脚、侧栏等等，为了重复利用
- **_site**：通过jekyll生成的整个项目的静态页面，在你刚下载的我的项目里是没有的，因为在.gitignore文件中加入了一条**_site/**，所以_site文件加下所有内容都不会提交到github仓库，所以你下载后也就没有这个文件夹，当执行`jekyll serve`之后会自动生成
- **_config.yml**：这个文件里有很多的配置信息

### 修改个人信息

打开_config.yml文件，修改**title**为你自己的名字或昵称，修改**subtitle**为博客的类型，比如我的博客主要记录学习笔记，所以我写了**学习/笔记**，修改**description**,修改**url**为你的地址，然后在命令行执行`jekyll serve`,你就看到博客主页显示了你的信息，哈哈哈……**如果你没有配置本地环境**，请直接push到github，然后访问https://username.github.io,也同样能看到你的信息，以后每次修改后都要执行**jekyll serve**,或push到github才能看到变化

### 修改头像

直接将images文件夹下的**avatar.jpg**图片替换成你的头像，再次访问时头像就变了

### 修改背景颜色

打开_config.yml文件，**cover_color**就是设置背景颜色的，你可以看到，我这里是**lightblue**,你还可以改成上面注释中的任意一个，然后看看效果。我当初下载这个模板时，背景颜色非常深，**lightblue**是我自定义的一种颜色，下面说说我自定义的过程

首先先访问你的博客，然后打开检查面板（我使用的是Chrome浏览器，单击右键，点击**检查**）

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-16_14-39-38.jpg)

此时可以看到页面代码，按Ctrl+f搜索，在搜索框中搜索**lightblue**,即我们使用的背景颜色名称，就会定位到对应的代码，点击定位到的css选择器名，即黄色部分，此时在下面显示出对应这个颜色的css代码

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-16_14-44-20.jpg)

现在让我们尝试改变颜色，在这里调试有个好处，你可以实时看到颜色的变化

![](http://image.xiaocaiji.site/blog/Honeycam%202019-07-16%2014-54-59.gif)

改变颜色，选一种自己喜欢的组合，将整个**.cover-lightblue**内容复制下来，从上上一张图中（红色圈出来的部分）我们可以看到，这段代码在min.css的627行，我们打开css/min.css文件，找到第627行，果然我们找到了和浏览器里一样的代码

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-16_15-03-14.jpg)

将刚才复制的我们改变颜色后的代码，复制到下面，重新起一个名字，注意`cover-`不能去掉，保存后，将_config.yml文件中**cover_color**设置成你起的名字，再次运行时背景颜色就变成你自定义的颜色了，很多修改，比如博客里的github链接修改为你自己的github，而不是跳转到我的github，都可以用类似的方法找到链接在文件中的位置，主要是要善用浏览器的调试功能，其他的修改请如法炮制

### 关于头像的效果

哈哈，小太阳头像配上旋转效果，我觉很美滋滋，我刚下载下来的模板头像效果是翻转的动画，我做了一点修改，具体涉及css3[动画](https://www.w3cschool.cn/css3/rvwu5flo.html)和[旋转](https://www.w3cschool.cn/css3/g5lhsflm.html)内容，其实很简单，打开css/min.css，翻到文件最后，找到旋转头像效果（有注释），具体代码如下

```css
/*旋转头像效果-start*/
@-webkit-keyframes z {
	from { -webkit-transform: translateX(0) translateY(0) translateZ(0) rotateX(0deg) rotateY(0deg) rotateZ(0deg) scaleX(1) scaleY(1) scaleZ(1); }
	
	to { -webkit-transform: translateX(0) translateY(0) translateZ(0) rotateX(0deg) rotateY(0deg) rotateZ(360deg) scaleX(1) scaleY(1) scaleZ(1); }
}
.rotate-x img{ 
	border-radius: 50%; }

.rotate-x img:hover { 
	border-radius: 50%;
	-webkit-animation: z 0.5s linear 0s infinite;  }
/*旋转头像效果-end*/
```

这里对源代码做了删减，多余的代码只是为了兼容不同的浏览器，这里没必要列出

- **@-webkit-keyframes z**：定义了一个名称为**z**的动画，**from**定义了动画的初始状态，**to**定义了动画的末态，很容易理解，动画是从from变换到to
- **-webkit-transform**：定义了一组对元素的转换
- **translateX() translateY() translateZ()**：设置沿三个轴的平移量，0即为不平移
- **rotateX() rotateY() rotateZ()**：设置绕三个轴的旋转度数
- **scaleX() scaleY() scaleZ()**：设置沿三个轴的缩放

所以整个动画的意义就是，将对象沿Z轴旋转360度，即一圈，在三维坐标中，Z轴就是指向我们的轴

- **border-radius: 50%**：规定了图片以圆形显示，图片本来是方形的
- **-webkit-animation: z 0.5s linear 0s infinite**：将我们创建的名称为z的动画绑定到选择器**.rotate-x img:hover**上，

后面的几个参数意义分别是：z（动画名称）、0.5s（完成一次动画的周期是0.5秒，即0.5s转一圈）、linear（规定动画的速度曲线，linear是指匀速，可查看[更多速度曲线](https://www.w3cschool.cn/cssref/css3-pr-animation-timing-function.html)）、0s（直接开始，不延迟）、infinite（播放次数，infinite是无限循环）

- **.rotate-x img:hover**：将动画绑定到类名为rotate-x下的图片上（在本项目里即为头像），且鼠标放在上面时才开始播放动画，所以最终当鼠标移动到头像上时，头像就会转动

你也可以发挥想象，自己做一个头像的效果，可以参考W3C上对[CSS](https://www.w3cschool.cn/css3/)的介绍

### 图片问题

- 当你尝试直接替换images文件夹下背景图片时，你会发现背景图片并没有变化
- 当你清空浏览器缓存，重新访问我博客时，你会发现头像比背景图片加载的慢

原因是我将背景图片放到了[图床](https://baike.baidu.com/item/%E5%9B%BE%E5%BA%8A/10721348?fr=aladdin)上，文件夹里的背景图片并没有起作用，所以直接替换是没用的，为什么要用到图床呢？

其一是提高图片加载速度，其二是为了方便文章中插入图片。

方便文章中插入图片是因为你将本地写的文章push到github上时得使用相对路径，用绝对路径就得做出修改，比较麻烦，使用图床，不管本地还是push之后，链接不用更改，只要图床服务器不出问题，你能访问互联网，图片就可以加载出来，图床的使用网上有很多的[教程](https://www.cnblogs.com/ssgeek/p/10854839.html)，这里就不细说了

如果你不暂时不想使用图床，也没问题，打开_includes/side-panel.html，将第一行后面的background-image:url里的链接改为**images/background-cover.jpg**，图片替换为你自己的背景图片，再次访问后你也可以看到背景图片变化了

如果你已经使用了图床，也只需改变背景图片的链接即可

### 域名

如果你不想使用`username.github.io`来访问你的网站，你首先得注册一个域名，注册域名网上有很多教程，这里就不细说了，域名备案审核通过之后，加一条解析记录，记录类型为**CNAME**，即将你的域名指向另一个域名，将解析结果指向**username.github.io**，然后在项目根目录下新建一个名为CNAME的文件，里面写上你申请的域名，比如我的CNAME的内容是www.xiaocaiji.site，然后就可以通过http://www.xiaocaiji.site来访问我的博客了

## 文章是怎样分类显示的

下图是我的分类显示博文的页面，可以看到这个页面是根目录下的tags页面，页面主要有两大部分，上半部分是所有文章的一个分类，以及每个分类下文章的数量；下面是每个分类下所有文章的列表

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-17_10-41-24.jpg)

我们打开/tags.html文件，内容如下

![](http://image.xiaocaiji.site/blog/Snipaste_2019-07-17_10-51-10.jpg)

第一次打开这个文件，很可能看不懂，html代码里面夹杂着其他的代码，这些用大括号和百分号组成的代码，其实是Liquid语言，要看懂这段代码，你首先得了解一下这种语言，这里有[中文文档](https://liquid.bootcss.com/basics/introduction/)，这里是[英文文档](https://shopify.github.io/liquid/basics/introduction/)，其实很简单，只需要不到一个小时，你就能懂基础的语法了

**首先来看博文目录是怎样生成的**

- **{% raw %}{% assign tags_list = site.tags %}{% endraw %}**：声明了一个tag_list变量，内容是**site.tags**，**site.tags**其实是jekyll中的一个变量，那么到底是什么内容呢？内容如下

```
{"博客"=>[#, #], "笔记"=>[#]}
```

**博客**和**笔记**是文章的分类，说明现在有两种文章类型，一种是博客，另一种是笔记，博客下面有两篇文章（两个**#**），笔记下面有一篇文章（一个**#**）

- **{% raw %}{% include JB/tags_list %}{% endraw %}**：即插入了_includes/JB/tags_list文件，我们打开这个文件，主要起作用的代码如下：

```
{% raw %}{% for tag in tags_list %} 
    <li>
        <a href="{{ BASE_PATH }}{{ site.JB.tags_path }}#{{ tag[0] }}-ref">
        {{ tag[0] }} <span>{{ tag[1].size }}</span>
        </a>
    </li>
{% endfor %}{% endraw %}
```

- **{% raw %}{% for tag in tags_list %}{% endraw %}**：循环遍历tag_list里的每一条内容， tag_list里的内容就是上面的大括号里内容，所以tag就是某一条内容，比如第一次循环，tag就是`"博客"=>[#, #]`,那么很自然tag[0]就是分类名，即`博客`，tag[1].size就是属于博客类型的文章的个数，即`2`条，所以最终你看到的结果就是`分类+文章数目`

**再来看博文标题列表是怎样生成的**

经过上面的分析，我们知道tag[1]是属于tag标签下所有文章的集合，那么只要循环遍历每个文章分类（tag）和分类下的文章（tag[1]），就可以输出每篇文章的标题了，循环每个tag的代码在上面的/tags.html的博文列表那一段，循环某个tag下所有文章的代码在_includes/JB/pages_list中，其中主要起作用的内容如下

```
{% raw %}{% for node in pages_list %}
    <li><a href="{{ BASE_PATH }}{{node.url}}">{{node.title}}</a></li>
{% endfor %}{% endraw %}
```

## 怎样写一篇博文

你只需要在你的markdown文章前加上[头信息](http://jekyllcn.com/docs/frontmatter/)，就可以被jekyll处理成可以访问的页面，头信息格式如下：

```
---
layout : post
title: "jelyll搭建个人博客3"
date: 2019-07-17
tag: 博客
---
```

三个短横线之内的就是头部信息，layout是文章最后套用的模板,与文章显示的样式有关，title就是博文列表里显示的文章标题，如果为空，则最后会显示文件名，tag就是文章分类显示时的类别，`注意冒号后均有一个空格`