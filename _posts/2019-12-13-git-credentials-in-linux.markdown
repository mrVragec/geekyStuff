---
title:  "Git credentials in Linux for anyone"
date:   2019-12-13 22:28:00
categories: [git, linux, credentials]
tags: [git, linux, credentials, password]
---

So I work daily with git, especially with different git repos (gitHub and gitLab). And typing a password every time for a pull/push is a nightmare. 
So I google-up a little bit and find out quite a nice solution got my problem (using libsecret as a credential manager). 
So here are needed steps to set it up. 
First, you will need to install `libsecret-1-0` (and `libsecret-1-dev`). 

On Ubuntu (and similar derivates) you can use the following command
``` console
geek@home:~$ apt-get install libsecret-1- libsecret-1-dev
```
and in ManjaroLinux you can find this package in Package Manager. 

After installation, you will need to build the credential manager (:S). 

On Ubuntu, you can use following command: 
``` console
geek@home: cd /usr/share/doc/git/contrib/credential/libsecret 
geek@home:~$ make
```

In ManjaroLinux the libsecret is in different directory:
``` console
geek@home: cd /usr/share/git/credential/libsecret/
geek@home:~$ make
```
Finally, you should point git to use libsecret as credential helper:

For Ubuntu: 
``` console
geek@home: git config --global credential.helper /usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret
```

For ManjaroLinux:
``` console
geek@home: git config --global credential.helper /usr/share/git/credential/libsecret/git-credential-libsecret
```

