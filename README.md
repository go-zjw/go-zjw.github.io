---
title: hexo+GitHub 搭建个人博客
---
  现在市面上的博客很多，如CSDN，博客园，简书等平台，可以直接在上面发表，用户交互做的好，写的文章百度也能搜索的到。缺点是比较不自由，会受到平台的各种限制和恶心的广告。

  而自己购买域名和服务器，搭建博客的成本实在是太高了，不光是说这些购买成本，单单是花力气去自己搭这么一个网站，还要定期的维护它，对于我们大多数人来说，实在是没有这样的精力和时间。直接在GitHub Pages平台上托管我们的博客。这样就可以安心的来写作，又不需要定期维护，而且hexo作为一个快速简洁的博客框架，用它来搭建博客真的非常容易。

# hexo搭建
## hexo简介
hexo是一款基于Node.js的静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在GitHub和Coding上，是搭建博客的首选框架。大家可以进入hexo官网进行详细查看，因为Hexo的创建者是台湾人，对中文的支持很友好，可以选择中文进行查看。
## 安装Git
Git是目前世界上最先进的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。也就是用来管理你的hexo博客文章，上传到GitHub的工具。Git非常强大，我觉得建议每个人都去了解一下。廖雪峰老师的Git教程写的非常好，大家可以了解一下。 [git教程]: https://www.liaoxuefeng.com/wiki/896043488029600
windows：到git官网上下载,Download git,下载后会有一个Git Bash的命令行工具，以后就用这个工具来使用git。
顺便说一下，windows在git安装完后，就可以直接使用git bash来敲命令行了，不用自带的cmd，cmd有点难用。

linux：对linux来说实在是太简单了，因为最早的git就是在linux上编写的，只需要一行代码
``` bash
sudo apt-get install git
``` 
安装好后，用git --version 来查看一下版本

## 安装Node.js
Hexo是基于Node.js编写的，所以需要安装一下Node.js和里面的npm工具。

windows：Node.js选择LTS版本就行了。
1.Node.js 官方网站下载：https://nodejs.org/en/download/
2.双击打开安装包，下一步下一步即可（笔者安装路径为“D:\Program Files\nodejs”）：
3.安装成功
linux：

``` bash
sudo apt-get install nodejs
sudo apt-get install npm
```
安装完后，打开命令行
``` bash
node -v
npm -v
```
检查一下有没有安装成功


## 安装hexo
前面git和Node.js安装好后，就可以安装hexo了，你可以先创建一个文件夹blog，然后命令行cd（cd不行的用cd /d）到这个文件夹下（或者在这个文件夹下直接右键git bash打开）。

输入命令
``` bash
npm install -g hexo-cli
```
依旧用hexo -v查看一下版本

至此就全部安装完了。

接下来初始化一下hexo
``` bash
hexo init myblog
```

这个myblog可以自己取什么名字都行，然后
``` bash
cd myblog #进入这个myblog文件夹
npm install
```
新建完成后，指定文件夹目录下有：

node_modules: 依赖包
public：存放生成的页面
scaffolds：生成文章的一些模板
source：用来存放你的文章
themes：主题
_config.yml: 博客的配置文件
……
``` bash
hexo g
hexo server
```
打开hexo的服务，在浏览器输入localhost:4000就可以看到你生成的博客了。


命令行使用ctrl+c可以把服务关掉。

## GitHub创建个人仓库
首先，你先要有一个GitHub账户，去注册一个吧。

注册完登录后，在GitHub.com中看到一个New repository，新建仓库


创建一个和你用户名相同的仓库，后面加.github.io，只有这样，将来要部署到GitHub page的时候，才会被识别，也就是xxxx.github.io，其中xxx就是你注册GitHub的用户名。
记得选择Public。



点击create repository。

## 生成SSH添加到GitHub
回到你的git bash中，
``` bash
git config --global user.name "yourname"
git config --global user.email "youremail"
```
这里的yourname输入你的GitHub用户名，youremail输入你GitHub的邮箱地址。这样GitHub才能知道你是不是对应它的账户。

可以用以下两条，检查一下你有没有输对
``` bash
git config user.name
git config user.email
```
然后创建SSH,一路回车
``` bash
ssh-keygen -t rsa -C "youremail"#youremail是你的GitHub邮箱地址
```
这个时候它会告诉你已经生成了.ssh的文件夹。一般会在用户文件夹里。



ssh，简单来讲，就是一个秘钥，其中，id_rsa是你这台电脑的私人秘钥，不能给别人看的，id_rsa.pub是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。

而后在GitHub的setting（用户设置）中，找到SSH and GPG keys的设置选项，点击New SSH key
把你的id_rsa.pub里面的信息复制进去。



在gitbash中，查看是否成功
``` bash
ssh -T git@github.com
```
## 将hexo部署到GitHub
这一步，我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开站点配置文件 _config.yml，翻到最后，修改为
``` bash
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git#填上刚刚创建的github个人仓库地址
  branch: master
```
这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。
``` bash
npm install hexo-deployer-git --save
```
然后
``` bash
hexo clean
hexo generate
hexo deploy
```
其中hexo clean清除了缓存，也可以不加。
hexo generate 顾名思义，生成博客静态文件，可以用 hexo g缩写
hexo deploy 部署文件到github，可以用hexo d缩写

注意deploy时可能要你输入github账户和密码。

过一会儿就可以在http://yourname.github.io 这个网站看到你的博客了！！


## 设置个人域名
现在你的个人网站的地址是 yourname.github.io，如果觉得这个网址逼格不太够，这就需要你设置个人域名了。但是需要花钱。

注册一个阿里云账户,在阿里云上买一个域名，我买的是 fangzh.top，各个后缀的价格不太一样，比如最广泛的.com就比较贵，看个人喜好咯。

你需要先去进行实名认证,然后在域名控制台中，看到你购买的域名。

点解析进去，添加解析。

不要企图通过ping的方式，来找出所有的IP地址。因为你的网站的IP是会变化的哦。所以你需要分别添加这四个解析到IP地址的A记录。
主机记录	@
记录类型    A
线路类型	默认
记录值   185.199.108.153   185.199.109.153   185.199.110.153   185.199.111.153
然后再添加一个CNAME解析。
主机记录	www
记录类型   CNAME
线路类型	默认
记录值  yourname.github.io



然后在你的博客文件source中创建一个名为CNAME文件，不要后缀。写上你的域名。



## 发布博客
在git bash中，输入
``` bash
hexo clean
hexo g
hexo d
```
过不了多久，再打开你的浏览器，输入你自己的域名，就可以看到搭建的网站啦！

接下来你就可以正式开始写文章了。
``` bash
hexo new newpapername#填上你文章的标题
```
然后在source/_post中打开markdown文件，就可以开始编辑了。附上markdown常规语法：https://www.jianshu.com/p/191d1e21f7ed/。当你写完的时候，再
``` bash。
hexo clean
hexo g
hexo d
``` 

就可以看到更新了。


