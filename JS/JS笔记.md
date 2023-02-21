## 1. js的三种引入方式

**行内式，内嵌式，引入式**

**html：**

```html
<!-- 内嵌式 -->
<script type="text/javascript">alert('点秋香')</script>

<!-- 引入式 -->
<script type="text/javascript" src="./3_14.js"></script>
	
<!-- 行内式 --><!-- onclick:点击  alert：弹出框-->
<input type="button" value="唐伯虎" onclick="alert('点秋香')"
```

**JS ：**

```javascript
alert('点秋香')
```

## 2. 注释

单行注释以` //` 开头

多行注释以` /* `开始，以 `*/` 结尾

## 3. 输入与输出

```javascript
//这是一个输入框
prompt('请输入你的年龄：');

//这是一个输出框
alert('威海加油，疫情终将战胜');

//控制台.日志(输出)，给程序员看的
console.log('程序员看的哦');

var age = 18;
console.log(age);//18

//1.用户输入姓名  存储到一个myname的变量里面
var myname = prompt('请输入您的名字：');
//2.输出这个用户名
alert(myname);	
```

## 4. 变量

- 变量必须以字母开头
- 变量也能以 $ 和 _ 符号开头（不过我们不推荐这么做）
- 变量名称对大小写敏感（y 和 Y 是不同的变量）

JavaScript 变量有很多种类型，但是现在，我们只关注数字和字符串。 

### 4.1 一条语句，多个变量*

```javascript
//可以在一条语句中声明很多变量。该语句以 var 开头，并使用逗号分隔变量即可：
var lastname="Doe", age=30, job="carpenter";
//声明也可横跨多行：
var lastname="Doe",
age=30,
job="carpenter";
//一条语句中声明的多个变量不可以同时赋同一个值:
var x,y,z=1;
//x,y 为 undefined， z 为 1。
```

### 4.2 未声明值的变量

```javascript
//声明无值的变量。未使用值来声明的变量，其值实际上是 undefined。
var carname;
```

### 4.3 重新声明变量

```javascript
//如果重新声明 JavaScript 变量，该变量的值不会丢失：
//在以下两条语句执行后，变量 carname 的值依然是 "Volvo"：
var carname="Volvo";
var carname;
//允许重复声明变量，后声明的覆盖之前的
var a = 1;
var a = 'x';
console.log(a);
// 输出 'x'
```

### 4.4 使用赋值运算符给变量赋值

```javascript
var x = 5
var y = 6
var z = (x + y) * 10
```

## 数据类型

**JavaScript 拥有动态类型** 

```javascript
//JavaScript 拥有动态类型。这意味着相同的变量可用作不同的类型：
var x;               // x 为 undefined
var x = 5;           // 现在 x 为数字
var x = "John";      // 现在 x 为字符串
```

 ***\*typeof检查类型\****

```javascript
typeof "John"                // 返回 string
typeof 3.14                  // 返回 number
typeof false                 // 返回 boolean
typeof [1,2,3,4]             // 返回 object
typeof {name:'John', age:34} // 返回 object
```

![img](https://img-blog.csdnimg.cn/60e584dbc1dc42db8bb8bebefe6ed8ef.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qmY54S2YA==,size_20,color_FFFFFF,t_70,g_se,x_16) **toString() 方法可以把数值转换为字符串** 

```javascript
var a=100;
var c=a.toString();
alert(typeof(c));      //typeof()方法验证转换后的数据类型
```

**把字符串转换为数值** 

parseInt() 和 parseFloat()

```javascript
var str="123.30";
var a=parseInt(str);    //parseInt()方法把字符串转换为整数，parseFloat()方法把字符串转换为浮点数
var b=parseFloat(str);
```

**转换为数字型** 

Number()和算数隐式转换

```javascript
var str = '123';
console.log(Number(str));//1
console.log(Number('12'));//2
//运用算数+-*/实现隐式转换
console.log('12' - 0);
```

**转换为布尔类型** 

Boolean() **ps**: 代表空 否定的值会转换为**false** **如**：0，NaN，null，underfined

![img](https://img-blog.csdnimg.cn/8cd92b2c209041a48046ecb42b01de55.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qmY54S2YA==,size_18,color_FFFFFF,t_70,g_se,x_16) 

**Undefined 和 Null**

Undefined： 这个值表示变量不含有值

Null：      可以通过将变量的值设置为 null 来清空变量

null 用于对象, undefined 用于变量，属性和方法

对象只有被定义才有可能为 null，否则为 undefined

 ***\*字符串\****

```javascript
//字符串可以是插入到引号中的任何字符。你可以使用单引号或双引号：
var carname = "Volvo XC60";
var carname = 'Volvo XC60';
//字符串长度
//可以使用内置属性 length 来计算字符串的长度：
var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
var sln = txt.length;
```

 反斜杠**(\)**是一个转义字符。 转义字符将特殊字符转换为字符串字符：

![img](https://img-blog.csdnimg.cn/51cf59af60254abda62cbf59b480c6e3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qmY54S2YA==,size_20,color_FFFFFF,t_70,g_se,x_16)

##  运算符 

***\*算术运算符\****

y=5，下面的表格解释了这些算术运算符：

![img](https://img-blog.csdnimg.cn/98301b45528048609458a12311f36325.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qmY54S2YA==,size_20,color_FFFFFF,t_70,g_se,x_16)

 ***\*赋值运算符\****

 给定 **x=10** 和 **y=5**，下面的表格解释了赋值运算符：

![img](https://img-blog.csdnimg.cn/0f55c1ce07874b4b982f1a1c63a84b79.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qmY54S2YA==,size_20,color_FFFFFF,t_70,g_se,x_16)***\*字符串的 + 运算符\**** 

```javascript
var txt1="What a very";
var txt2="nice day";
var txt3=txt1+txt2;
//txt3 运算结果:What a verynice day
```

 如果把数字与字符串相加，结果将成为字符串！

 ***\*比较运算符\****

x=5，下面的表格解释了比较运算符：

![img](https://img-blog.csdnimg.cn/e14325807ade4a9182f27e6c73a0e718.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qmY54S2YA==,size_20,color_FFFFFF,t_70,g_se,x_16)

***\*逻辑运算符\**** 
 给定 x=6 以及 y=3，下表解释了逻辑运算符：

![img](https://img-blog.csdnimg.cn/0ecc938d164841358939f684c4df8b98.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qmY54S2YA==,size_20,color_FFFFFF,t_70,g_se,x_16) ***\*条件运算符\****

其他数据类型转换为布尔类型的规则: null、undefined、0、NaN、空字符串转换为false，其他转化为 true。

```javascript
//如果变量 age 中的值小于 18，则向变量 voteable 赋值 "年龄太小"，否则赋值 "年龄已达到"。
voteable=(age<18)?"年龄太小":"年龄已达到";
```

## 条件语句 

- **if 语句** - 只有当指定条件为 true 时，使用该语句来执行代码
- **if...else 语句** - 当条件为 true 时执行代码，当条件为 false 时执行其他代码
- **if...else if....else 语句**- 使用该语句来选择多个代码块之一来执行
- **switch 语句** - 使用该语句来选择多个代码块之一来执行

***\*if 语句\**** 

```javascript
//当时间小于 20:00 时，生成问候 "Good day"：
if (time<20){
    x="Good day";
}
```

***\*if...else语句\****

```javascript
//当时间小于 20:00 时，生成问候 "Good day"，否则生成问候 "Good evening"。
if (time<20) {
      x="Good day";
  } else {
    x="Good evening";
  }
//x 的结果是：Good evening
```

**if...else if...else 语句** 

使用 if....else if...else 语句来选择多个代码块之一来执行。

***\*语法\****

if (*condition1*)
{
  *当条件 1 为 true 时执行的代码*
}
else if (*condition2*)
{
  *当条件 2 为 true 时执行的代码*
}
else
{
 *当条件 1 和 条件 2 都不为 true 时执行的代码*
}

```javascript
//如果时间小于 10:00，则生成问候 "Good morning"，如果时间大于 10:00 小于 20:00，则生成问候 "Good day"，否则生成问候 "Good evening"：
if (time<10)
{
    document.write("<b>早上好</b>");
}
else if (time>=10 && time<20)
{
    document.write("<b>今天好</b>");
}
else
{
    document.write("<b>晚上好!</b>");
}
//x 的结果是：晚上好!
```

***\*switch语句\**** 

**语法**

switch(n) {

  case 1: 执行代码块 1

 break;

  case 2: 执行代码块 2

 break;

  default: 与 case 1 和 case 2 不同时执行的代码

}