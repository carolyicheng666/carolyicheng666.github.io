---
title: Github 设置 SSH Key
date: 2017-12-12 10:29:05
tags: [Github]
categories: '编程'
---


{% cq %}
只要记住你的名字  
不管你在世界的哪个地方  
我一定会，去见你  

**你的名字**
{% endcq %}


<!-- more -->



**https 和 SSH 的区别：**  
1、前者可以随意克隆github上的项目，而不管是谁的；而后者则是你必须是你要克隆的项目的拥有者或管理员，且需要先添加 SSH key ，否则无法克隆。  
2、https url 在push的时候是需要验证用户名和密码的；而 SSH 在push的时候，是不需要输入用户名的，如果配置SSH key的时候设置了密码，则需要输入密码的，否则直接是不需要输入密码的。

那么如何在github上添加SSH key呢？

步骤1：检查你电脑是否已经有 SSH key
---

在Git Bash中输入：
``` bash
$ cd ~/.sh
$ ls
```
这两个命令就是检查是否已经存在 id_rsa.pub 或 id_dsa.pub 文件，命令下可能还会有其他文件，其中 .pub 文件是你的公钥，而另一个没有 .pub 后缀的同名文件则是你的私钥。如果找不到这样的文件（或者根本没有 .ssh 目录），那么你需要进入步骤2；如果文件已经存在，那么你可以跳过步骤2，直接进入步骤3




步骤2：创建一个 SSH key
---

在Git Bash中输入：
``` bash
$ ssh-keygen -t rsa -C "your_email@example.com"
```
*-t* —— 指定密钥类型，默认是 rsa ，可以省略  
*-C* —— 设置注释文字，比如邮箱  
*-f* —— 指定密钥文件存储文件名  
以上代码省略了 -f 参数，因此，运行上面那条命令后会让你输入一个文件名，用于保存刚才生成的 SSH key 代码
``` bash
$ Generating public/private rsa key pair.
$ Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): [Press enter]
```
如果不输入文件名直接回车，那么就会使用默认的文件名生成 id_rsa 和 id_rsa.pub 两个文件  
接着又会提示你输入两次密码（该密码是你 push 文件的时候要输入的密码，而不是 github 登录的密码）  
我们要用 SSH 方式 push 代码，当然是能简单就简单点，这里推荐不输入密码直接回车，以后 push 的时候就可以不用输密码
``` bash
$ Enter passphrase (empty for no passphrase):
$ Enter same passphrase again:
```
接下来，就会显示类似下面这种提示:
``` bash
$ Your identification has been saved in /c/Users/you/.ssh/id_rsa.
$ Your public key has been saved in /c/Users/you/.ssh/id_rsa.pub.
$ The key fingerprint is:
$ 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
```
当你看到上面这段代码的时候，那就说明，你的 SSH key 已经创建成功，你只需要添加到github的SSH key上就可以了。



步骤3：在 Github 上添加SSH key
---
- 拷贝 id_rsa.pub 文件的内容
- 网页打开自己的 github ，点击头像右边的小箭头，选择 Settings ，进入后再在左侧选择 SSH and GPG keys
- 点击 Add SSH key 按钮添加一个 SSH key 。把你复制的 SSH key 代码粘贴到 key 所对应的输入框中，记得 SSH key 代码的前后不要留有空格或者回车。当然，上面的 Title 所对应的输入框你也可以输入一个该 SSH key 显示在 github 上的一个别名。默认的会使用你的邮件名称



步骤4：测试是否添加成功
---

在Git Bash中输入：
``` bash
$ ssh -T git@github.com
```

会提示一段警告代码，类似下面这样：
``` bash
$ The authenticity of host 'github.com (207.97.227.239)' can't be established.
$ RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
$ Are you sure you want to continue connecting (yes/no)?
```

这是正常的，你输入 yes 回车即可。然后会出现：
``` bash
$ Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

这样就设置成功了。随便找个以前的项目，按如上方法操作，可能会看到 “access denied” ，者表示拒绝访问，那么你就需要使用 https 去访问，而不是 SSH 。  顺便说一句如何把一个项目从 https 换成 ssh ，把下面命令的 ssh-url 换成项目的即可：
``` bash
git remote set-url origin 'ssh-url'
```



写在后面
---

可能有的朋友会担心如果在公司的电脑配置了，自己家里的电脑要怎么办，或者今后换了电脑、重装了系统要怎么办等等问题。  

这里首先得清楚公钥和私钥的关系，公钥就相当于大门，私钥就相当于钥匙，要相互匹配才能把门打开。比如说现在这种情况，公钥已经在步骤3中配置完成，而私钥则保存在我们的电脑上，默认路径是 `C:\Users\XXX\.ssh` 。   

网上大致提供了两种解决办法：一种是为大门再配一把钥匙；另一种是另外开一个大门，为其匹配一把钥匙。  

我尝试了第一种方法，卡在了步骤4的第一步，提示的是 `Permission denied (publickey).` 这说明这样做并不行，具体操作我不细说了，总之又是改文件名又是改配置的，就算成功了也很是麻烦。这里我采用的做法是，直接拷贝这个 `.ssh` 文件夹到另一电脑的相同目录下，也就是说让这两台电脑使用同一把钥匙开门，是不是很简单~  

第二种方法适合的是两台电脑不是同一种操作系统的情况，比如说你在公司用的是 Windows ，而你家里用的是 Linux 等等，当然设置的方式照着上面再来一遍就行了  

最后，还是要提醒一下，你要保证这两台电脑足够安全，不然要是某个家伙动了你的电脑，papapa把你的东西全删了，你哭都找不到人哭去......