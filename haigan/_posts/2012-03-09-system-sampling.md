--- 
layout: post
title: "系统抽样"
description: "系统抽样的一些感想"
tags: 
- 系统抽样
- Systematic Sampling
status: publish
type: post
published: true

---

#系统抽样Systematic Sampling

今天来谈谈系统抽样。先看看在R中的一段最简单的动画[模拟](http://animation.yihui.name/samp:systematic_sampling)：

`
sample.system(nrow = 10, ncol = 10, size = 15, p.col = c("blue", "red"), 
    p.cex = c(1, 3))
`

再次引用yihui的wiki语录吧
>Systematic Sampling is often used instead of random sampling. It is also called an Nth name selection technique. After the required sample size has been calculated, every Nth record is selected from a list of population members. As long as the list does not contain any hidden order, this sampling method is as good as the random sampling method. Its only advantage over the random sampling technique is simplicity. Systematic sampling is frequently used to select a specified number of records from a computer file. 

大意如下：
>系统抽样常常作为简单随机抽样的一种替代方法被使用。它也被称为"一个N中名字的选择技术"（为啥呢？）一旦需要的样本量计算出来（n），那么每一个Nth记录都会被从总体中选出来。只有总体抽样框中不包含隐藏的序列，那么这种抽样技术的效果会优于简单随机抽样方法（*是这样的吗？*）。其实，相对于简单随机抽样技术，它唯一的优势在于"简便"（抽样实施过程简便）。Ps:系统抽样技术经常被使用来选择电脑文件中的“记录”。

一个[国家统计局的话](http://www.stats.gov.cn/tjzs/tjcd/t20030807_96587.htm)：
>等距抽样，也叫机械抽样或系统抽样。是将总体各单位按一定标志或次序排列成为图形或一览表式（也就是通常所说的排队），然后按相等的距离或间隔抽取样本单位。特点是：抽出的单位在总体中是均匀分布的，且抽取的样本可少于纯随机抽样。等距抽样既可以用同调查项目相关的标志排队，也可以用同调查项目无关的标志排队。等距抽样是实际工作中应用较多的方法，目前我国城乡居民收支等调查，都是采用这种方式。


ok,yihui的翻译完了。其实了，yihui说的不是很易懂，但是涵盖得较为全面。

 * 第一点：谈系统抽样必和简单随机抽样做比较。
 * 第二点：系统抽样的特点：牵一发动全身，故名为“系统”。
 * 第三点：系统抽样一定实际抽样实践走出来的。

PS:在进行系统抽样前有一个总体抽样框的序列，面临着一个如何排序的问题（有关标志排序还是无关标志排序呢？）
如果是无关标志排序，那么得到的结果是简单随机抽样的结果（因此，效果一样，操作更简便，这就是选它的理由。）
如是有关标志排序，则情况不一样。比如按照收入区间进行排序，抽样的时候排序的变量即使系统抽样序列的对象。上述例子要进行系统抽样的抽样框不是样本序列号，而是收入序列，当抽出相应收入序列后，按照事先确定的每个样本序列的收入入样区间确定最终选取的样本。
这种方法，是被经常第被使用的：选择辅助排序变量进行抽样间接抽取样本。

##抽样过程

###直线等距等概率抽样过程（Linear Systematic Sampling）

注意：采用此法抽样框要能等分才是等概率抽样，否则请跳转圆形等距抽样（Circular Syatematic Sampling）

为了表示一点geek:用R语言来描述一下过程，其实都很初级，也很幼稚啊

`
sample.sys = function(x,n)
{
  N = length(x)
  k = as.integer(N/n)
  r = sample(k,1)
  sam =0
  for(j in 1: n)
{
  sam[j] = r + (j-1)*k;
} 
  sam
}
`
>其实吧，如果可以编写代码来解决的话，那么前提必须是：有抽样框的序列！
然而在实际抽样过程中，比如到了某个小区，你要随机抽样，最简便的方法，难道不是“系统抽样”吗？

**注意事项**：上述表示的等概率抽样的前提是K算出来是个整数！如果不是整数，采用直线等距抽样（2.5选组距为2）将会产生无偏估计。怎么办？不用担心，这些传统的抽样技术早就被研究得很彻底了！采用圆形系统抽样（Circular Systematic Sampling）
>circular.sys = function(x,n)
{
  N = length(x)    
  k =as.integer(N/n)
  r = sample(N,1)#从1：N中随机选取一个起点
  sam = 0
  for(j in 1: n)
  {
    if (r + (j-1)*k <= N)
      {sam[j] = r + (j-1)*k}
    else sam[j] = min (r + (j-1)*k,r + (j-1)*k-N)
  }
  sam
}

然而实际工作者还有更粗暴和简便的方法，就是强制N=nk.那么做法就是这样的：
>linear.sys.fact = function(x,n)
{
  #据说这里还要用set.seed()
  N = length(x)
  k = as.integer(N/n)
  runif(N-n*k>0)
  {
    x = x[-sample(N,N-n*k)]
  }
  r = sample(k,1) #随机起点被限制在了1：k中
  sam =0
  for(j in 1:n)
  {
    sam[j] = r + (j-1)*k;
  } 
  sam
}

