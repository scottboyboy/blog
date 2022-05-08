
---
title: 关于配置hexo-yilia主题的若干个简单问题
date: 2016-03-27 13:56:35
tags: [hexo] 
---

## 1. 从github上clone主题
*附上地址链接*：[github](https://github.com/litten/hexo-theme-yilia)
--- 
**我有哪几种方法clone主题：**
<!-- more -->
**最暴力**的方法：直接 Download Zip
**最优雅**的方法：在你的本地hexo文件夹中右键单击，Git Bash Here，

    $ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
**稍间接**的方法：点击 [github](https://github.com/litten/hexo-theme-yilia)  

> github->fork->打开桌面版github->左上角 +      号->clone->选择文件夹->OK!


---
## 2. 我必须要需要修改什么？
> 修改hexo跟目录下的_config.yml: 66行左右
        
        theme: yilia
        
---
## 3. 我可以选择修改什么？
> language，接上条，在10行左右，可添加

     language: zh-Hans
     
---
> 多说评论，在 yilia 根目录下的 _config.yml：44行左右，改为你的多说id，如域名xxx.duoshuo.com，改为

    duoshuo: xxx
    
---

> 个人链接，这个自己可以登陆对应的网站，找到对应的网址贴上。

---
> 友情链接，同上，想添加xxx直接贴上。

---
> aboutme，同上，又是一道送分题。

---
> 头像，先把自己的图片复制到 yilia 根目录下的source/img 里，修改文件名为 avatar.png
    在 yilia 根目录下的 _config.yml：38行左右

    avatar: /img/avatar.png
## 4. 注意点什么？
> 送分题。拼写，空格，引号，true/false。

---

> * 以上clone一个简单的blog已经够了，内容可能简单、存在错误，欢迎留言讨论。