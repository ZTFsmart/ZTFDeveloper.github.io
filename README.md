想搞个个人博客玩玩吗,看我的教程就够了,[我的博客](https://ztfsmart.github.io/)
#ubuntu下搭建Hexo+GitHub博客

#一、安装 Node.js和npm
>sudo apt-get update
>sudo apt-get install nodejs
>sudo apt-get install npm

如果报错,请更改软件源--[清华大学开源软件源](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/),并更新

查看nodejs和npm版本号
>nodejs -v
>npm -v

可以正常打印版本号说明,安装成功

#二、安装 Hexo
创建博客所在目录
>mkdir hexo

安装 Hexo

> 创建目录
>mkdir hexo
>切换目录
>cd hexo
>全局安装 Hexo，需要最高权限，记得输入root密码
>sudo npm install -g hexo-cli
>初始化 Hexo
>hexo init

如果报错执行代码,不报错忽略
>sudo npm config set user 0
>sudo npm config set unsafe-perm true
>sudo npm install -g hexo-cli

安装插件
>npm install hexo-generator-index --save
>npm install hexo-generator-archive --save
>npm install hexo-generator-category --save
>npm install hexo-generator-tag --save
>npm install hexo-server --save
>npm install hexo-deployer-git --save
>npm install hexo-deployer-heroku --save
>npm install hexo-deployer-rsync --save
>npm install hexo-deployer-openshift --save
>npm install hexo-renderer-marked --save
>npm install hexo-renderer-stylus --save
>npm install hexo-generator-feed --save
>npm install hexo-generator-sitemap --save

测试安装成功
>hexo server

![成功的提示](http://img.blog.csdn.net/20171027163740417?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1dGlhbmZ1NTIx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

浏览器输入 http://0.0.0.0:4000 可以访问到首页
![这里写图片描述](http://img.blog.csdn.net/20171027163800961?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1dGlhbmZ1NTIx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

修改Hexo配置文件hexo/_config.yml
>提示：key对应没有值的时候，冒号后面一定要有空格！否则会报错
>例如: timezone:会报错，timezone: 则不会。
>下边列出了常用的配置信息


站点信息设置
>title: ZTFDeveloper's Blog #站名
subtitle:  #副标题
description: 前进的开发者#站描述
author:  ZTF#作者
language: zh-CN #语言
timezone:



想把博客部署到自己的服务器,并通过IP或域名访问需要配置url
>url: http://blog.prozin.xyz
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:



 Deployment 这里设置了Git,mocorochio为你的github用户名
>deploy:
  type: git
  repo: git@github.com:mocorochio/micorochio.github.io.git
  branch: master


#三、Hexo 相关命令
新建文章(会在 hexo/source/_post 下生成对应.md 文件)
>hexo n “文章名称”

生成静态文件(位于 hexo/public 目录)
>hexo g

启动 Hexo 预览
>hexo s

提交部署(需要相关配置)
>hexo d

#四、部署到GitHub

##1.配置本机全局git环境（如果已经配置过请忽略）
首先请使用邮箱注册github账号，否则会影响下面操作，记住你注册的邮箱。
另外，请在ubuntu上设置你的git
>git config --global user.email "you@example.com"
>git config --global user.name "Your Name"


##2.生成SSH秘钥
先确定你的VPS 有没有生成过ssh的key，
验证

>less  ~/.ssh/id_rsa.pub


###**如果报错,执行下面代码**

 -C后面加你在github的用户名邮箱，这样公钥才会被github认可
 >ssh-keygen -t rsa -C example@163.com

 查看 公钥内容 稍后加入Github 账户的 sshkey中
 >less ~/.ssh/id_rsa.pub

你会看到一堆代码

###**如果没有报错**

 -C后面跟住你在github的用户名邮箱，这样公钥才会被github认可
 >ssh-keygen -t rsa -C example@163.com

 回车后，输入一个文件夹名字，存储新的SSH 秘钥
>.ssh/github

查看 公钥内容 稍后加入Github 账户的 sshkey中
>less ~/.ssh/id_rsa.pub


![这里写图片描述](http://img.blog.csdn.net/20171027170451454?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1dGlhbmZ1NTIx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##3.创建博客工程
>创建一个新项目，项目名称为 用户名.github.io ，比如我的Github用户名是mritd，则创建的项目名为 mritd.github.io
>用户名是你的github用户名哦！千万别弄错了，不然访问不到的！

![这里写图片描述](http://img.blog.csdn.net/20171027170816691?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1dGlhbmZ1NTIx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##4.将ssh秘钥添加到github中
![这里写图片描述](http://img.blog.csdn.net/20171027171423476?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1dGlhbmZ1NTIx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


#五、配置Hexo，编译与部署

还记得我们在_config.yml里最后一段的配置吗？根据刚才创建的GitHub项目填上repo

>deploy: 
  type: git
  repo: git@github.com:mocorochio/micorochio.github.io.git
  branch: master

最后一步，编译，上传静态代码

编译
>hexo generate

在主机的hexo目录下 执行以下命令将自动更新到Github
>hexo d

这时候就可以通过刚才设置的github访问博客了
比如我的就是:[https://ztfsmart.github.io/](https://ztfsmart.github.io/)
至此你的博客就完成了,但是你想弄点不一样的主题请看下边

#六、主题设置
选择你喜欢的[主题](https://hexo.io/themes/)
修改主题我另写一篇吧

[我的博客](https://ztfsmart.github.io/)
