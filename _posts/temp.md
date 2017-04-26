### 流程

需求分析
原型迭代
（领域模型）
数据库建模
接口定义（工具？“三次握手”？）
并行编码
提交测试


#### 使用<a>标签下载文档
我在使用a标签下载文件是，老是会自动跳转到链接，远啦需要加上download属性
```html
<a href="url" download="">下载</a>
```


#### 代码逻辑方案原则
1、单一控制原则，将逻辑控制统一放在一个逻辑代码段中，倾向于控制集中，虽然可能会降低效率。但是会简化逻辑控制。
2、

#### final局部变量的声明周期
```java
public String buildUri(String userId){
       final Date date=new Date();
       //date=new Date(); 将会报错，本次调用不可变
       return date.toString();
   }
```
结论：final只是标记了date在本次调用中不能再次指向别的对象，并不代表这个对象始终存在，整个程序生命周期中只初始化一次。

#### @PathVariable不能匹配“/”
