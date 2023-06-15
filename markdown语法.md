---
title: "我的文档"
author: "作者名"
date: "2021-01-01"
header-includes:
    - \usepackage{fancyhdr}
    - \pagestyle{fancy}
    - \fancyhf{}
    - \lhead{左侧页眉内容}
    - \rhead{右侧页眉内容}
---

# 正文内容

# 标题1 #

## 标题2 ## 

### 标题3 ### 

> 这是一段引用 > 

#### 这是一个有序列表 1. 

1. 第一步

2. 第二步

#### 这是一个无序列表 -

- 无序中。。。
- 2
- 4

#### 任务类 - [ ] or - [x]

- [ ] 吃饭
- [ ] 睡觉
- [x] 打豆豆

#### 列表嵌套 三个空格 或shift+回车

1. 第一   
   2. 第一   

#### 代码块 ```java

```jade
//```java     ```+语言
    public static void main(String[] args) {
    	int i = 1;
    	System.out.println(i);
}
```

#### 数学公式 $$   $$

$$
\frac{\partial f}{\partial x} = 2\sqrt{a}x
$$
#### 表格：|表头|              |:---|左对齐     |---:|右对齐    |:---:|居中对齐     |单元格|

| 姓名 | 年龄 | 成绩 |
| :--- | ---: | :--: |
| 张三 |   19 |  99  |
| 李四 |   18 |  80  |

#### 脚注

上脚注[^一键三连]



#### 横线 --- 

---

哈哈哈（横线作用：分隔文章）

[id]:baidu.com "一个搜索引擎"

[^一键三连]: 三连

#### 链接 

[百度](baidu.com "一个搜索引擎")

[百度][id] 给他一个id 以后经常用这个链接的话直接写id就行

请参考[标题1](#标题1)  链接标题 点击跳转到标题

URL:

http://www.baidu.com

链接图片! [解释](图片路径)

不推荐：

<img src="C:\Users\^\Pictures\桌面\1.png" style="zoom:11%;"/>

推荐使用这种方式引入图片：

![图片](C:\Users\^\Pictures\桌面\1.png)

*斜体*  ，    **加粗** ， `行内代码`  ，<u>下划线</u> :smile:，~~删除线~~ ，***加粗倾斜***

```text
*斜体*  ，    **加粗** ， `行内代码`  ，<u>下划线</u> :smile:，~~删除线~~ ，***加粗倾斜***
```

插入音频



$\theta=x$2$，

H~2~O

 <mark>这是一段高亮文字</mark> 

```text
 <mark>这是一段高亮文字</mark> 
```

<font face="微软雅黑">我是微软雅黑</font>
<font color=red>我是红色</font>
<font face="黑体" color=green size=5>我是黑体，绿色，尺寸为5</font>

```text
<font face="微软雅黑">我是微软雅黑</font>
<font color=red>我是红色</font>
<font face="黑体" color=green size=5>我是黑体，绿色，尺寸为5</font>
```

<table><tr><td bgcolor=green>背景色green</td></tr></table>

```text
<table><tr><td bgcolor=yellow>背景色yellow</td></tr></table>
```

<p align="right">右对齐</p>

```text
<p align="right">右对齐</p>
```

## TOC[#](https://www.cnblogs.com/librarookie/p/15429262.html#toc)

> TOC 全称为 Table of Content，自动列出全部标题。

- 用法：
  `[toc]`

  在 Markdown 中，自动生成目录非常简单，只需要在恰当的位置添加 `[TOC]` 符号，凡是以 # 定义的标题都会被编排到目录中。

  note:

  1. 如果你想要在你的 `TOC` 中排除一个标题，请在你的标题 `后面` 添加 `{ignore=true}` 即可
  2. 部分平台支持，已测试`typora`支持， `vscode-Markdown TOC`插件支持（`vscode原生Markdown`不支持）

## 插入音频

`<iframe height=498 width=510 src="">`

`<audio id="audio" controls="" preload="none">
      <source id="mp3" src="音频路径">
</audio>`

<iframe src="//player.bilibili.com/player.html?aid=226837727&bvid=BV17h41137i4&cid=1077691440&page=67&autoplay=false" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

&autoplay=false：禁止自动播放

