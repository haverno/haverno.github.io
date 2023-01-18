---
title: "主机配置多个git账号"
date: 2023-01-18T14:24:25+08:00
draft: false
---

1. 如果没有ssh密钥，执行`ssh-keygen -C "xxx"`，名字可以设置为*id_rsa_xxx*，其目录默认在~/.ssh下。
2. 对于多个ssh密钥对及github的配置: `vim ~/.ssh/config`，例如：
    ```sh
    Host jarvis
        User "kennyissak@gmail.com"
        Hostname ssh.github.com
        IdentityFile ~/.ssh/id_rsa_jarvis
        Port 443
    Host haverno
        User "lyy18807297646@gmail.com"
        Hostname ssh.github.com
        IdentityFile ~/.ssh/id_rsa_haverno
        Port 443
    ```
3. 当要在github上通过ssh方式clone项目时，将github.com替换为上面的Host，例如：`git clone git@github.com:xxx/xxx.git`，替换为：`git clone git@haverno:xxx/xxx.git`
4. 如果要指定账号commit或push已经clone下来的项目，取消user的全局配置，并在项目的根目录执行`git confit -e`，修改配置：
    ```sh
    [remote "origin"]
    url = git@haverno:xxx
    [user]
	email = xxx
	name = xxx
    ```
    再进行commit & push即可。