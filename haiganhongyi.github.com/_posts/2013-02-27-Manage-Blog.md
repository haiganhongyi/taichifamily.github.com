--- 
layout: post
title: "管理博客"
tags: 
- 博客
status: publish
type: post
published: true

---
为什么要管理博客：
<pre lang="rsplus">您好，这个世界真的有太多好玩的事物了哈！
博客是点点还是github呢？
Github已经被那么多大神攻占了啊。
小谢在此也占了一个角落?
关于安装
-railinstall 2.01版本
-gem jekyll
-gem rdiscount --version 1.6.8

</pre>
本地博客预览的Git命令：
<pre lang="rsplus">
git clone git@github.com:haiganhongyi/haiganhongyi.github.com.git liaoling
cd liaoling
/** git remote add origin git@github.com:haiganhongyi/haiganhongyi.github.com.git
/**上面这句只需执行一次，以后本机不需执行。
/**在本地写MarkDown博客
git add .
git commit -m "Anoter Post"
git push origin master
/*ok the end
</pre>
这方面的教程，官方[http://pages.github.com/]的最好。同时国内[yanping](http://yanping.github.com) 记录得也非常
方便。主要问题在于，如果不是linux系统（比如Ubuntu），那么Windows里面会遇到中文编码的
种种问题地！