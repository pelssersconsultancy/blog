title: How to setup your own blog with Github Pages and Hexo
date: 2016-04-08 22:54:02
categories:
- Tutorials
tags:
- Hexo
- Github 
- Markdown
---
### 1. Create a new github account

 Navigate to [Github] and click the Signup button. It is important to choose the same 
 username as the one you want to use for [**username**].github.io
   
 Let's assume you created a github account with username [**johndoe**] and email address [**johndoe@gmail.com**].

### 2. Create a new repository 

 [Github pages] explains how to setup your own blog site, so you can check it for reference. 
 Log in on [Github] and click on the button 'New Repository'.   

 Now you should see a similar page like below with Owner [**johndoe**]. Fill in [**johndoe.github.io**] as repository name.

 {% asset_img creating_repository.png Creating github repository %} 

### 3. Setup you ssh key for github
Github is a versioning system where each different version of any file belonging to your site will be saves and tracked.  

However, in order to be able to save your files from your computer to github, you need to do following steps:
* generate an ssh-key for the above created github account
  ```
  johndoe@laptop:~$ ssh-keygen -t rsa -b 4096 -C "johndoe@gmail.com"
  Generating public/private rsa key pair.
  Enter file in which to save the key (/Users/johndoe/.ssh/id_rsa): /Users/johndoe/.ssh/id_johndoe_rsa
  Enter passphrase (empty for no passphrase):
  Enter same passphrase again:
  Your identification has been saved in /Users/johndoe/.ssh/id_johndoe_rsa.
  Your public key has been saved in /Users/johndoe/.ssh/id_johndoe_rsa.pub.
  The key fingerprint is:
  SHA256:zFNrmBSufsH/URijFp/+rO9iKlRmj8XRjB1HasidLDE johndoe@gmail.com
  The key's randomart image is:
  +---[RSA 4096]----+
  |        .   E=.oo|
  |       . . .o*++ |
  |        o o.*.*  |
  |       * ++=oB   |
  |      . S+=++ .  |
  |     .  .*....   |
  |      ... . o    |
  |       ..  .o+   |
  |         ..oo==  |
  +----[SHA256]-----+
  ```

* Verify the following two files have been generated
```
johndoe@laptop:~$ ls -la .ssh
-rw-------   1 johndoe  staff  3243 23 apr 16:39 id_johndoe_rsa
-rw-r--r--   1 johndoe  staff   743 23 apr 16:39 id_johndoe_rsa.pub
```


* Copy the contents of the public key to the clipboard
```
$ cat .ssh/id_johndoe_rsa.pub | pbcopy
```

* Go the the [Github] and click on Settings. 

{% asset_img github_settings.png Edit your github settings %} 


* Next click on 'SSH and GPG keys' in the left menu. Now click on the button 'New SSH key'. 

 Fill in a title like 'SSH key of John Doe' and paste the string that you copied into   
 the clipboard before' which should look a bit like below.

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDddmyo4+UtzzUju6GwBGQu6NatFVmnDSZrykeqnXLSFT6JY1Ky57x62h80eFkmewauqnVZO09H8klkFMXDeuoweGeYFqowG+c5XwKsTIr4hC5Zxw+9CRa71xk7Xppdc4ROVZsbHVgb4F1kPkcNe4O9R+lyZqRNOb1clwngPfusyzH1WWl60fqhyU9n7WlqwdJ/ih9so8FgvmPYM27uKexOtcF7YWlXYnCLN1n8eRdGvTN01fRtON7zwxj3CyB+qD1JXfumneP0bcYSvBpVNZFDcRBclP7KluvEFYWDaZ8GH7xLJtrf34rIACE7LOFRWFYSTz5BI66794BGhastyczRcPZOzWa1ZQSsdB5iNLX8U32ZLqI71rA8tiViVGqpcVGkCRrl/QetdMEuebf67clXd4bp+AC8753UYfeIYAp0+uAT9zwnZWoBg3JW+RtwzeG7Hg8N3WnB6ULU0PqnA2fUtu0Fm4LpK9YcmRJ+izr6SlcZH87nK/rLcqv99vJSEaIcJz4+IBgZ8MKzfIZ5thritMD8xbKqMkGl1D6oG2ggR+WvnVgSlzdczHXreDy/KHWFDvTH/Vivta5EuOo7mRPkSAB2Ml809ev0+Ul6UvyJXYdTu5+ne5KzSHYWZV+C2lsT/aDT2gmsKNhlyPZOgwvn6IGinTKbO40QEjYozkPxFw== johndoe@gmail.com

```

* Create a new .ssh/config file or append to existing config file following host
```
Host github.com-johndoe
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_johndoe_rsa
```

### 4. Make sure you have [NPM] and [NodeJs] installed

### 5. Make sure you have [git] installed

### 6. Install [Hexo]
```
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```

Browse to http://localhost:4000 and verify you see the initial empty blog. To shutdown the server again just press ctrl-c

### 7. Look for a [theme] you like to use and install it.

 I chose the [icarus] theme.  To install it change directory into your newly created **blog** folder and clone the theme using git
 
 ```
 $ git clone https://github.com/ppoffice/hexo-theme-icarus.git themes/icarus
 ```
 
### 8. Adapt the following in the file "blog/_config.yml"

```
# Site
title: Pelssers Consultancy
author: Robby Pelssers
language: en


# URL
url: http://pelssersconsultancy.github.io/

# Writing
new_post_name: :year-:month-:day-:title.md # File name of new posts
post_asset_folder: true

## Themes: https://hexo.io/themes/
theme: icarus

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com-johndoe:johndoe/johndoe.github.io.git
  branch: master
```
 

### 9. Rename the file 'blog/themes/icarus/_config.yml.example' to '_config.yml'

### 10. Adapt the file 'blog/themes/icarus/_config.yml' to your needs. E.g.:


### 11. Create your first blog post

```
robbypelssers1@Macbook-Robby-Pelssers:~/Documents/pelssersconsultancy/blog$ hexo new "My First Blog Post"
INFO  Created: ~/Documents/pelssersconsultancy/blog/source/_posts/2016-04-23-My-First-Blog-Post.md
```

As you can see a skeleton [markdown] file gets generated. 

The markdown for this first blog article looks like:

```
# How to setup your own blog

### 1. Create a new github account

 Navigate to [Github] and click the Signup button. It is important to choose the same 
 username as the one you want to use for [**username**].github.io
   
 Let's assume you created a github account with username [**johndoe**] and email address [**johndoe@gmail.com**].

### 2. Create a new repository 

 [Github pages] explains how to setup your own blog site, so you can check it for reference. 
 Log in on [Github] and click on the button 'New Repository'.   

 Now you should see a similar page like below with Owner [**johndoe**]. Fill in [**johndoe.github.io**] as repository name.

 {% asset_img creating_repository.png Creating github repository %} 

### 3. Setup you ssh key for github
Github is a versioning system where each different version of any file belonging to your site will be saves and tracked.  

...
...
...

[Github]: https://github.com
[Github pages]: https://pages.github.com
[pelssersconsultancy]: http://pelssersconsultancy.github.io
[NodeJS]: https://nodejs.org
[NPM]: https://www.npmjs.com/
[git]: https://git-scm.com/download
[Hexo]: https://hexo.io/
[theme]: https://hexo.io/themes/
[icarus]: https://github.com/ppoffice/hexo-theme-icarus
[markdown]: https://guides.github.com/features/mastering-markdown/
```


[Github]: https://github.com
[Github pages]: https://pages.github.com
[pelssersconsultancy]: http://pelssersconsultancy.github.io
[NodeJS]: https://nodejs.org
[NPM]: https://www.npmjs.com/
[git]: https://git-scm.com/download
[Hexo]: https://hexo.io/
[theme]: https://hexo.io/themes/
[icarus]: https://github.com/ppoffice/hexo-theme-icarus
[markdown]: https://guides.github.com/features/mastering-markdown/


### 12.Generate the site
```
$ hexo generate
```

### 13. Deploy your site to github
```
$ hexo deploy
```




