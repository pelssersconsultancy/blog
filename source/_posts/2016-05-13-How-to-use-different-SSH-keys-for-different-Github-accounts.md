---
title: How to use different SSH keys for different Github accounts
date: 2016-05-13 09:48:18
tags:
 - SSH
 - Github
---

 {% asset_img github-preview.gif Github image %} 

### 1. Context

As self-employed developer I have two github accounts:
* 1 account for personal use [robbypelssers]
* 1 account for my company [pelssersconsultancy]

So I would like to use a different ssh key to authenticate with per account.
 
The default ssh key is coupled to [robbypelssers] and I created a second ssh key for [pelssersconsultancy].

```
robbypelssers1@Macbook-Robby-Pelssers:~/.ssh$ ls -la *pelssersconsultancy*
-rw-------  1 robbypelssers1  staff  3243 Apr 23 11:39 id_pelssersconsultancy_rsa
-rw-r--r--  1 robbypelssers1  staff   757 Apr 23 11:39 id_pelssersconsultancy_rsa.pub
```

In order to always couple the  my private .ssh key  'id_pelssersconsultancy_rsa' to 
any repository from pelssersconsultancy I added the following entry to .ssh/config

```
robbypelssers1@Macbook-Robby-Pelssers:~/.ssh$ more config
Host github.com-pelssersconsultancy
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_pelssersconsultancy_rsa
```
    
So in order to checkout the project [mytodolist] normally I would do 

```
$ git clone git@github.com:pelssersconsultancy/mytodolist.git
```

but instead now I can do

```
$ git clone git@github.com-pelssersconsultancy:pelssersconsultancy/mytodolist.git
```

Just to verify:

```
$ git remote show origin
 remote origin
  Fetch URL: git@github.com-pelssersconsultancy:pelssersconsultancy/mytodolist.git
  Push  URL: git@github.com-pelssersconsultancy:pelssersconsultancy/mytodolist.git
```

[robbypelssers]: https://github.com/robbypelssers
[pelssersconsultancy]: https://github.com/pelssersconsultancy
[mytodolist]: https://github.com/pelssersconsultancy/mytodolist

