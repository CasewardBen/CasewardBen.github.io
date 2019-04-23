---
layout: post
title: 聊聊Django中静态文件的配置和app的使用
category: django
tages: [django]
---
Django是一个开放源代码的Web应用框架，由Python写成。采用了MVT的软件设计模式，即模型Model，视图View和模板Template。它最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的。并于2005年7月在BSD许可证下发布。这套框架是以比利时的吉普赛爵士吉他手Django Reinhardt来命名的。

Django的主要目标是使得开发复杂的、数据库驱动的网站变得简单。Django注重组件的重用性和“可插拔性”，敏捷开发和DRY法则（Don't Repeat Yourself）。在Django中Python被普遍使用，甚至包括配置文件和数据模型。<https://zh.wikipedia.org/wiki/Django>

类似的Web开发框架还有Flask等，
这里我们不从头开始介绍Django开发的整个过程，网上有Django开发文档和各种Django教程，我只提出几点需要注意的和容易引起困惑的要点。
首先，
在你使用Django之前一定要确认好自己的Django版本
在你使用Django之前一定要确认好自己的Django版本
在你使用Django之前一定要确认好自己的Django版本
Django目前最新版本为2.x，2.x和1.x不管是在使用还是开发的配置细节上都存在很大的差异，甚至如果你会用其中某一个版本，那么另一个版本第一次上手的时候你会一个接一个坑的踩，这里我建议你使用最新版的2.x，2.x相较于1.x在使用中更加方便一点，使用过程可以省略诸多配置的过程，尤其是起初不熟悉的时候对settings.py文件中各种路径的配置，我相信使用过程中你会遇到这个困惑，后面我会提到。
解释一下几个概念，project和app，很好理解，就是整个项目和项目中的功能，注意，你的开发都是基于独立的app的，这样可以分割项目模块，分头开发，并且你所开发的app可以复用，也就是上面维基百科提到的“可插拔性”，
简单提一下开发过程,
cmd到你准备存放项目的文件夹，执行
```python
django-admin startproject yourProjectName
```
你会发下原始文件夹下生成了一个yourProjectName文件夹，接下来你会在这个文件夹下得到一系列的文件和文件夹，其中有一个manage.py文件，这就是接下来我们要用的
```python
py manage.py startapp yourAppName
```
这一步你就生成了自己的app，
你可以用同样的方法生成很多app，
但是在后续的使用过程中你会发现，新的app使用的时候好像一直报错
具体是什么错误呢，
时间久远了，不截屏了，如果你仔细看浏览器的报错信息就会发现，新的app各种路径似乎都有问题，应该可能大概是FileNotFound
记住，以后每新建一个app，去settings.py文件中，INSTALLED_APPS项中添加一句
```python
'yourProjectName.yourAppName.YourProjectNameConfig',
```

下面介绍一些关于静态文件路径设置问题，这是一开始使用Django时很令人困惑的一点，
首先我是基于Django2.x说的，因为老版本牵涉到settings.py中一些默认路径的设置，不友好，好麻烦，放弃了
稍微了解点Web开发的都知道一个网站有html,css,可能有javascript，图片等等，那么在Django框架中这些文件放在哪呢，
进到你的app里，新建一个名叫templates的文件夹和一个static的文件夹，
html放在templates里，其他css啊，JavaScript啊都叫静态文件，放在static里，你可以在static里在新建一个css文件夹和一个js文件夹来区分，
然后html里怎么调用这些静态文件呢
在html开头调用外部静态文件之前加一句load static，用{% 括起来
```html
<link rel="stylesheet" href="{% static 'css/yourCss.css' %}" >
```
第一句就是提供一个静态的默认路径，就是我们上面新建的那个static文件夹，
下面的调用模仿就好了，
其他静态文件也是一样的，
但是，
JavaScript文件里怎么调用静态文件呢呢？？？
比如一个html文件或者一个图片
这里使用的是相对路径，但是这个相对路径是基于调用这个js文件的html对于要调用的静态文件的相对路径，
举个例子
你js文件夹中A.js要调用img文件夹里的图片B.gif，
而调用这个js文件的html是templates文件夹中的C.html文件
好，那你在js中要这么写
```python
'../static/img/B.gif'
```
感受一下，不难理解。

好，balabala说了一堆，要记住的就两个
```python
'yourProjectName.yourAppName.YourProjectNameConfig',
```
和
```python
'../static/img/B.gif'
```
至于其他的问题，仔细研究研究浏览器的报错和服务器窗口的报错，
要相信只要有明确报错的，那都不是事，嘿