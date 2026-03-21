---
title: '博客更新'
date: '2025-01-08 01:35:00'
authors:
  - me
categories:
  - '记录'
  - '网站'
toc: true
---

在忙了一段时间后，花了一下午捣鼓了blog。
<!--more-->
### 页面头图更换
将页面头图换成了暗喻幻想中的旅途画卷。在游玩的时候就感觉印象深刻，这个地方atlus做的还是非常好的，非常的好看。

### 评论功能的实现
在折腾了快两个小时gitalk无果之后选择了[Utterances](https://github.com/apps/utterances)。
安装Utterances app后，在GitHub端配置选择Repository的用户名和仓库名称（blog的仓库，例如我的是 meltlis/meltlis.github.io），之后依次是issues与posts的映射关系还有评论的issues的label，全部默认就行。最后就是选择喜好的Theme。
最后在本地_config.yml对照配置就完成了。记得将comments enable并选择Utterances。
配置如下:
```
utterances:
  repo: (YourUserName/YourRepositoryName)
  issue_term: pathname
  label: utterances
  theme: github-light
  theme_dark: github-dark
```

### 未来可能的友联
依照Fluid文档完成了折跃门部分，可以找博客主索要或者互加友联。

### 关于页完善
依照Fluid文档完成了关于页的简介。
