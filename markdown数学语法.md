# 一、数学公式环境

## 1. 行中公式与独立公式

`$<rquation>$`：在任意位置插入公式
 `$$<equation>$$`：单独一行显示公式

## 2. 单行公式

```ruby
\begin{equation}
公式 \label{<自定义的公式名>}
\end{equation}
```

在该环境中的公式会自动编号，如果不想公式自动编号，可以在公式后面使用`\notag`取消编号或者使用`\tag{<公式序号>}`自定义编号
 如果想要得到一个不加序号的公式环境，请使用`equation*`作为环境名或直接使用`$$<equation>$$`
 `\label{<自定义的公式名>}`是交叉引用，我们可以使用`\ref{<自定义的公式名>}`或`\eqref{<自定义的公式名>}`在文章的任何地方引用对应公式的序号

## 3. 多行公式

```xml
\begin{align}
公式一\\ \label{<自定义的公式名>}
公式二\\ \label{<自定义的公式名>}
公式三\\ \label{<自定义的公式名>}
...................
\end{align}
```

该环境可以写多行公式，且自动为公式标号，同样可以使用交叉引用。也可以通过`\notag`取消编号或`\tag{<公式序号>}`自定义编号
 在该环境中，我们可以使用`\\`来换行,区分不同的公式。也可以使用`&`使公式或式子对齐。
 常用的是将&放在每一个公式的等号前，使每一个公式的等号对齐。或将他用在公式与公式之间，用来对齐公式，此时&相当于类似制表位的东西。
 同样，`align`也对应`align*`这样的不产生序号的环境

## 4. 数组、矩阵和分段函数

### 4.1 数组

```cpp
\begin{array}{cccc}
x_{11} & x_{12} & \ldots & x_{1n}\\
x_{21} & x_{22} & \ldots & x_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
x_{n1} & x_{n2} & \ldots & x_{nn}\\
\end{array}
```

array环境用于生成一个数组，{}中4个c表示该数组有四列，且c的意思是居中对齐，还可以改为l和r。
 在环境中用`&`分隔不同的数组元素，用`\\`为数组换行
 ![\begin{array}{cccc} x_{11} & x_{12} & \ldots & x_{1n}\\ x_{21} & x_{22} & \ldots & x_{2n}\\ \vdots & \vdots & \ddots & \vdots\\ x_{n1} & x_{n2} & \ldots & x_{nn}\\ \end{array}](https://math.jianshu.com/math?formula=%5Cbegin%7Barray%7D%7Bcccc%7D%20x_%7B11%7D%20%26%20x_%7B12%7D%20%26%20%5Cldots%20%26%20x_%7B1n%7D%5C%5C%20x_%7B21%7D%20%26%20x_%7B22%7D%20%26%20%5Cldots%20%26%20x_%7B2n%7D%5C%5C%20%5Cvdots%20%26%20%5Cvdots%20%26%20%5Cddots%20%26%20%5Cvdots%5C%5C%20x_%7Bn1%7D%20%26%20x_%7Bn2%7D%20%26%20%5Cldots%20%26%20x_%7Bnn%7D%5C%5C%20%5Cend%7Barray%7D)

### 4.2 分段函数

仍然使用array多行公式环境，配合`\left`和`\right.`使{ 的高度与公式进行自动匹配。



```ruby
$$Y=\left\{
\begin{array}{cc}
x+1&x<10\\
x+2&x≥10\\
\end{array}\right.
$$
```

![Y=\left\{ \begin{array}{cc} x+1&x<10\\ x+2&x≥10\\ \end{array}\right.](https://math.jianshu.com/math?formula=Y%3D%5Cleft%5C%7B%20%5Cbegin%7Barray%7D%7Bcc%7D%20x%2B1%26x%3C10%5C%5C%20x%2B2%26x%E2%89%A510%5C%5C%20%5Cend%7Barray%7D%5Cright.)

### 4.3 矩阵

**无边框矩阵**



```ruby
\begin{matrix}
x_{11} & x_{12} & \ldots & x_{1n}\\
x_{21} & x_{22} & \ldots & x_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
x_{n1} & x_{n2} & \ldots & x_{nn}\\
\end{matrix}
```

![\begin{matrix} x_{11} & x_{12} & \ldots & x_{1n}\\ x_{21} & x_{22} & \ldots & x_{2n}\\ \vdots & \vdots & \ddots & \vdots\\ x_{n1} & x_{n2} & \ldots & x_{nn}\\ \end{matrix}](https://math.jianshu.com/math?formula=%5Cbegin%7Bmatrix%7D%20x_%7B11%7D%20%26%20x_%7B12%7D%20%26%20%5Cldots%20%26%20x_%7B1n%7D%5C%5C%20x_%7B21%7D%20%26%20x_%7B22%7D%20%26%20%5Cldots%20%26%20x_%7B2n%7D%5C%5C%20%5Cvdots%20%26%20%5Cvdots%20%26%20%5Cddots%20%26%20%5Cvdots%5C%5C%20x_%7Bn1%7D%20%26%20x_%7Bn2%7D%20%26%20%5Cldots%20%26%20x_%7Bnn%7D%5C%5C%20%5Cend%7Bmatrix%7D)
 可以看到，matrix和array环境其实是几乎一样的,只是不能使用{cccc}等类似选项。
 **有边框矩阵**
 我们可以将环境变量`matrix`更换为方边框`bmatrix`,行列式边框`vmatrix`,中括号边框`pmatrix`,大括号边框`Bmatrix`直接生成对应的边框。无需像array环境一样使用\left和\right去匹配括号的高度

### 4.4 高斯—若尔当型矩阵

因为`matrix`等矩阵专用环境不能带类似{c|cc}的参数，所以我们只能使用`array`环境来设置高斯若尔当矩阵中的分隔线。



```cpp
\begin{array}{ccc|c}
x_{11} & x_{12} & \ldots & x_{1n}\\
x_{21} & x_{22} & \ldots & x_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
x_{n1} & x_{n2} & \ldots & x_{nn}\\
\end{array}
```

![\begin{array}{ccc|c} x_{11} & x_{12} & \ldots & x_{1n}\\ x_{21} & x_{22} & \ldots & x_{2n}\\ \vdots & \vdots & \ddots & \vdots\\ x_{n1} & x_{n2} & \ldots & x_{nn}\\ \end{array}](https://math.jianshu.com/math?formula=%5Cbegin%7Barray%7D%7Bccc%7Cc%7D%20x_%7B11%7D%20%26%20x_%7B12%7D%20%26%20%5Cldots%20%26%20x_%7B1n%7D%5C%5C%20x_%7B21%7D%20%26%20x_%7B22%7D%20%26%20%5Cldots%20%26%20x_%7B2n%7D%5C%5C%20%5Cvdots%20%26%20%5Cvdots%20%26%20%5Cddots%20%26%20%5Cvdots%5C%5C%20x_%7Bn1%7D%20%26%20x_%7Bn2%7D%20%26%20%5Cldots%20%26%20x_%7Bnn%7D%5C%5C%20%5Cend%7Barray%7D)
 {}中的`|`参数表示为该列增加竖直分割线

### 4.5 分块矩阵

分块矩阵不仅需要竖直分隔线，还需要水平分隔线。水平分割线我们可以使用`\hline`设置



```cpp
\begin{array}{ccc|c}
x_{11} & x_{12} & \ldots & x_{1n}\\
\hline
x_{21} & x_{22} & \ldots & x_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
x_{n1} & x_{n2} & \ldots & x_{nn}\\
\end{array}
```

![\begin{array}{ccc|c} x_{11} & x_{12} & \ldots & x_{1n}\\ \hline x_{21} & x_{22} & \ldots & x_{2n}\\ \vdots & \vdots & \ddots & \vdots\\ x_{n1} & x_{n2} & \ldots & x_{nn}\\ \end{array}](https://math.jianshu.com/math?formula=%5Cbegin%7Barray%7D%7Bccc%7Cc%7D%20x_%7B11%7D%20%26%20x_%7B12%7D%20%26%20%5Cldots%20%26%20x_%7B1n%7D%5C%5C%20%5Chline%20x_%7B21%7D%20%26%20x_%7B22%7D%20%26%20%5Cldots%20%26%20x_%7B2n%7D%5C%5C%20%5Cvdots%20%26%20%5Cvdots%20%26%20%5Cddots%20%26%20%5Cvdots%5C%5C%20x_%7Bn1%7D%20%26%20x_%7Bn2%7D%20%26%20%5Cldots%20%26%20x_%7Bnn%7D%5C%5C%20%5Cend%7Barray%7D)

注意如果想令这些带分割线的矩阵带上想要的边框，要使用\left和\right进行括号高度匹配，具体见最后一小节

### 4.6 在一行内显示矩阵

矩阵很大，如果我们想让他缩小便于在一行显示:



```ruby
这是一个行中矩阵的示例 $\bigl( \begin{smallmatrix} a & b \\ c & d \end{smallmatrix} \bigr)$ 。
```

![\bigl( \begin{smallmatrix} a & b \\ c & d \end{smallmatrix} \bigr)](https://math.jianshu.com/math?formula=%5Cbigl(%20%5Cbegin%7Bsmallmatrix%7D%20a%20%26%20b%20%5C%5C%20c%20%26%20d%20%5Cend%7Bsmallmatrix%7D%20%5Cbigr))
 `smallmatrix`是小型矩阵的环境参数，`\bigl`和`\bigr`是固定大小括号匹配

## 5. 括号的匹配

### 5.1 自动匹配

根据数学公式自动调整括号的高度
 `\left<左括号>`
 `\right<右括号>`
 \left和\right必须配对使用。如果只想要一边的括号，应该使用`\left.`或`\right.`

### 5.2 固定匹配

以下命令产生的都是固定高度的括号，我们可以人为进行选择:
 `\bigl`和 `\bigr`
 `\biggl` 和`\biggr`



```ruby
$(\bigl(\biggl( \biggr\}\bigr\}\}$
```

![(\bigl(\biggl( \biggr\}\bigr\}\}](https://math.jianshu.com/math?formula=(%5Cbigl(%5Cbiggl(%20%5Cbiggr%5C%7D%5Cbigr%5C%7D%5C%7D)
 它们不必配对使用

# 二. 微积分

## 1. 极限

```
\lim_{变量 \to 表达式} 表达式
```

## 2. 积分

`\int_积分下限^积分上限 {被积表达式}`
 `int`只是其中一个运算符，详情参考微积分运算符

## 3. 累加，累乘

使用 `\sum_{下标表达式}^{上标表达式} {累加表达式}` 来输入一个累加。
 与之类似，使用 `\prod` `\bigcup` `\bigcap` 来分别输入累乘、并集和交集。
 此类符号在行内显示时上下标表达式将会移至右上角和右下角。

# 三. 运算符和特殊字符

## 1. 上下标

`^`表示上标,`_`表示下标。如果上下标的内容多于一个字符，需要用`{}`将这些内容括成一个整体。上下标可以嵌套，也可以同时使用。
 `$2^{x^y}$`![2^{x^y}](https://math.jianshu.com/math?formula=2%5E%7Bx%5Ey%7D)
 `$H_2O$`![H_2O](https://math.jianshu.com/math?formula=H_2O)

## 2. 分式

```
$frac{分子}{分母}$
```

## 3. 开方

`$\sqrt [根指数,省略时为2]{被开方数}$`
 `$\sqrt[n]{3}$`![\sqrt[n]{3}](https://math.jianshu.com/math?formula=%5Csqrt%5Bn%5D%7B3%7D)

## 4. 省略号和点乘

| 命令   | 效果                                                      |
| ------ | --------------------------------------------------------- |
| \dot   | ![\dots](https://math.jianshu.com/math?formula=%5Cdots)   |
| \cdots | ![\cdots](https://math.jianshu.com/math?formula=%5Ccdots) |
| \vdot  | ![\vdots](https://math.jianshu.com/math?formula=%5Cvdots) |
| \ddot  | ![\ddots](https://math.jianshu.com/math?formula=%5Cddots) |
| \cdot  | ![\cdot](https://math.jianshu.com/math?formula=%5Ccdot)   |

## 5. "帽"运算符

常用的三个

| 命令    | 效果                                                         |
| ------- | ------------------------------------------------------------ |
| \vec{x} | ![\vec{x}](https://math.jianshu.com/math?formula=%5Cvec%7Bx%7D) |
| \bar{x} | ![\bar{x}](https://math.jianshu.com/math?formula=%5Cbar%7Bx%7D) |
| \hat{x} | ![\hat{x}](https://math.jianshu.com/math?formula=%5Chat%7Bx%7D) |

## 6．微积分运算符

|  输入   |                            显示                             |  输入  |                           显示                            |  输入  |                           显示                            |
| :-----: | :---------------------------------------------------------: | :----: | :-------------------------------------------------------: | :----: | :-------------------------------------------------------: |
|  \int   |    ![\int](https://math.jianshu.com/math?formula=%5Cint)    | \iint  |  ![\iint](https://math.jianshu.com/math?formula=%5Ciint)  | \iiint | ![\iiint](https://math.jianshu.com/math?formula=%5Ciiint) |
| \iiiint | ![\iiiint](https://math.jianshu.com/math?formula=%5Ciiiint) | \oint  |  ![\oint](https://math.jianshu.com/math?formula=%5Coint)  | \prime | ![\prime](https://math.jianshu.com/math?formula=%5Cprime) |
|  \lim   |    ![\lim](https://math.jianshu.com/math?formula=%5Clim)    | \infty | ![\infty](https://math.jianshu.com/math?formula=%5Cinfty) | \nabla | ![\nabla](https://math.jianshu.com/math?formula=%5Cnabla) |

## 7．关系运算符

|   输入   |                             显示                             |    输入    |                             显示                             |   输入    |                             显示                             |    输入    |                             显示                             |
| :------: | :----------------------------------------------------------: | :--------: | :----------------------------------------------------------: | :-------: | :----------------------------------------------------------: | :--------: | :----------------------------------------------------------: |
|   \pm    |     ![\pm](https://math.jianshu.com/math?formula=%5Cpm)      |   \times   |  ![\times](https://math.jianshu.com/math?formula=%5Ctimes)   |   \div    |    ![\div](https://math.jianshu.com/math?formula=%5Cdiv)     |    \mid    |    ![\mid](https://math.jianshu.com/math?formula=%5Cmid)     |
|  \nmid   |   ![\nmid](https://math.jianshu.com/math?formula=%5Cnmid)    |   \cdot    |   ![\cdot](https://math.jianshu.com/math?formula=%5Ccdot)    |   \circ   |   ![\circ](https://math.jianshu.com/math?formula=%5Ccirc)    |    \ast    |    ![\ast](https://math.jianshu.com/math?formula=%5Cast)     |
| \bigodot | ![\bigodot](https://math.jianshu.com/math?formula=%5Cbigodot) | \bigotimes | ![\bigotimes](https://math.jianshu.com/math?formula=%5Cbigotimes) | \bigoplus | ![\bigoplus](https://math.jianshu.com/math?formula=%5Cbigoplus) |    \leq    |    ![\leq](https://math.jianshu.com/math?formula=%5Cleq)     |
|   \geq   |    ![\geq](https://math.jianshu.com/math?formula=%5Cgeq)     |    \neq    |    ![\neq](https://math.jianshu.com/math?formula=%5Cneq)     |  \approx  | ![\approx](https://math.jianshu.com/math?formula=%5Capprox)  |   \equiv   |  ![\equiv](https://math.jianshu.com/math?formula=%5Cequiv)   |
|   \sum   |    ![\sum](https://math.jianshu.com/math?formula=%5Csum)     |   \prod    |   ![\prod](https://math.jianshu.com/math?formula=%5Cprod)    |  \coprod  | ![\coprod](https://math.jianshu.com/math?formula=%5Ccoprod)  | \backslash | ![\backslash](https://math.jianshu.com/math?formula=%5Cbackslash) |

## 8．集合运算符

|   输入    |                             显示                             |   输入    |                             显示                             |   输入    |                             显示                             |
| :-------: | :----------------------------------------------------------: | :-------: | :----------------------------------------------------------: | :-------: | :----------------------------------------------------------: |
| \emptyset | ![\emptyset](https://math.jianshu.com/math?formula=%5Cemptyset) |    \in    |     ![\in](https://math.jianshu.com/math?formula=%5Cin)      |  \notin   |  ![\notin](https://math.jianshu.com/math?formula=%5Cnotin)   |
|  \subset  | ![\subset](https://math.jianshu.com/math?formula=%5Csubset)  |  \supset  | ![\supset](https://math.jianshu.com/math?formula=%5Csupset)  | \subseteq | ![\subseteq](https://math.jianshu.com/math?formula=%5Csubseteq) |
| \supseteq | ![\supseteq](https://math.jianshu.com/math?formula=%5Csupseteq) |  \bigcap  | ![\bigcap](https://math.jianshu.com/math?formula=%5Cbigcap)  |  \bigcup  | ![\bigcup](https://math.jianshu.com/math?formula=%5Cbigcup)  |
|  \bigvee  | ![\bigvee](https://math.jianshu.com/math?formula=%5Cbigvee)  | \bigwedge | ![\bigwedge](https://math.jianshu.com/math?formula=%5Cbigwedge) | \biguplus | ![\biguplus](https://math.jianshu.com/math?formula=%5Cbiguplus) |

## 9．对数运算符

| 输入 |                         显示                          | 输入 |                        显示                         | 输入 |                        显示                         |
| :--: | :---------------------------------------------------: | :--: | :-------------------------------------------------: | :--: | :-------------------------------------------------: |
| \log | ![\log](https://math.jianshu.com/math?formula=%5Clog) | \lg  | ![\lg](https://math.jianshu.com/math?formula=%5Clg) | \ln  | ![\ln](https://math.jianshu.com/math?formula=%5Cln) |

## 10. 三角运算符

|   输入   |                             显示                             | 输入 |                         显示                          |   输入   |                             显示                             |
| :------: | :----------------------------------------------------------: | :--: | :---------------------------------------------------: | :------: | :----------------------------------------------------------: |
| 30^\circ | ![30^\circ](https://math.jianshu.com/math?formula=30%5E%5Ccirc) | \bot | ![\bot](https://math.jianshu.com/math?formula=%5Cbot) | \angle A | ![\angle A](https://math.jianshu.com/math?formula=%5Cangle%20A) |
|   \sin   |    ![\sin](https://math.jianshu.com/math?formula=%5Csin)     | \cos | ![\cos](https://math.jianshu.com/math?formula=%5Ccos) |   \tan   |    ![\tan](https://math.jianshu.com/math?formula=%5Ctan)     |
|   \csc   |    ![\csc](https://math.jianshu.com/math?formula=%5Ccsc)     | \sec | ![\sec](https://math.jianshu.com/math?formula=%5Csec) |   \cot   |    ![\cot](https://math.jianshu.com/math?formula=%5Ccot)     |

## 11. 逻辑运算符

|   输入   |                             显示                             |    输入    |                             显示                             |    输入     |                             显示                             |
| :------: | :----------------------------------------------------------: | :--------: | :----------------------------------------------------------: | :---------: | :----------------------------------------------------------: |
| \because | ![\because](https://math.jianshu.com/math?formula=%5Cbecause) | \therefore | ![\therefore](https://math.jianshu.com/math?formula=%5Ctherefore) |             |                                                              |
| \forall  | ![\forall](https://math.jianshu.com/math?formula=%5Cforall)  |  \exists   | ![\exists](https://math.jianshu.com/math?formula=%5Cexists)  | \not\subset | ![\not\subset](https://math.jianshu.com/math?formula=%5Cnot%5Csubset) |
|  \not<   |  ![\not<](https://math.jianshu.com/math?formula=%5Cnot%3C)   |   \not>    |  ![\not>](https://math.jianshu.com/math?formula=%5Cnot%3E)   |    \not=    |  ![\not=](https://math.jianshu.com/math?formula=%5Cnot%3D)   |

## 12. 连线符号

|                      输入                      |                             显示                             |
| :--------------------------------------------: | :----------------------------------------------------------: |
|                 \fbox{a+b+c+d}                 | ![\fbox{a+b+c+d}](https://math.jianshu.com/math?formula=%5Cfbox%7Ba%2Bb%2Bc%2Bd%7D) |
|            \overleftarrow{a+b+c+d}             | ![\overleftarrow{a+b+c+d}](https://math.jianshu.com/math?formula=%5Coverleftarrow%7Ba%2Bb%2Bc%2Bd%7D) |
|            \overrightarrow{a+b+c+d}            | ![\overrightarrow{a+b+c+d}](https://math.jianshu.com/math?formula=%5Coverrightarrow%7Ba%2Bb%2Bc%2Bd%7D) |
|          \overleftrightarrow{a+b+c+d}          | ![\overleftrightarrow{a+b+c+d}](https://math.jianshu.com/math?formula=%5Coverleftrightarrow%7Ba%2Bb%2Bc%2Bd%7D) |
|            \underleftarrow{a+b+c+d}            | ![\underleftarrow{a+b+c+d}](https://math.jianshu.com/math?formula=%5Cunderleftarrow%7Ba%2Bb%2Bc%2Bd%7D) |
|           \underrightarrow{a+b+c+d}            | ![\underrightarrow{a+b+c+d}](https://math.jianshu.com/math?formula=%5Cunderrightarrow%7Ba%2Bb%2Bc%2Bd%7D) |
|         \underleftrightarrow{a+b+c+d}          | ![\underleftrightarrow{a+b+c+d}](https://math.jianshu.com/math?formula=%5Cunderleftrightarrow%7Ba%2Bb%2Bc%2Bd%7D) |
|               \overline{a+b+c+d}               | ![\overline{a+b+c+d}](https://math.jianshu.com/math?formula=%5Coverline%7Ba%2Bb%2Bc%2Bd%7D) |
|              \underline{a+b+c+d}               | ![\underline{a+b+c+d}](https://math.jianshu.com/math?formula=%5Cunderline%7Ba%2Bb%2Bc%2Bd%7D) |
|          \overbrace{a+b+c+d}^{Sample}          | ![\overbrace{a+b+c+d}^{Sample}](https://math.jianshu.com/math?formula=%5Coverbrace%7Ba%2Bb%2Bc%2Bd%7D%5E%7BSample%7D) |
|         \underbrace{a+b+c+d}_{Sample}          | ![\underbrace{a+b+c+d}_{Sample}](https://math.jianshu.com/math?formula=%5Cunderbrace%7Ba%2Bb%2Bc%2Bd%7D_%7BSample%7D) |
|  \overbrace{a+\underbrace{b+c}_{1.0}+d}^{2.0}  | ![\overbrace{a+\underbrace{b+c}_{1.0}+d}^{2.0}](https://math.jianshu.com/math?formula=%5Coverbrace%7Ba%2B%5Cunderbrace%7Bb%2Bc%7D_%7B1.0%7D%2Bd%7D%5E%7B2.0%7D) |
| \underbrace{a\cdot a\cdots a}_{b\text{ times}} | ![\underbrace{a\cdot a\cdots a}_{b\text{ times}}](https://math.jianshu.com/math?formula=%5Cunderbrace%7Ba%5Ccdot%20a%5Ccdots%20a%7D_%7Bb%5Ctext%7B%20times%7D%7D) |
|            \underrightarrow{1℃/min}            | ![\underrightarrow{1℃/min}](https://math.jianshu.com/math?formula=%5Cunderrightarrow%7B1%E2%84%83%2Fmin%7D) |

## 13. 箭头符号

- 推荐使用符号：

|   输入   |                             显示                             |  输入   |                            显示                             |    输入    |                             显示                             |
| :------: | :----------------------------------------------------------: | :-----: | :---------------------------------------------------------: | :--------: | :----------------------------------------------------------: |
|   \to    |     ![\to](https://math.jianshu.com/math?formula=%5Cto)      | \mapsto | ![\mapsto](https://math.jianshu.com/math?formula=%5Cmapsto) |            |                                                              |
| \implies | ![\implies](https://math.jianshu.com/math?formula=%5Cimplies) |  \iff   |    ![\iff](https://math.jianshu.com/math?formula=%5Ciff)    | \impliedby | ![\impliedby](https://math.jianshu.com/math?formula=%5Cimpliedby) |

- 其它可用符号：

|        输入         |                             显示                             |        输入         |                             显示                             |
| :-----------------: | :----------------------------------------------------------: | :-----------------: | :----------------------------------------------------------: |
|      \uparrow       | ![\uparrow](https://math.jianshu.com/math?formula=%5Cuparrow) |      \Uparrow       | ![\Uparrow](https://math.jianshu.com/math?formula=%5CUparrow) |
|     \downarrow      | ![\downarrow](https://math.jianshu.com/math?formula=%5Cdownarrow) |     \Downarrow      | ![\Downarrow](https://math.jianshu.com/math?formula=%5CDownarrow) |
|     \leftarrow      | ![\leftarrow](https://math.jianshu.com/math?formula=%5Cleftarrow) |     \Leftarrow      | ![\Leftarrow](https://math.jianshu.com/math?formula=%5CLeftarrow) |
|     \rightarrow     | ![\rightarrow](https://math.jianshu.com/math?formula=%5Crightarrow) |     \Rightarrow     | ![\Rightarrow](https://math.jianshu.com/math?formula=%5CRightarrow) |
|   \leftrightarrow   | ![\leftrightarrow](https://math.jianshu.com/math?formula=%5Cleftrightarrow) |   \Leftrightarrow   | ![\Leftrightarrow](https://math.jianshu.com/math?formula=%5CLeftrightarrow) |
|   \longleftarrow    | ![\longleftarrow](https://math.jianshu.com/math?formula=%5Clongleftarrow) |   \Longleftarrow    | ![\Longleftarrow](https://math.jianshu.com/math?formula=%5CLongleftarrow) |
|   \longrightarrow   | ![\longrightarrow](https://math.jianshu.com/math?formula=%5Clongrightarrow) |   \Longrightarrow   | ![\Longrightarrow](https://math.jianshu.com/math?formula=%5CLongrightarrow) |
| \longleftrightarrow | ![\longleftrightarrow](https://math.jianshu.com/math?formula=%5Clongleftrightarrow) | \Longleftrightarrow | ![\Longleftrightarrow](https://math.jianshu.com/math?formula=%5CLongleftrightarrow) |

## 14. 希腊字母

|   输入   |                             显示                             |  输入   |                            显示                             |   输入   |                             显示                             |   输入   |                             显示                             |
| :------: | :----------------------------------------------------------: | :-----: | :---------------------------------------------------------: | :------: | :----------------------------------------------------------: | :------: | :----------------------------------------------------------: |
|  \alpha  |  ![\alpha](https://math.jianshu.com/math?formula=%5Calpha)   |    A    |        ![A](https://math.jianshu.com/math?formula=A)        |  \beta   |   ![\beta](https://math.jianshu.com/math?formula=%5Cbeta)    |    B     |        ![B](https://math.jianshu.com/math?formula=B)         |
|  \gamma  |  ![\gamma](https://math.jianshu.com/math?formula=%5Cgamma)   | \Gamma  |  ![\Gamma](https://math.jianshu.com/math?formula=%5CGamma)  |  \delta  |  ![\delta](https://math.jianshu.com/math?formula=%5Cdelta)   |  \Delta  |  ![\Delta](https://math.jianshu.com/math?formula=%5CDelta)   |
| \epsilon | ![\epsilon](https://math.jianshu.com/math?formula=%5Cepsilon) |    E    |        ![E](https://math.jianshu.com/math?formula=E)        |  \zeta   |   ![\zeta](https://math.jianshu.com/math?formula=%5Czeta)    |    Z     |        ![Z](https://math.jianshu.com/math?formula=Z)         |
|   \eta   |    ![\eta](https://math.jianshu.com/math?formula=%5Ceta)     |    H    |        ![H](https://math.jianshu.com/math?formula=H)        |  \theta  |  ![\theta](https://math.jianshu.com/math?formula=%5Ctheta)   |  \Theta  |  ![\Theta](https://math.jianshu.com/math?formula=%5CTheta)   |
|  \iota   |   ![\iota](https://math.jianshu.com/math?formula=%5Ciota)    |    I    |        ![I](https://math.jianshu.com/math?formula=I)        |  \kappa  |  ![\kappa](https://math.jianshu.com/math?formula=%5Ckappa)   |    K     |        ![K](https://math.jianshu.com/math?formula=K)         |
| \lambda  | ![\lambda](https://math.jianshu.com/math?formula=%5Clambda)  | \Lambda | ![\Lambda](https://math.jianshu.com/math?formula=%5CLambda) |   \mu    |     ![\mu](https://math.jianshu.com/math?formula=%5Cmu)      |    M     |        ![M](https://math.jianshu.com/math?formula=M)         |
|   \nu    |     ![\nu](https://math.jianshu.com/math?formula=%5Cnu)      |    N    |        ![N](https://math.jianshu.com/math?formula=N)        |   \xi    |     ![\xi](https://math.jianshu.com/math?formula=%5Cxi)      |   \Xi    |     ![\Xi](https://math.jianshu.com/math?formula=%5CXi)      |
|    o     |        ![o](https://math.jianshu.com/math?formula=o)         |    O    |        ![O](https://math.jianshu.com/math?formula=O)        |   \pi    |     ![\pi](https://math.jianshu.com/math?formula=%5Cpi)      |   \Pi    |     ![\Pi](https://math.jianshu.com/math?formula=%5CPi)      |
|   \rho   |    ![\rho](https://math.jianshu.com/math?formula=%5Crho)     |    P    |        ![P](https://math.jianshu.com/math?formula=P)        |  \sigma  |  ![\sigma](https://math.jianshu.com/math?formula=%5Csigma)   |  \Sigma  |  ![\Sigma](https://math.jianshu.com/math?formula=%5CSigma)   |
|   \tau   |    ![\tau](https://math.jianshu.com/math?formula=%5Ctau)     |    T    |        ![T](https://math.jianshu.com/math?formula=T)        | \upsilon | ![\upsilon](https://math.jianshu.com/math?formula=%5Cupsilon) | \Upsilon | ![\Upsilon](https://math.jianshu.com/math?formula=%5CUpsilon) |
|   \phi   |    ![\phi](https://math.jianshu.com/math?formula=%5Cphi)     |  \Phi   |    ![\Phi](https://math.jianshu.com/math?formula=%5CPhi)    |   \chi   |    ![\chi](https://math.jianshu.com/math?formula=%5Cchi)     |    X     |        ![X](https://math.jianshu.com/math?formula=X)         |
|   \psi   |    ![\psi](https://math.jianshu.com/math?formula=%5Cpsi)     |  \Psi   |    ![\Psi](https://math.jianshu.com/math?formula=%5CPsi)    |  \omega  |  ![\omega](https://math.jianshu.com/math?formula=%5Comega)   |  \Omega  |  ![\Omega](https://math.jianshu.com/math?formula=%5COmega)   |

# 五. 文档样式

## 1. 字体转换

若要对公式的某一部分字符进行字体转换，可以用 `{\字体 {需转换的部分字符}}` 命令，其中 `\字体` 部分可以参照下表选择合适的字体。一般情况下，公式默认为意大利体 ![italic](https://math.jianshu.com/math?formula=italic) 。
 如果直接使用`\字体`而没有{}的作用域，那么这条命令之后的字体都被转换
 示例中 **全部大写** 的字体仅大写可用。

| 输入 |   说明   |                             显示                             | 输入  |    说明    |                             显示                             |
| :--: | :------: | :----------------------------------------------------------: | :---: | :--------: | :----------------------------------------------------------: |
| \rm  |  罗马体  | ![\rm{Sample}](https://math.jianshu.com/math?formula=%5Crm%7BSample%7D) | \cal  |    花体    | ![\cal{SAMPLE}](https://math.jianshu.com/math?formula=%5Ccal%7BSAMPLE%7D) |
| \it  | 意大利体 | ![\it{Sample}](https://math.jianshu.com/math?formula=%5Cit%7BSample%7D) | \Bbb  |  黑板粗体  | ![\Bbb{SAMPLE}](https://math.jianshu.com/math?formula=%5CBbb%7BSAMPLE%7D) |
| \bf  |   粗体   | ![\bf{Sample}](https://math.jianshu.com/math?formula=%5Cbf%7BSample%7D) | \mit  |  数学斜体  | ![\mit{SAMPLE}](https://math.jianshu.com/math?formula=%5Cmit%7BSAMPLE%7D) |
| \sf  |  等线体  | ![\sf{Sample}](https://math.jianshu.com/math?formula=%5Csf%7BSample%7D) | \scr  |   手写体   | ![\scr{SAMPLE}](https://math.jianshu.com/math?formula=%5Cscr%7BSAMPLE%7D) |
| \tt  | 打字机体 | ![\tt{Sample}](https://math.jianshu.com/math?formula=%5Ctt%7BSample%7D) | \frak | 旧德式字体 | ![\frak{Sample}](https://math.jianshu.com/math?formula=%5Cfrak%7BSample%7D) |

## 2. 文字颜色

使用 `\color{颜色}{文字}` 来更改特定的文字颜色。
 更改文字颜色 **需要浏览器支持** ，如果浏览器不知道你所需的颜色，那么文字将被渲染为黑色。

对于较旧的浏览器（HTML4与CSS2），以下颜色是被支持的：

|  输入  |                             显示                             |  输入   |                             显示                             |
| :----: | :----------------------------------------------------------: | :-----: | :----------------------------------------------------------: |
| black  | ![\color{black}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bblack%7D%7Btext%7D) |  grey   | ![\color{grey}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bgrey%7D%7Btext%7D) |
| silver | ![\color{silver}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bsilver%7D%7Btext%7D) |  white  | ![\color{white}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bwhite%7D%7Btext%7D) |
| maroon | ![\color{maroon}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bmaroon%7D%7Btext%7D) |   red   | ![\color{red}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bred%7D%7Btext%7D) |
| yellow | ![\color{yellow}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Byellow%7D%7Btext%7D) |  lime   | ![\color{lime}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Blime%7D%7Btext%7D) |
| olive  | ![\color{olive}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bolive%7D%7Btext%7D) |  green  | ![\color{green}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bgreen%7D%7Btext%7D) |
|  teal  | ![\color{teal}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bteal%7D%7Btext%7D) |  auqa   | ![\color{auqa}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bauqa%7D%7Btext%7D) |
|  blue  | ![\color{blue}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bblue%7D%7Btext%7D) |  navy   | ![\color{navy}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bnavy%7D%7Btext%7D) |
| purple | ![\color{purple}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bpurple%7D%7Btext%7D) | fuchsia | ![\color{fuchsia}{text}](https://math.jianshu.com/math?formula=%5Ccolor%7Bfuchsia%7D%7Btext%7D) |

## 3. 在公式中添加文本

在公式环境中使用 `\text {文字}`可以添加文字
 在 `\text {文字}` 中仍可以使用 `$公式$` 插入其它公式。

## 4. 空格

| 输入   | 显示                                                         |
| ------ | ------------------------------------------------------------ |
| \quad  | ![a\quad a](https://math.jianshu.com/math?formula=a%5Cquad%20a) |
| \qquad | ![a\qquad a](https://math.jianshu.com/math?formula=a%5Cqquad%20a) |

## 5. 删除线

使用删除线功能必须声明 `$$` 符号。

在公式内使用 `\require{cancel}` 来允许 **片段删除线** 的显示。
 声明片段删除线后，使用 `\cancel{字符}`、`\bcancel{字符}`、`\xcancel{字符}` 和 `\cancelto{字符}` 来实现各种片段删除线效果。
 示例：
 `$\require{cancel}\cancel{a}$`
 ![\require{cancel}\cancel{a}](https://math.jianshu.com/math?formula=%5Crequire%7Bcancel%7D%5Ccancel%7Ba%7D)



作者：Bneh
链接：http://events.jianshu.io/p/f0de9f572c9d
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。