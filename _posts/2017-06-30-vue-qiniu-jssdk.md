---
layout: post
title:  "使用vue、elementUI、七牛jssdk做上传组件"
date:   2017-06-30 10:58
categories: 前端
tags: Vue plupload
author: Over air
mathjax: true
---
* content
{:toc}

> 需求：使用七牛jssdk实现分片上传视频等大文件，上传图片等小文件到自有api接口，并根据文件格式提交到不同的类目下

#### 使用vue、elementUI做页面逻辑样式，七牛jssdk做上传组件
遇到这个需求，本来想使用webuploader或者直接用elementUI的`<el-upload>`的，主要七牛的demo jssdk用的是plupload，并且做了自有改写，想想还是别自己折腾，就暴力改写了一点`qiniu.js`实现了需求

#### 上传组件时间调用顺序
1、调用m.qiniu.js的`FilesAdded`方法 <br/>
2、调用当前组件index.js的`FilesAdded`方法 <br/>
3、调用m.qiniu.js的`BeforeUpload`方法 <br/>
4、调用当前组件index.js的`BeforeUpload`方法 <br/>

#### 通过setOption设置uploader属性
考虑到不同的文件可能存在上传到不同的服务器的情况，会在组件里加上一个mConfig的配置项,对应的`reOption`便是需要覆盖的项目。
下面的实例配置表示: <br/>
a、后缀名`bmp,jpeg,jpg,png,gif`的文件会归为Image属性文件（file.upType='Image'） <br/>
b、上传到`http://xxxx.com/file.api`这个接口 <br/>
c、设置上传请求接口header，接口请求要求 <br/>
d、file_data_name指定文件上传时文件域的名称 <br/>
e、上传文件不需要获取token <br/>
```js
var mConfig = {
    Image: {//key
        typeKey: "bmp,jpeg,jpg,png,gif",//后缀名,不区分大小写,filters
        reOption: {//需要改写覆盖的otption，默认七牛,
            'url': 'http://xxxx.com/file.api',
            'headers': {
                'Authorization': window.localStorage.getItem('token') ? "Bearer " + window.localStorage.getItem('token') : ''
            },
            'file_data_name': 'BD_PRODUCT_IMG.content',//文件bucket，自有api上传需要
            'get_new_uptoken': false
        }
    }
  }
```

#### 限制上传文件格式
原本plupload有上传文件格式限制的实现
```js
filters : {
    prevent_duplicates: true,
    // Specify what files to browse for
    mime_types: [
        {title : "Image files", extensions : "bmp,jpeg,jpg,gif,png"}, // 限定jpg,gif,png后缀上传
        {title : "Zip files", extensions : "zip,rar,AI,TIFF,pdf"}, // 限定zip后缀上传
        {title : "Doc files", extensions : "excel,txt,ppt,word"} // 限定文档上传*!/
    ]
}
```
但由于这种实现会影响呼出文件夹性能，因而统一采用后缀名的方式实现文件限制。并直接配置到`mConfig`里面，顺便将相应的后缀名文件放到对应类别中。

#### 获取七牛上传token
七牛上传需要获取上传token，这个需要服务器端接口支持。
因为多数的接口都会有自己的安全机制，所以推荐使用`uptoken_func`方式获取，并一定用回调的方式返回处理：
```js
//uptoken_url: "/uptoken",
uptoken_func: function(file,cb){//后端接口请求token，一般用这个
//自己的请求封装以及安全处理：$.api
     $.api('upload.getQiniuUploadToken',{async:false}).done(function (result) {
         if($.apiResultSync(result).code==0){
           //自己的返回结果处理
           //回调返回token值
             cb($.apiResultSync(result).data.value);
         }
     }).fail(function () {
         console.log('custom uptoken_func err');
         cb('');
     });
 }
```
至此，一个改写过的上传组件就可以了。一般情况下，m.qiniu.js不需要修改了，直接使用m.qiniu.min.js就可以了，自定制的业务直接写在组件的index.js就可以了。 <br/>
这个组件被放在了git上了，可以直接获取uploadQiniu组件文件夹，引用的时候，需要改写index.js实现自己的业务限制级逻辑。 <br/>
项目地址：[七牛上传](https://github.com/flyontheair/QiniuUpload)
