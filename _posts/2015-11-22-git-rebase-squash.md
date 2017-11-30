---
layout: post
title: Git怎样合并最近两次提交
date: 2015-11-22
banner_image: 
tags: [Git]
---

有时候,为了让commit更整洁一些，需要把近几次的commit合并成一个。比如提交了一个修改，后来又进行了相关的重构，重构完了又提交一次，这时可能会想把这两次commit合并为一个。使用git rebase命令可以实现。

<!--more-->

**`git rebase -i HEAD~2`**  
敲完这个命令并回车后，会出现类似下图所示界面:  
{% include image_caption.html imageurl="/images/posts/git_rebase_1.jpeg" caption="git rebase #1" %} 

根据提示，把第二个“pick”改成“squash”，这样就可以把第二个commit合并到到第一个里，修改并保存后会出现类似下图所示界面:  
{% include image_caption.html imageurl="/images/posts/git_rebase_2.jpeg" caption="git rebase #2" %}
这个界面会把两次commit的comments列出来，你可以任意修改成想要的注释，然后保存即可。  

如果想合并多个commit，把2改成相应的数字即可。另外，如果commit已经push到远程，最好就不要再去合并了，否则会比较麻烦，老老实实再提交commit吧，注释写清楚一些就好。

