---
layout: post
title:  "两个数组求差集"
date:   2017-05-11 16:11:18 +0800
categories: 前端
tags: javascript 算法
author: Over air
mathjax: true
---

* content
{:toc}

>前端js遇到 比较两个数组的差异,获取删除的项和新增的项 这样一个需求

#### 嵌套循环
开发嘛，不再在乎细节。简单、可读性高是第一要务。今天的任务完成了吗？ <br/>
简单暴力的第一个实现，嵌套循环，获取删除项，再次循环获取新增项，时间复杂度：m*n <br/>
实现一
```js
setDiff:function(first,second){
    var start = new Date().getTime();
    console.log("开始时间：",start+'ms');
    let delList=[];
    let addList=[];
    let oldList=[];
    for(var i = 0,len1=first.length; i < len1; i++) {
      let flag1=true;
      for(var j = 0,len2=second.length; j < len2; j++) {
        if(first[i].id==second[j].id){
          flag1=false;
          oldList.push(first[i]);//老的
        }
      }
      if(flag1){//被删除
        delList.push(first[i]);
      }
    }
    for(var j = 0,len2=second.length; j < len2; j++) {
      let flag=true;
      for(var k= 0,len3=oldList.length;k<len3;k++){
        if(second[j].id==oldList[k].id){
          flag=false;
        }
      }
      if(flag){
        addList.push(second[j]);
      }
    }
    var end = new Date().getTime();
    console.log("结束时间：",end+'ms');
    console.log("方法一执行时间：",(end-start)+'ms')
    return {
      delList:delList,
      addList:addList,
      oldList:oldList
    }
}
```
但是，一个程序猿的操守还是有点的，遇到这种嵌套循环，我就发抖。想想空间换时间，给我点空间！ <br/>
数组遍历存对象（key-value）,时间复杂度n。 <br/>
实现二
```js
setDiffTwo:function(first,second){
    var start = new Date().getTime();
    console.log("开始时间：",start+'ms');
    let delList=[];
    let addList=[];
    let oldList=[];
    let temp={};
    //数组转对象
    for(var i = 0,len1=first.length; i < len1; i++){
      temp[first[i].id]=first[i];
    }

    for(var i = 0,len2=second.length; i < len2; i++){
      if(typeof temp[second[i].id]!='undefined'){
        oldList.push(second[i]);//重复项目
        delete temp[second[i].id];//删除重复项目,剩余删除项
      }else{
        addList.push(second[i]);//新增项
      }
    }

    for(var item in temp){
      delList.push(item);
    }

    var end = new Date().getTime();
    console.log("结束时间：",end+'ms');
    console.log("方法二执行时间：",(end-start)+'ms')
    return {
      delList:delList,
      addList:addList,
      oldList:oldList
    }
}
```
简单测试一下效果
测试代码
```js
test:function(n){
  //初始化
  var list1=[];
  var list2=[];
  for(var i=1;i<n;i++){
    if(i%2==0)
      list1.push({id:i,name:i+"max"});
  }
  for(var i=1;i<n;i++){
    if(i%3==0)
      list2.push({id:i,name:i+"max"});
  }
  var rel=this.setDiff(list1,list2);
  console.log(rel);
  console.log("-----------------------方法二-------------------------");
  var rel=this.setDiffTwo(list1,list2);
  console.log(rel);
}
```
`test(1000)`结果：
```bash
开始时间： 1494496600796ms
Demo2.vue?a82e:66 结束时间： 1494496600801ms
Demo2.vue?a82e:67 方法一执行时间： 5ms
Demo2.vue?a82e:31 Object {delList: Array(333), addList: Array(167), oldList: Array(166)}
Demo2.vue?a82e:32 -----------------------方法二-------------------------
Demo2.vue?a82e:76 开始时间： 1494496600804ms
Demo2.vue?a82e:96 结束时间： 1494496600804ms
Demo2.vue?a82e:97 方法二执行时间： 0ms
Demo2.vue?a82e:34 Object {delList: Array(333), addList: Array(167), oldList: Array(166)}
```
`test(10000)`结果：
```bash
开始时间： 1494496638581ms
Demo2.vue?a82e:66 结束时间： 1494496638647ms
Demo2.vue?a82e:67 方法一执行时间： 66ms
Demo2.vue?a82e:31 Object {delList: Array(3333), addList: Array(1667), oldList: Array(1666)}
Demo2.vue?a82e:32 -----------------------方法二-------------------------
Demo2.vue?a82e:76 开始时间： 1494496638648ms
Demo2.vue?a82e:96 结束时间： 1494496638649ms
Demo2.vue?a82e:97 方法二执行时间： 1ms
Demo2.vue?a82e:34 Object {delList: Array(3333), addList: Array(1667), oldList: Array(1666)}
```
虽然不太可能出现这么大的数组，但在大数据量上的区别还是很吓人的。
