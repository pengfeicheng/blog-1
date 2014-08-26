---
layout: post
title:  "小伙伴们如何开始发表文章？"
author: wutiantong
tags: site tips-share
---

###简要的回答

1. `$git clone https://github.com/senscribe/blog.git`
2. 在工作目录下的`_posts`子目录中新建一个文件，文件名参考其它文件的格式。
3. 使用[markdown语言](http://daringfireball.net/projects/markdown/syntax)编辑blog内容。
4. 完成后用`git commit`提交修改，并用`git push`推送至GitHub服务器上即可。

###在本地运行jekyll

在本地运行jekyll可以预览你当前编辑内容的显示效果。这项功能实际上非常重要，如果没有这项功能的话，你只能通过把当前修改提交并推送到GitHub上来查看实际显示效果，如果有问题还要再次修改&提交&推送，这会造成一些不美观的提交历史，而且用起来也很不方便。

jekyll的安装方法可以参考<http://jekyllrb.com/docs/installation/>或者<https://help.github.com/articles/using-jekyll-with-pages#installing-jekyll>，后者虽然较为复杂，但是也更为推荐，毕竟我们正在用的正是GitHub Pages。

需要特别注意的是，运行jekyll的命令为：`bundle exec jekyll serve --baseurl ''`，具体原因请参考<http://jekyllrb.com/docs/github-pages/#project-page-url-structure>，在此就不多做解释了。

小提示：在运行命令中加上`--watch`参数可以在修改时自动重建site，实现实时预览效果，出处请参考<http://jekyllrb.com/docs/usage/>。

###正确命名你的post文件名

相关规则请参考<http://jekyllrb.com/docs/posts/#creating-post-files>

###简单了解一下Front Matter

每个post文件的开头位置必须是Front Matter用于定义这篇blog的一些特定属性，具体格式请参考<http://jekyllrb.com/docs/frontmatter/>。

这些属性中最重要的一个就是`layout`，`layout`所对应的值实际上是位于`_layouts`子目录中的html模版，blog对应的模版就是`post.html`，因此post文件中Front Matter的`layout`取值为`post`（省略后缀名）。   
其它重要的属性包括：`title`表示blog的标题，`author`表示blog的作者，`tags`表示blog内容对应的标签，相当于关键词。   
`categories`表示blog的分类，这里注意一下，分类名将会自动填入blog的URL中，比如请看第一篇blog——由jekyll自动生成的欢迎页——它的Front Matter中包括`categories: jekyll update`，你可以看到它的URL则是`http://senscribe.github.io/blog/jekyll/update/2014/08/20/welcome-to-jekyll.html`。不过，我认为一般的blog并不需要特意设置`categories`。

###进阶技能

jekyll进阶技能主要依赖于一种叫[Liquid](http://wiki.shopify.com/Liquid)的东东，有兴趣的小伙伴们可以自己学一下先，非常简单好懂。