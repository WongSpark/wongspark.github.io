---
layout: post
title: 使用Github Pages建立个人博客
categories: Tools
description: 一次使用Github pages搭建个人博客的实践
keywords: Github pages, 博客
---

随着Github的日渐流行，coder拥有自己的github已经成了普遍的一件事。而随着github pages的推出，coder在云端搭建自己的博客也成为了可能。下面是一次使用Github pages快速搭建个人博客的实践。以下实践过程适合有一定编程基础，或者对编程具有一定兴趣的朋友阅读，否则读来也是无益。

## 实践过程

### 申请github账号 [github](https://github.com/)

要想在github上搭建博客，首先需要注册github。关于github的好处，此处我便不细说了，这不是本文关注的重点。

### 打开[github pages](https://pages.github.com/)

参照github pages官网的教程，创建一个个人项目仓库，用来作为个人博客的云端载体。

如果觉得官网英文难以理解，可以跟随下面的中文教程：

1. 登录github页面，创建新仓库（New repository）

![OpenGrok Search and Browse](/images/posts/tools/setup-blog-using-github/1.png)

2. 填写项目库名称，github规定，按照username.github.io创建的项目仓库，可以自动生成一个github page，存放在https://username.github.io下。（如下图所示，因为我已经创建国博客，所以显示账户已经存在）

![OpenGrok Search and Browse](/images/posts/tools/setup-blog-using-github/2.png)

3. 仓库创建完成后，点击如图所示的Set up in Desktop（如果你预先跟随github pages的入门教程操作过一遍，那么你的电脑上应该已经安装了Github客户端），将项目同步到本地。

![OpenGrok Search and Browse](/images/posts/tools/setup-blog-using-github/3.png)

4. 此时，你已经拥有个空白的博客网站。接下来就需要微博客网站添加内容。网页可以自己手写，也可以colone他人的项目。

### 克隆他人页面

刚刚创建完成的个人博客除了一个简单的index和readme，什么也没有，与我们的想象相去甚远。别急，下面我们来实践一下克隆成品的个人博客模板。

[博客模板](https://github.com/mzlogin/mzlogin.github.io)

上面是我克隆的博客模板的地址，笔者将以此为例进行说明。（可以参照该项目的README来建站）

1. 克隆项目到本地

   将colone到本地的项目拷贝到你自己的本地项目下，如图，除了圈出的部分，其余全部拷贝

![OpenGrok Search and Browse](/images/posts/tools/setup-blog-using-github/4.png)

2. 修改域名

   如果你需要绑定自己的域名，那么修改 CNAME 文件的内容；如果不需要绑定自己的域名，那么删掉 CNAME 文件。

3. 修改配置

   网站的配置基本都集中在 _config.yml 文件中，将其中与个人信息相关的部分替换成你自己的，比如网站的 title、subtitle 和 Disqus 的用户名等。

   因为 Disqus 处理用户名与域名白名单的策略存在缺陷，请一定将 disqus.username 修改成你自己的。

4. 删除无用文件

   如下文件夹中除了 template.md 文件外，都可以全部删除，然后添加你自己的内容。

   * _posts 文件夹中是我已发布的博客文章。

   * _drafts 文件夹中是我尚未发布的博客文章。

   * _wiki 文件夹中是我已发布的 wiki 页面。

   * images 文件夹中是我的文章和页面里使用的图片。

5. 修改「关于」页面

   修改pages文件夹下的 `about.md` 文件
   pages/about.md 文件内容对应网站的「关于」页面，里面的内容多为个人相关，将它们替换成你自己的信息，包括 _data 目录下的 skills.yml 和 social.yml 文件里的数据。

### 注意
   在config.yml中，有几处修改需要尤其注意，如图所示的disqus username需要自己到https://disqus.com上去申请。

![OpenGrok Search and Browse](/images/posts/tools/setup-blog-using-github/5.png)

   同理，gitment评论插件也需要到对应网站去申请。申请完成后，记得建立存放评论内容的仓库（只需建立一个空仓库即可）

![OpenGrok Search and Browse](/images/posts/tools/setup-blog-using-github/6.png)

   页面发布后，你需要访问页面并使用你的 GitHub 账号登录（请确保你的账号是第二步所填 repo 的 owner），点击初始化按钮。之后其他用户即可在该页面发表评论。
   
![OpenGrok Search and Browse](/images/posts/tools/setup-blog-using-github/9.png)

![OpenGrok Search and Browse](/images/posts/tools/setup-blog-using-github/8.png)

![OpenGrok Search and Browse](/images/posts/tools/setup-blog-using-github/7.png)
