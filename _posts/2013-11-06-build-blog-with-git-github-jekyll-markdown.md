---
layout: post
title: "使用git+github+jekyll+markdown写博客"
description: ""
category: 
tags: [github]
---
{% include JB/setup %}

&emsp;&emsp;之前在网上看到很多网友使用github pages来搭建自己的博客，免费流量，免费托管，自由书写，随心所欲，禁不住也来尝试一下，希望自己以后能养成勤写、勤思考的习惯。下面就把这篇文章的诞生过程记录下来，作为我的第一篇博客，供大家参考。我是在win 7系统下完成整个过程的。

&emsp;&emsp;本文的结构如下：

### 一.基础环境的搭建
1.安装git

2.创建SSH

3.将ssh key添加到github

4.使用github pages建立博客

5.使用jekyllbootstrap简化博客创建

6.写博客

### 二.给博客添加评论模块

### 三.代码高亮显示

### 四.遇到的问题及解决方法

&emsp;&emsp;下面开始进入正题。

### 一.基础环境的搭建

1.安装git

下载git for windows工具（[地址](https://code.google.com/p/msysgit/downloads/list)），我下载的是git-1.8.4版本，然后安装。

2.创建SSH

启动git bash，输入如下命令：
{% highlight bash %}
$ cd ~/.ssh
{% endhighlight %}
如果之前没有创建过ssh key，则会提示“No such file or directory”，输入下面的命令生成新的key:
{% highlight bash %}
$ ssh-keygen -t rsa -C "邮件地址@youremail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):
{% endhighlight %}
我们按下回车即可。然后系统会要你输入加密串：
{% highlight bash %}
Enter passphrase (empty for no passphrase):<输入加密串>
Enter same passphrase again:<再次输入加密串>
{% endhighlight %}
输入完成后即可看到这样的提示，表示ssh key已经创建并保存完成：
{% highlight bash %}
Your identification has been saved in /your directory/.ssh/id_rsa.  
Your public key has been saved in /your directory/.ssh/id_rsa.pub.  
The key fingerprint is:  
01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db 邮件地址@youremail.com
{% endhighlight %}
在win 7系统中，默认情况下，是保存在C:\用户\你的用户名\.ssh目录下。

3.将ssh key添加到github

在本机设置SSH Key之后，需要添加到GitHub上，以完成SSH链接的设置。
用文本编辑工具打开id_rsa.pub文件，如果看不到这个文件，你需要设置显示隐藏文件。

登录github,在页面右上角点击"account setting"。

点击页面左侧菜单栏的SSH keys，进入ssh key管理页面，点击"Add ssh key"。将id_ras.pub中的内容复制到页面中的key栏里，点击"Add key"完成添加。

输入下面的命令测试一下：
{% highlight bash %}
$ ssh -T git@github.com
{% endhighlight %}

如果看到命令行提示：
{% highlight bash %}
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
{% endhighlight %}
只需输入"yes"即可，连接成功！

下面还需要设置用户的账号信息，Git会根据用户的名字和邮箱来记录提交。GitHub也是用这些信息来做权限的处理，输入下面的代码进行个人信息的设置，把名称和邮箱替换成你自己的，名字必须是你的真名，而不是GitHub的昵称。
{% highlight bash %}
$ git config --global user.name "你的名字"
$ git config --global user.email "your_email@youremail.com"
{% endhighlight %}

4.使用github pages建立博客

说实话，在写这篇博客的时候，我也只是刚刚接触github pages不久，虽然以前可能已经无意识的碰触过它。个人理解：github pages就是给托管在github上的项目生成页面展示的一个模板工具。我们都知道在github上托管了成千上万的项目，这其中包括大名鼎鼎的Twitter，jQuery，Ruby on Rais。为了大家更好的学习这些开源项目，必须对这些项目生成好的展示页面，包括功能简介、文档说明等等，github pages的主要目的也就在于此，并且提供了丰富的主题。由于它有这样的功能，因此也就被巧妙的用来搭建博客，因为博客也是一个静态的展示页面，只不过有自己独特的页面结构罢了。

github pages会查看github中特定版本库中的分支，进行解析，并最终组织成为一个静态网站。

github pages的使用有两种基本的方式：用户/组织页面，项目页面。
前者直接使用:username.github.io这个域名定位到你的特定版本仓库，并且会使用master分支；后者使用username.github.io/project的域名访问，并且使用特定的gh-pages分支。我们搭建博客使用的是第一种方式。

github pages使用Jekyll作为后台引擎。Jekyll将模板目录中的文件采用markdown、liquid转换器进行转换，并输出成完整的、适合在服务器上发布的静态站点。

到这一步，应该说所需要的环境已经基本搭建完成，我们在github respository管理页面右侧点击settings。

然后在Github Pages那一栏里点击Automatic Page Generator按钮即可自动生成一个github pages的模板供我们使用。


5.使用jekyllbootstrap简化博客创建

正如其名，jekyllbootstrap是一个已经帮我们做好基础配置工作，简化我们操作的工具。它提供了以下功能：

* 允许我们使用markdown来书写博客
* 使用git来管理一切
* 在终端里即可完成发布
* 一条命令即可更换主题
* 默认提供了对博客评论模块的支持

让我们来体验一下。

首先在github账户中创建一个版本仓库。并命名为USERNAME.github.com。

然后安装jekyllbootstrap:
{% highlight bash %}
$ git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com
cd USERNAME.github.com
$ git remote set-url origin git@github.com:USERNAME/USERNAME.github.com.git
$ git push origin master
{% endhighlight %}
完成后等待10分钟，即可在http://USERNAME.github.io上访问到自己的博客页面了。

为了更方便的在我们本地预览博客展示效果，我们需要在本地安装jekyll，由于它是用ruby来写成的，因此我们还需要安装ruby环境。下载rubyinstaller并安装，我本地安装的是rubyinstaller-1.9.3-p194版本。安装完成后需要配置环境变量，方便使用。

然后需要安装[Devkit](http://rubyinstaller.org/add-ons/devkit/)，Devkit是windows平台下编译和使用本地C/C++扩展包的工具，它是用来模拟Linux平台下的make,gcc,sh来进行编译。将下载下来的安装包解压到某个目录下，然后执行
{% highlight bash %}
$ ruby dk.rb init
$ ruby dk.rb install
{% endhighlight %}

在安装完ruby和Devkit后，jekyll的安装就十分简单了，首先将gem的源更换为淘宝的源，否则会安装失败（因为墙的原因，某些组件会访问网络失败）。
{% highlight bash %}
$ gem sources --remove https://rubygems.org/ 
$ gem sources -a http://ruby.taobao.org/
{% endhighlight %}

然后执行安装jekyll
{% highlight bash %}
$ gem install jekyll
{% endhighlight %}

等待片刻即可安装成功。安装成功后，执行
{% highlight bash %}
$ jekyll -version
{% endhighlight %}
来查看当前安装的jekyll版本。

安装完成后，我们在git bash中cd到刚才创建的USERNAME.github.com目录中，然后执行
{% highlight bash %}
$ jekyll server
{% endhighlight %}
即可在http://localhost:4000上访问我们的博客了。

还可以使用
{% highlight bash %}
$ jekyll server --watch
{% endhighlight %}
来实现自动更新。

jekyllbootstrap提供了很多[主题](http://themes.jekyllbootstrap.com/)可供我们选择，当我们选好一套主题后，点击"Install Theme"即可自动生成安装主题的代码，执行即可更换主题。例如我选择的是twitter的主题：
{% highlight bash %}
$ rake theme:install git="https://github.com/jekyllbootstrap/theme-twitter.git"
{% endhighlight %}

6.写博客
使用jekyllbootstrap来创建博客也只需要一条语句，只需指定博客名称即可。
{% highlight bash %}
$ rake post title="Hello World"
{% endhighlight %}
然后就会在_posts目录下面生成一个md格式的文件，使用markdown语言书写博客内容即可。默认的文件名为：日期-标题.md。

由于我们是用markdown来书写内容的，免不了要使用一款markdown编辑器来进行编辑，我曾经使用过MarkdownPad,觉得挺好用的，但是由于最近在学习使用Sublime，在网上搜索了一下，发现可以用它在加上markdown插件来进行markdown编辑以及浏览器端查看，我目前使用的插件是[Markdown-preview](https://github.com/revolunet/sublimetext-markdown-preview)

### 二.给博客添加评论模块
目前最流行的评论插件是disqus，我们需要去官网注册一个账号。我们可以在disqus的管理页面上进行一些个性化设置，可以修改默认的语言，颜色，排序方式等。

jekyllbootstrap本身已经提供了disqus支持，修改_config.yml文件，找到"comments"配置选项。
{% highlight xml %}
  # Settings for comments helper
  # Set 'provider' to the comment provider you want to use.
  # Set 'provider' to false to turn commenting off globally.
  #
  comments :
    provider : disqus
    disqus :
      short_name : happyroc
    livefyre :
      site_id : 123
    intensedebate :
      account : 123abc
    facebook :
      appid : 123
      num_posts: 5
      width: 580
      colorscheme: light
{% endhighlight %}

provider选项指定了默认的评论插件，我们使用的是disqus，上面的disqus中的short_name为你注册的用户名。

### 三.代码高亮显示
Jekyll原生支持语法高亮工具[Pygments](http://pygments.org/)。Pygments支持超过100种语言，是一款很棒的代码高亮工具！
1.首先我们需要修改_config.yml，设置pygments: true。
2.接着需要安装Python，因为Pygments是基于Python的。下载Python的安装包，安装完成后执行
{% highlight bash %}
python -V
{% endhighlight %}
如果能够正常显示Python的版本则表示安装成功。我安装的是python2.6版本。安装路径为C:/Python26
3.使用easy_install来安装Pygments。在[这里](https://pypi.python.org/pypi/setuptools)下载相应版本的setuptools，安装。并把路径 C:\Python26\Scripts添加到环境变量Path。然后在命令行中执行
{% highlight bash %}
easy_install Pygments
{% endhighlight %}
即可安装好Pygments。
4.在 Pygments demo 上根据不同语言找到自己喜欢的高亮方案，比如 friendly,在我们博客的工作目录下，执行如下命令
{% highlight bash %}
cd assets\themes\twitter\css
pygmentize -S friendly -f html > pygments.css
{% endhighlight %}
5.把生成的样式文件加到我们的网页中
修改_includes\themes\twitter\default.html，添加
{% highlight html %}
<link href="{{ ASSET_PATH }}/css/pygments.css" rel="stylesheet">
{% endhighlight %}

现在，只要把代码包围在

some code

就可以了。

### 四.遇到的问题及解决方法
jekyll默认情况下对中文的支持有些问题，需要我们修改一下编码。
编辑C:\Ruby193\lib\ruby\gems\1.9.1\gems\jekyll-1.4.0\lib\jekyll目录下的convertible.rb，将代码修改为：
{% highlight bash %}
self.content = File.read(File.join(base, name), :encoding => "utf-8");
{% endhighlight %}

另外，Jekyll默认的markdown解析器maruku对中文支持不够完善，所以果断换成RDiscount解析器，可以避免很多不必要的麻烦。
安装RDiscount解析器：
{% highlight bash %}
gem install rdiscount
{% endhighlight %}
设置RDiscount解析器：
在_config.yml文件中，添加一行：
{% highlight bash %}
markdown: rdiscount
{% endhighlight %}