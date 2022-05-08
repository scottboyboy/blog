---

title: 多个github帐号引发的问题

date: 2019-05-19 16:51:00

tags: [git]

---

近期使用github的过程中，引发了各种问题。例如：

- [多个github帐号带来的认证问题](#%E5%A4%9A%E4%B8%AAgithub%E5%B8%90%E5%8F%B7%E5%B8%A6%E6%9D%A5%E7%9A%84%E8%AE%A4%E8%AF%81%E9%97%AE%E9%A2%98)
- [submodule问题](#submodule%E9%97%AE%E9%A2%98)
- [npm下载缓慢](#npm%E4%B8%8B%E8%BD%BD%E7%BC%93%E6%85%A2)

### 多个github帐号带来的认证问题

当前问题：两个github帐号，如何在同一台电脑上pull、push。

重点是 `~/.ssh/config文件`，部分内容如下
<!-- more -->

```
#github
Host github.com
        HostName github.com
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/id_rsa_github

#github_scottboy
Host github_scottboy
        HostName github.com
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/scottboy_github
        User scottboyboy
```
大致道理是通过Host来区别不同的git地址，这个Host相当于HostName的别名，你可以安装一定规则随便起，不要太过分就ok。

那么大致流程呢：
1. 生成公钥、私钥对，公钥保存在github网页端，私钥保存在本地 `ssh-add`  添加;
2. 修改 `~/.ssh/config` 文件大致结构如上，内容按照自己的填写;
3. 测试是否可以控制远程仓库，命令：
   ```
   ssh -T git@github.com    #注意：测试的是第一个github帐号，对应的是config文件中的Host github.com。如果你看到了Hi，差不多成功了！

   ssh -T git@github_scottboy   #注意：测试的是第二个github帐号，对应的是config文件中的Host github_scottboy。如果你还看到了Hi，应该是成功了！
   ```
4. 取消全局定义的 `user.name` 和 `user.email`，命令：
    ```
   git config --global --unset user.name
   git config --global --unset user.email
   ```
5. 可以使用`git clone`命令，注意此处的域名有了别名，不要到处使用`git@github.com`了！
    ```
    git clone git@github.com:scottboyboy/hibernate.git  #错误：你去clone一个私有的项目，虽然你在浏览器看得到，但是域名是github.com，你会看到“你没有权限”的消息
    git clone git@github_scottboy:scottboyboy/hibernate.git #正确
    ```
6. 以及搞定了`clone`，现在需要设置提交时的对应仓库的用户名，邮箱，命令
   ```
   git config user.name "hasdfasd"
   git config user.email "asdfasf@asdf.com"
   ```

> 好了，现在基本可以愉快了有多个帐号玩耍了。

---------------
### submodule问题

今天换了个博客主题theme，部署完之后，发现看不到网页。刚开始还以为是hexo出问题了，但是也没有；最后跑到主题作者的issue看了一圈，没发现啥，最后的最后跑到了自己的博客发布仓库看了一下，原因竟是主题目录是一个`子模块submodule`，我打不开目录。

最后，想明白了：我直接clone了主题作者的仓库，里面有`.git`文件夹，当时删除这个文件夹可能会解决问题，但是当时没这么做。

我又在浏览器里面直接下载了压缩包，然后 `rm -f -f .git`，提交发现还是不行，搜索了资料**如何删除子模块**。

```  
git rm --cached theme/even/ #终于成功了，部署后发现有页面了！
```

-------
### npm下载缓慢

各种用户变量，都给加上：
```
NODE_PATH=你的博客仓库node js module路径
Path追加 node js安装路径
```
发现了taobao的npm镜像，https://npm.taobao.org/ ，大致参照就可以。


参考文献：
1. git全局配置 https://www.cnblogs.com/zqunor/p/9055262.html
2. 删除子模块 http://www.worldhello.net/2010/01/26/425.html
3. taobao的npm镜像 https://npm.taobao.org/