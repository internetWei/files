&nbsp;&nbsp;&nbsp;&nbsp;关于Markdown的基础语法，网上有很多资源，不过大部分都是一些基础语法，而且排版和样式我都不太喜欢，这是我结合了Markdown的基础语法和一些高级语法而写的Markdown语法备忘录。

> 建议下载到本地保存，语法有点多，要用的时候看看就好，有些语法浏览器不支持，如果不显示的话可以[点击链接](https://gitee.com/internetWei/files/raw/master/Files/Markdown语法备忘录.md)下载.md文件查看。

基础语法
===============

#### 转义字符
```
我不是\*倾斜文字\*
```
我不是\*倾斜文字\*

#### 标题
```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

另一种写法的一级标题
===============
另一种写法的二级标题
---------------
```

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
另一种写法的一级标题
===============
另一种写法的二级标题
---------------

#### 多行段落
```
第一行

<br>
<br>
第二行
```

第一行

<br>
<br>
第二行

#### 单行注释
```
[//]: 这是一行注释信息
```
[//]: 这是一行注释信息

#### 多行注释
```
<div style='display: none'>
    我是第一行注释，
    我是第二行注释，
</div>

<!--
我是第一行注释，
我是第二行注释。
-->
```
<div style='display: none'>
    我是第一行注释，
    我是第二行注释，
</div>

<!--
我是第一行注释，
我是第二行注释。
-->

#### 空格
```
第一个文字 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;第二个文字
```
第一个文字 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;第二个文字

#### 加粗
```
我是 __加粗文字__
我是**加粗文字**
```
我是 __加粗文字__
我是**加粗文字**

#### 倾斜
```
我是 _倾斜文字_
我是*倾斜文字*
```
我是 _倾斜文字_
我是*倾斜文字*

#### 加粗倾斜
```
我是 ___加粗倾斜文字___
我是***加粗倾斜文字***
我是 __*加粗倾斜文字*__
```
我是 ___加粗倾斜文字___
我是***加粗倾斜文字***
我是 __*加粗倾斜文字*__

#### 块引用
```
> #### markdown语法!
>
> - 第一点好处.
> - 第二点好处.
>
> *任何* 一篇文档都是 **非常不错的**.
> > 我是嵌套文案
> > > 我是嵌套再嵌套的文案
```
> #### markdown语法!
>
> - 第一点好处.
> - 第二点好处.
>
> *任何* 一篇文档都是 **非常不错的**.
> > 我是嵌套文案
> > > 我是嵌套再嵌套的文案

#### 有序列表
```
1. 第1方案
2. 第2方案
    1. 第2.1方案
    2. 第2.2方案
3. 第3方案
```
1. 第1方案
2. 第2方案
    1. 第2.1方案
    2. 第2.2方案
3. 第3方案

#### 无序列表
```
* 无序列表1
+ 无序列表2
- 无序列表3
```
* 无序列表1
+ 无序列表2
- 无序列表3

#### 代码
```
`printf("hello world");`
```
`printf("hello world");`

#### 代码块
~~~
    #include<stdio.h>
    int main()
    {
	   printf("hello world");
	   return 0;
    }


```
#include<stdio.h>
int main()
{
	printf("hello world");
	return 0;
}
```
~~~
    #include<stdio.h>
    int main()
    {
	   printf("hello world");
	   return 0;
    }
```
#include<stdio.h>
int main()
{
	printf("hello world");
	return 0;
}
```

#### 分割线
```
下面是分割线1
***
下面是分割线2
---
下面是分割线3
_________________
-----------------
```
下面是分割线1
***
下面是分割线2
---
下面是分割线3
_________________
-----------------

#### 邮箱和URL
```
这是我的邮箱<internetwei@foxmail.com>
这是百度官网<https://www.baidu.com>
```
这是我的邮箱<internetwei@foxmail.com>
这是百度官网<https://www.baidu.com>
     
#### 链接
```
[百度官网](https://www.baidu.com)
[百度官网](https://www.baidu.com, '当鼠标悬停在文字上方时，你会看到它')
这是一个**[高亮链接](https://www.baidu.com)**
```
[百度官网](https://www.baidu.com)
[百度官网](https://www.baidu.com, '当鼠标悬停在文字上方时，你会看到它')
这是一个**[高亮链接](https://www.baidu.com)**

#### 图片
```
![阿西河图片](https://p.upyun.com/docs/cloud/demo.jpg "当鼠标悬停在文字上方时，你会看到它")
```
![阿西河图片](https://p.upyun.com/docs/cloud/demo.jpg "当鼠标悬停在文字上方时，你会看到它")


扩展语法
===============

#### 列表
```
左对齐 | 中间对齐 | 右对齐
:--- | :---: | ---:
markdown | 非常 &#124; 棒 | 右对齐
markdown | 专业 &#124; 的 | 右对齐
```
左对齐 | 中间对齐 | 右对齐
:--- | :---: | ---:
markdown | 非常 &#124; 棒 | 右对齐
markdown | 专业 &#124; 的 | 右对齐

#### 代码高亮
~~~
``` Javascript
if (true){ 
    return true;
} 
```
~~~

``` Javascript
if (true){ 
    return true;
} 
```

![代码高亮关键字](https://gitee.com/internetWei/images/raw/master/uPic/202103090003.png)

#### 标题ID
```
* [目录1](#标题1)
* [目录2](#标题2)

##### 标题1
文字内容
##### 标题2
文字内容
```
* [目录1](#标题1)
* [目录2](#标题2)

##### 标题1
文字内容
##### 标题2
文字内容

#### 脚注
```
脚注1 [^1 ]
脚注2 [^2 ]

[^1 ]: 脚注1内容
[^2 ]: 脚注2内容
```
脚注1 [^1 ]
脚注2 [^2 ]

[^1 ]: 脚注1内容
[^2 ]: 脚注2内容

#### 任务列表
```
- [x] 这是完成的任务
- [ ] 这是没有完成的任务
```
- [x] 这是完成的任务
- [ ] 这是没有完成的任务

#### 删除线
```
这是 ~~删除线~~
```
这是 ~~删除线~~

配合HTML
===============

#### 字体
```
<font face="黑体" size=5>我是黑体字</font>
<font face="微软雅黑" size=5>我是微软雅黑</font>
<font face="STCAIYUN" size=5>我是华文彩云</font>
```
<font face="黑体" size=5>我是黑体字</font>
<font face="微软雅黑" size=5>我是微软雅黑</font>
<font face="STCAIYUN" size=5>我是华文彩云</font>

#### 颜色
```
<font color=#0099ff size=5>color=#0099ff</font>
<font color=#00ffff size=5>color=#00ffff</font>
<font color=gray size=5>color=gray</font>
```
<font color=#0099ff size=5>color=#0099ff</font>
<font color=#00ffff size=5>color=#00ffff</font>
<font color=gray size=5>color=gray</font>

#### 上标
```
n<sup>2</sup>=n+1
```
n<sup>2</sup>=n+1

#### 下标
```
H<sub>2</sub>O
```
H<sub>2</sub>O

#### 特殊字符

由于图片太长，这里就不显示了，感兴趣的朋友可以[点击链接查看](https://gitee.com/internetWei/images/raw/master/uPic/202103090020.png)

#### 背景颜色
```
<p style="background-color: #b6ffc8;">绿色</p>
<p style="center;background-color: #ffb6d2;">红色</p>
<p style="right;background-color: #b6d7ff;">蓝色</p>
```
<p style="background-color: #b6ffc8;">绿色</p>
<p style="center;background-color: #ffb6d2;">红色</p>
<p style="right;background-color: #b6d7ff;">蓝色</p>

#### 对齐方式
```
<p style="text-align: left;background-color: #b6ffc8;">左对齐</p>
<p style="text-align: center;background-color: #ffb6d2;">中间居中</p>
<p style="text-align: right;background-color: #b6d7ff;">右对齐</p>
```
<p style="text-align: left;background-color: #b6ffc8;">左对齐</p>
<p style="text-align: center;background-color: #ffb6d2;">中间居中</p>
<p style="text-align: right;background-color: #b6d7ff;">右对齐</p>

#### 图片居中并自定义尺寸
```
<div style="text-align: center;background-color: #b6ffc8;">
    <img src="https://p.upyun.com/docs/cloud/demo.jpg" 
        alt="阿西河图片" 
        title="阿西河图片Title" 
        style="width:300px;height:300px;"
        >
</div>
```
<div style="text-align: center;background-color: #b6ffc8;">
    <img src="https://p.upyun.com/docs/cloud/demo.jpg" 
        alt="阿西河图片" 
        title="阿西河图片Title" 
        style="width:300px;height:300px;"
        >
</div>


#### 插入音乐
```
<iframe 
    frameborder="no" 
    border="0" 
    marginwidth="0" 
    marginheight="0" 
    width=100% 
    height=auto 
    src="//music.163.com/outchain/player?type=2&id=528478901&auto=1&height=66">
</iframe>
```
<iframe 
    frameborder="no" 
    border="0" 
    marginwidth="0" 
    marginheight="0" 
    width=100% 
    height=auto 
    src="//music.163.com/outchain/player?type=2&id=528478901&auto=1&height=66">
</iframe>

#### 插入视频
```
<iframe 
    src="//player.bilibili.com/player.html?aid=10631344&cid=17548810&page=1" 
    scrolling="no" 
    style="border:0;width:100%;height:auto;min-height:790px;"
    allowfullscreen="true"> 
</iframe>
```
<iframe 
    src="//player.bilibili.com/player.html?aid=10631344&cid=17548810&page=1" 
    scrolling="no" 
    style="border:0;width:100%;height:auto;min-height:790px;"
    allowfullscreen="true"> 
</iframe>