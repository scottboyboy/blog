---
title: ssh服务的一些经历

date: 2017-08-12 11:37:51

tags: [ssh,ubuntu,sudoers]

---


## 先了解下SSH？

> SSH is a software package that enables secure system administration and file transfers over insecure networks. It is used in nearly every data center, in every larger enterprise.

我们常谈的是SSH协议，例如使用口令远程登陆服务器，为了在不安全的网络环境下进行一些操作。

## 没有SSH远程登录配置？
今天看了aliyun的一篇文档，从无到有完成了一个
<!-- more -->

* 禁止root登陆
* 使用口令登陆
* 禁止密码登录
* 禁止22端口，修改其他端口

当然了，中间也遇到了问题，参考了其他文档。
工具说明：
* xshell(或者putty)，用来连接远程服务器
* puttygen(或者gitbash或者linux bash)，用来生成口令

也可以使用其他工具，能完成即可。

---
- 禁止root登陆
禁止root登陆很简单，但是前提是你需要有其他的user。
话不多说，看过程。**root权限操作的。**

```
root@iZ28si2aplqZ:/home# adduser test		#加一个user，登录名是test
Enter new UNIX password: 					#密码
Retype new UNIX password: 					#重复密码

# 忘记了几秒之前输入的密码了？
root@iZ28si2aplqZ:/home# passwd test		#更改test用户的密码
Enter new UNIX password: 
Retype new UNIX password: 

# 注意：先使用test账户登录
如果登录成功，现在有了2个连接；

# 修改sshd_config文件，不允许root用户登录，并保存
找到 
PermitRootLogin no						#修改为no，即不允许使用root账户登录

# 重启ssh服务
root@iZ28si2aplqZ:/home# /etc/init.d/ssh restart
[ ok ] Restarting ssh (via systemctl): ssh.service.

# 尝试root登陆，发现无法登陆，这时候完成了第一步：禁止root登陆。
```
![禁止root登陆](https://asjdfkl1239807yuiao-1253113844.cos.ap-beijing.myqcloud.com/ssh-blog/permitrootlogin.png)

- 使用口令登陆
当然需要生成一对口令，公钥和私钥，然后公钥(.pub)需要放在远程一个合适的位置。
我也相信你有很多方案将公钥传输过去。
话不多说，看过程。

```
mashangzhao@iZ28si2aplqZ:~$ ssh-keygen -t rsa	#生成一对rsa口令
Generating public/private rsa key pair.
Enter file in which to save the key (/home/mashangzhao/.ssh/id_rsa): aliyun	#文件名
Enter passphrase (empty for no passphrase): 	#口令短语
Enter same passphrase again:
Your identification has been saved in aliyun.
Your public key has been saved in aliyun.pub.

# 这时生成好了一对口令
# 公钥(.pub)保存在服务器这边，私钥拿回来，放在本地。

# 配置部分
#1 用户家目录（一般这里）新建.ssh文件夹，/home/user/.ssh/，将aliyun.pub内容复制到authorized_keys中，
mashangzhao@iZ28si2aplqZ:~$ cat aliyun.pub >> .ssh/authorized_keys

#2 配置/etc/ssh/sshd_config文件（有彩色的截图）

#3 修改权限
mashangzhao@iZ28si2aplqZ:/home/mashangzhao# chmod 700 .ssh		#修改.ssh目录权限
mashangzhao@iZ28si2aplqZ:/home/mashangzhao# chmod 400 .ssh/authorized_keys	#修改authorized_keys权限

#4 重启ssh服务
/etc/init.d/ssh restart

#5 测试是否可以使用口令登陆？
如果可以，可以进行下一步，禁止密码登陆。
```
![生成一个密钥](https://asjdfkl1239807yuiao-1253113844.cos.ap-beijing.myqcloud.com/ssh-blog/ssh-key-rsa.png)
![sshd_config](https://asjdfkl1239807yuiao-1253113844.cos.ap-beijing.myqcloud.com/ssh-blog/sshd_config_1.png)
![不允许空的密码](https://asjdfkl1239807yuiao-1253113844.cos.ap-beijing.myqcloud.com/ssh-blog/permitemptypasswords.png)
![密码认证先开启](https://asjdfkl1239807yuiao-1253113844.cos.ap-beijing.myqcloud.com/ssh-blog/passwordauthentication.png)

- 禁止密码登录

```
这一步的目的很明确，禁止使用账户的密码来连接服务器。但是，注意：前提是已经使用口令可以连接。

#1 修改sshd_config文件中的
PasswordAuthentication no			#yes -> no最终改为no即可
#2 重启ssh服务
/etc/init.d/ssh restart

#3 测试还能用密码登陆吗？
答案是：当然不能。那就对了。
```

- 禁止22端口，修改为其他端口

```
#1 绕开常用的22号端口，改为其他端口；
修改sshd_config文件，
Port xxxx			#xxxx为其他端口

#2 重启服务尝试
```

- 小说明
	* xshell的远程传输文件还是很给力，当然前提是服务器安装sz和rz. apt-get install lrzsz
相比较之前的putty+winscp 还是很愉快
	* sudo apt-get install lrzse
发现没有权限，解决方案是加入sudoers

```
whereid sudoers
sudo root
chmod u+w /etc/sudoers
vi /etc/sudoers
# 在root用户的下一行添加上想给sudo权限的用户名即可
root	ALL=(ALL:ALL) ALL
mashangzhao	ALL=(ALL:ALL) ALL
```










