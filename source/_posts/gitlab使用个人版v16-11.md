---

title: gitlab使用个人版v16.11

date: 2016-11-13 20:53:00

tags: [gitlab]

---

## 安装gitbash


**附上地址链接**：[git](https://git-scm.com/downloads)

<!-- more -->

## 配置gitlab和github


**同时**使用github和gitlab，引发了此问题，所以需要再次清理旧的配置。从新开始：


* 打开git bash，在你的用户目录，生成ssh钥匙对，并且`指定`文件名为id_rsa_gitlab

   ```
   cd ~/
   ssh-keygen -t rsa -f ~/.ssh/id_rsa_gitlab -C "你的邮箱"
   ```

* 可以重复上面步骤，给github账户也生成钥匙对，注意文件名不要与上面的冲突，`复制ssh`到你的gitlab或者github账户

    > 此处是`公钥`，一定注意


* 添加私钥，不过`不太明白`这里

   大概是一个全局变量，省去了每次输入密钥。

   ```
    ssh-add ~/.ssh/id_rsa_gitlab
    ssh-add ~/.ssh/id_rsa
   ```

* 有时会出新问题：

  ```
  Could not open a connection to your authentication agent.
  ```

  此问题是需要ssh-agent 启动bash。如下操作即可。。

  ```
  ssh-agent bash --login -i
  ```


* 新建一个config文件

    ```
    touch config
    ```

* 内容如下

   ```
    #gitlab
    Host gitlab.com

        HostName gitlab.com

        PreferredAuthentications publickey

        IdentityFile ~/.ssh/id_rsa_gitlab

    #github
    Host github.com

        HostName github.com

        PreferredAuthentications publickey

        IdentityFile ~/.ssh/id_rsa
   ```

* 测试是否OK

   ```
   ssh -T git@gitlab.com
   ssh -t git@gitlab.com
   ```

     > 提示之一：Welcome to GitLab, Shangzhao Ma!


`说明一切OK`


## 使用

* 先clone到本地，再初始化

    ```
    git clone git@gitlab.com:shiwk/WechatAttendanceSystem.git
    git init
    ```

* 新建个文件，写点内容试试

    ```
    touch README.md
    vim README.md
    ```

* 准备提交，写点commit

    ```
    git add README.md
    git commit -m "add README"
    ```

* push到具体的分支，此处是`master`
    ```
    git push -u origin master
    ```


- - - -
在使用的过程中又出现了新问题，或者类似的问题，就是有小号的github在同一台电脑，怎么处理？
解决方案类似，用户目录下的.ssh目录内的 `config` 文件，发挥作用。

```
#github
Host github.com
	HostName github.com
	PreferredAuthentications publickey
	IdentityFile ~/.ssh/id_rsa_github
#github_scottboy,Host后面的名字随便，HostName后面的不可以
Host othergithub
	HostName github.com
	PreferredAuthentications publickey
	IdentityFile ~/.ssh/scottboy_github
```

然后按照上面的ssh-add ~/.ssh/scottboy_github，
接下来测试是否设置成功：

```
ssh -T git@othergithub  #可以这样玩的 
```

然后 

```
git remote add origin git@othergithub:asdjflajfds/xx-project.git 
```

具体结构：

```
git remote add origin git@config里面规定的Host:你的github账户名/你的项目名.git
```

其中遇到了一点小问题：当你添加地址的时候不小心拼错了，可以

```
git remote rm origin
```

然后重新添加。

--------------
## 其他方法


* 查看本地分支

```
git branch
```


* 查看远端所有分支

```
git branch -r
```

* 创建新分支
```
git checkout -b msz
```
> Switched to a new branch 'msz'



* 切换分支到 master

```
git checkout master
```

* push到远端的分支 msz

```
git push -u origin msz
```

* 查看所有分支，包括本地和远端

```
git branch -a
```

* 删除本地分支 msz

```
git branch -d msz
```

    > Deleted branch msz (was 83e06c5).



* 删除远端分支 msz

    ```
    git branch -r -d origin/msz
    ```

    > Deleted remote-tracking branch origin/msz (was 83e06c5).


* 非常十分肯定的删除，即用空代替 msz

    ```
    git push origin :msz
    ```


**参考1** [github/gitlab同时管理多个ssh key](http://xuyuan923.github.io/2014/11/04/github-gitlab-ssh/)

**参考2** [一台机器上Github/Gitlab多账户管理SSH Key切换解决push冲突](http://www.ixirong.com/2015/07/29/how-to-use-github-gitlab-together/)

**参考** `还有一些没有列出`



> 待续…… ——by arther



* 克隆某个分支

    ```
    git clone -b   <branch name > <repo name>
    git clone -b develop git@gitlab.com:mashangzhao/wow.git
    ```


* 更新说明 
  * 2017#08#09  加入了ssh-agent不能认证问题解决方案。
  * 2019#03#31  github小号引发的问题
