---
layout: post
title:  "element-ui 踩坑实践"
date:   2017-04-03 23:45
categories: 前端
tags: element-ui
author: Max
mathjax: true
---
* content
{:toc}

#### element-ui使用表格展示数据。使用Popover弹出提示和tooltip冲突问题。
需求是，在表格上的数据太长是，使用省略号，鼠标移上去显示全部。考虑使用了Popover，因为可以自由的控制样式，不曾想，elementUI的表格，会自动添加tooltip提示文字（在表格的问题使用overflow：hidden并且其作用时）。而且没有关闭属性。我好难过。。。结果如图示： <br/>
![tip.jpg](http://gitpages-1251551899.picgz.myqcloud.com/4-13-1.JPG) <br/>
最后，放弃了，直接修改tooltip样式到需求，去掉Popover.

#### 使用element-ui的tab标签页，button按钮会刷新页面
问题描述，tab页面里使用了form表单，但是提交按钮直接使用了`<button>`,发现提交的时候会自动刷新页面。
问题解决：`<el-form>`里面使用`<el-button>`按钮提交数据，解决问题。应该是element-ui对按钮有些限制，后续研究。
