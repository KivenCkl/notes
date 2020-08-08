## 引言

在学习理工科知识或者是目前火热的深度学习等过程中，会涉及到大量的数学公式，并且考虑到准备以 Markdown 为主要做笔记方式，因此，在这里对 Markdown 中 LaTeX 数学公式命令做一个汇总。

> **重要说明：**示例中的 `$formula$` 是生成公式的 LaTeX 语法。

<!--more-->

## Markdown 中使用 LaTeX 基础语法

LaTeX 公式有两种，一种是用在正文中的，一种是单独显示的。

- 行内公式：用 `$formula$` 表示，例如: `$\sum_{i=0}^{n}i^2$` 表示 $\sum_{i=0}^{n}i^2$

- 独立公式：用 `$$formula$$` 表示，例如: `$$\sum_{i=0}^{n}i^2$$` 表示

$$\sum_{i=0}^{n}i^2$$

> **Tips:** 以下几个字符 # \$ % & ~ _ ^ \\ { } 有特殊意义，当表示这些字符时，需要转义，即在每个字符前加上 \\ ，对于 \\ ，可使用 `$\backslash$` 命令得到 $\backslash$ 。

## 常用数学表达命令

### 上下标表示

- 上标: 用 `^` 后的内容表示上标，例如: `$x^{(i)}$` , `$y^{(i)}$` 表示 $x^{(i)}$ , $y^{(i)}$

- 下标: 用 `_` 后的内容表示上标，例如: `$x_{(i)}$` , `$y_{(i)}$` 表示 $x_{(i)}$ , $y_{(i)}$

- 上下标混用，例如: `$x_1^2$` , `$x^{y^{z} }$` , `$x^{y_z}$` 表示 $x_1^2$, $x^{y^{z} }$, $x^{y_z}$

当角标位置看起来不明显时，可以强制改变角标层次或者在角标前面加上改变其大小的命令 (如 `\tiny` , `\small` , `\normalsize` , `\large` , `\Large` , `\LARGE` )，例如: `$y_N$` , `$y_{_N}$` , `$y_{\tiny{N} }$` 表示 $y_N$, $y_{_N}$, $y_{\tiny{N} }$

并且支持中文作为角标，例如: `${\partial f}_{\tiny极大值}$` , `${\partial f}_{\large 极大值}$` 表示 ${\partial f}_{\tiny 极大值}$, ${\partial f}_{\large 极大值}$

### 分数形式

分式命令:

- `$\dfrac{}{}$` , 表示该分式是以 displaystyle 设置的，例如: `$\dfrac{y}{x}$` 表示 $\dfrac{y}{x}$

- `$\tfrac{}{}$` , 表示该分式是以 textstyle 设置的，例如: `$\tfrac{y}{x}$` 表示 $\tfrac{y}{x}$

- `$\frac{}{}$` , 表示该分式根据环境设置样式，例如: `$\frac{y}{x}$` 表示 $\frac{y}{x}$

- `${}\over{}$` , 分式的另一种表达方式，一般不建议使用 (但是真的很方便啊)，例如: `$y \over x$` 表示 $y \over x$

其具体区别请参考：

[What is the difference between `\dfrac` and `\frac`?](https://tex.stackexchange.com/questions/135389/what-is-the-difference-between-dfrac-and-frac)

[What is the difference between `\over` and `\frac`?](https://tex.stackexchange.com/questions/73822/what-is-the-difference-between-over-and-frac)

连分式 `$x_0+\frac{1}{x_1+\frac{1}{x_2+\frac{1}{x_3+\frac{1}{x_4} } } }$` 表示 $x_0+\frac{1}{x_1+\frac{1}{x_2+\frac{1}{x_3+\frac{1}{x_4} } } }$

可以通过强制改变字体大小使得分子分母字体大小一致，例如: `$\newcommand{\FS}[2]{\dfrac{ #1}{ #2} }x_0+\FS{1}{x_1+\FS{1}{x_2+\FS{1}{x_3+\FS{1}{x_4} } } }$` 表示 $\newcommand{\FS}[2]{\dfrac{ #1}{ #2} }x_0+\FS{1}{x_1+\FS{1}{x_2+\FS{1}{x_3+\FS{1}{x_4} } } }$

其中第一行命令定义了一个新的分式命令，规定每个调用该命令的分式都按 `\displaystyle` 的格式显示分式，等价于 `$x_0+\dfrac{1}{x_1+\dfrac{1}{x_2+\dfrac{1}{x_3+\dfrac{1}{x_4} } } }$`

分数线长度值是预设为分子分母的最大长度，如果想要使分数线再长一点，可以在分子或分母两端添加一些间隔，例如: `$\frac{1}{2}$` , `$\frac{\;1\;}{\;2\;}$` 表示 $\frac{1}{2}$, $\frac{\;1\;}{\;2\;}$

### 根式

- 二次根式命令: `\sqrt{表达式}` , 例如: `$\sqrt{x}$` 表示 $\sqrt{x}$
- $n$次根式命令: `\sqrt[n]{表达式}` , 例如: `$\sqrt[3]{x}$` 表示 $\sqrt[3]{x}$

被开方表达式字符高度不一致时，根号上面的横线可能不在同一条直线上，可以在被开方表达式插入一个只有高度没有宽度的数学支柱 `\mathstrut` , 例如: `$\sqrt{a}+\sqrt{b}+\sqrt{c}$` , `$\sqrt{\mathstrut a}+\sqrt{\mathstrut b}+\sqrt{\mathstrut c}$` 表示 $\sqrt{a}+\sqrt{b}+\sqrt{c}$, $\sqrt{\mathstrut a}+\sqrt{\mathstrut b}+\sqrt{\mathstrut c}$

当开方次数的位置显得略低时，可以将开方改为上标，并拉近与根式的水平距离，即将命令中的 `[n]` 改为 `[^n\!]` (其中 ^ 表示是上标，! 表示缩小间隔), 例如: `$sqrt{1+\sqrt[p]{1+\sqrt[p]{1+a} } }$` , `$\sqrt{1+\sqrt[^p \!]{1+\sqrt[^p \!]{1+a} } }$` 表示 $\sqrt{1+\sqrt[p]{1+\sqrt[p]{1+a} } }$, $\sqrt{1+\sqrt[^p \!]{1+\sqrt[^p \!]{1+a} } }$

### 向量

使用 `\vec{矢量}` 来产生一个矢量，也可以使用 `\overrightarrow` 等命令自定义字母上方的符号，例如: `$$\vec{x} \quad \overleftarrow{xy} \quad \overleftrightarrow{xy} \quad \overrightarrow{xy}$$` 表示

$$
\vec{x} \quad \overleftarrow{xy} \quad \overleftrightarrow{xy} \quad \overrightarrow{xy}
$$

### 定界符 - 自适应放大命令

自适应放大命令: `\left` 和 `\right` , 本命令放在左右定界符前，随着公式内容大小自动调整符号大小，例如: `$\left(\frac{1}{xyz}\right)$` 表示 $\left(\frac{1}{xyz}\right)$

还有另外一种使用方式 `$() \big(\big) \Big(\Big) \Bigg(\Bigg)$` 表示 $() \big(\big) \Big(\Big) \Bigg(\Bigg)$

> **Tips:** `\left` 和 `\right` 需成对使用，只需要一边时，可用 `\left.` 或 `\right.` 进行配对，例如: `$\left.\frac{1}{2}x^2\right|_0^1$` 表示 $\left.\frac{1}{2}x^2\right|_0^1$ .

### 空白间距 - 占位宽度

`\quad` 代表当前字体下接近字符 `M` 的宽度。

| 符号               | 命令                 | 效果         |
| :----------------: | :------------------: | :----------: |
| 没有空格           | `$ab$`               | $ab$         |
| 紧贴，缩进1/6m宽度 | `$a\!b$`             | $a\!b$       |
| 小空格             | `$a\,b$`             | $a\,b$       |
| 1/3个空格          | `$a\ b$`             | $a\ b$       |
| 中等空格           | `$a\:b$` 或 `$a\;b$` | $a\;b$       |
| 一个空格           | `$a \quad b$`        | $a \quad b$  |
| 两个空格           | `$a \qquad b$`       | $a \qquad b$ |

### 省略号

省略号用 `\dots \cdots \vdot \ddots` 表示, `\dots` 和 `\cdots` 的纵向位置不同，前者一般用于有下标的序列，例如: `$$x_1, x_2, \dots, x_n \quad 1,2,\cdots,n \quad \vdots \quad \ddots$$` 表示

$$
x_1, x_2, \dots, x_n \quad 1,2,\cdots,n \quad \vdots \quad \ddots
$$

### 多行公式

#### 长公式

无须对齐可使用 `multline` , 需要对齐使用 `split` , 用 `\\` 和 `&` 来分行和设置对齐的位置，例如:

```latex
$$
\begin{multline}
x=a+b+c+{} \\
d+e+f+g
\end{multline}
$$
```

$$
\begin{multline}
x=a+b+c+{} \\
d+e+f+g
\end{multline}
$$

```latex
$$
\begin{split}
x={}$a+b+c+{} \\
$d+e+f+g
\end{split}
$$
```

$$
\begin{split}
x={}&a+b+c+{} \\
&d+e+f+g
\end{split}
$$

#### 公式组

不需要对齐的公式组用 `gather` , 需要对齐使用 `align` , 例如:

```latex
$$
\begin{gather}
a=b+c+d \\
x=y+z
\end{gather}
$$
```

$$
\begin{gather}
a=b+c+d \\
x=y+z
\end{gather}
$$

```latex
$$
\begin{align}
a &=b+c+d \\
x &=y+z
\end{align}
$$
```

$$
\begin{align}
a &=b+c+d \\
x &=y+z
\end{align}
$$

#### 分支公式

分段函数通常用 `cases` , 例如:

```latex
$$
y=\begin{cases}
-x,\quad &x \leq 0 \\
x, &x>0
\end{cases}
$$
```

$$
y=\begin{cases}
-x,\quad &x \leq 0 \\
x, &x>0
\end{cases}
$$

#### 公式编号

自动编号的公式可以用如下方法表示：

```latex
$$
\begin{equation}
E=mc^2
\end{equation}
$$
```

$$
\begin{equation}
E=mc^2
\end{equation}
$$

也可以通过命令 `\tag{n}` 手动为公式编号，并使用 `\label{}` 指令埋下锚点，例如:

```latex
$$
S(r_k)  = \sum_{r_k \ne r_i} \text{exp}(\frac{-D_s(r_k, r_i)}{\sigma_s^2})\label{eq:test}\tag{1.1}
$$
```

$$
S(r_k)  = \sum_{r_k \ne r_i} \text{exp}(\frac{-D_s(r_k, r_i)}{\sigma_s^2})\label{eq:test}\tag{1.1}
$$

使用 `\eqref` 指令引用前面埋下的锚点， `$\eqref{eq:test}$` 将显示为 $\eqref{eq:test}$

#### 取消公式编号

可在公式后加上 `\nonumber` 命令取消公式编号，例如:

```latex
$$
\begin{align}
a &=b+c+d \nonumber \\
x &=y+z \nonumber
\end{align}
$$
```

$$
\begin{align}
a &=b+c+d \nonumber \\
x &=y+z \nonumber
\end{align}
$$

对于公式组，可采用 `aligned` 不对公式编号，例如:

```latex
$$
\begin{aligned}
a &=b+c+d \\
x &=y+z
\end{aligned}
$$
```

$$
\begin{aligned}
a &=b+c+d \\
x &=y+z
\end{aligned}
$$

### 上下水平线

- `\overline{表达式}` : 在表达式上方画出水平线，例如: `$\overline{x+y}$` 表示 $\overline{x+y}$
- `\underline{表达式}` : 在表达式下方画出水平线，例如: `$\underline{x+y}$` 表示 $\underline{x+y}$

### 上下大括号

- `\overbrace{表达式}` : 在表达式上方画出一个水平的大括号，例如: `$\overbrace{1+2+3+\cdots+100}^{100}$` 表示 $\overbrace{1+2+3+\cdots+100}^{100}$
- `\underbrace{表达式}` : 在表达式下方画出一个水平的大括号，例如: `$\underbrace{1+2+3+\cdots+100}_{100}$` 表示 $\underbrace{1+2+3+\cdots+100}_{100}$

### 矩阵

生成矩阵的命令中每一行以 `\\` 结束，矩阵的元素之间用 `&` 来分隔开，例如:

```latex
$$
\begin{matrix}
x_{_{11} } & x_{_{12} } & \dots & x_{_{1n} } \\
x_{_{21} } & x_{_{22} } & \dots & x_{_{2n} } \\
\vdots & \vdots & \ddots  & \vdots  \\
x_{_{m1} } & x_{_{m2} } & \dots & x_{_{mn} } \\
\end{matrix}
$$
```

$$
\begin{matrix}
x_{_{11} } & x_{_{12} } & \dots & x_{_{1n} } \\
x_{_{21} } & x_{_{22} } & \dots & x_{_{2n} } \\
\vdots & \vdots & \ddots  & \vdots  \\
x_{_{m1} } & x_{_{m2} } & \dots & x_{_{mn} } \\
\end{matrix}
$$

带各类不同边界的矩阵:

```latex
$$
\begin{pmatrix} a & b \\ c & d \\ \end{pmatrix} \quad
\begin{bmatrix} a & b \\ c & d \\ \end{bmatrix} \quad
\left[ \begin{matrix} a & b \\ c & d \\ \end{matrix} \right] \quad
\begin{Bmatrix} a & b \\ c & d \\ \end{Bmatrix} \quad
\left\{ \begin{matrix} a & b \\ c & d \\ \end{matrix} \right\} \quad
\begin{vmatrix} a & b \\ c & d \\ \end{vmatrix} \quad
\begin{Vmatrix} a & b \\ c & d \\ \end{Vmatrix} \quad
\left[ \begin{array} {c|c} a & b \\ c & d \\ \end{array} \right]
$$
```

$$
\begin{pmatrix} a & b \\ c & d \\ \end{pmatrix} \quad
\begin{bmatrix} a & b \\ c & d \\ \end{bmatrix} \quad
\left[ \begin{matrix} a & b \\ c & d \\ \end{matrix} \right] \quad
\begin{Bmatrix} a & b \\ c & d \\ \end{Bmatrix} \quad
\left\{ \begin{matrix} a & b \\ c & d \\ \end{matrix} \right\} \quad
\begin{vmatrix} a & b \\ c & d \\ \end{vmatrix} \quad
\begin{Vmatrix} a & b \\ c & d \\ \end{Vmatrix} \quad
\left[ \begin{array} {c|c} a & b \\ c & d \\ \end{array} \right]
$$

### Norm - 范数符号

范数命令: `\parallel`
例如: `$\parallel x \parallel_2$` 表示 $\parallel x \parallel_2$

### 堆积符号

- `\stackrel{上位符号}{基位符号}` : 基位符号大，上位符号小，例如: `$\vec{x}\stackrel{\mathrm{def} }{=}{x_1,\dots,x_n}$` 表示 $\vec{x}\stackrel{\mathrm{def} }{=}{x_1,\dots,x_n}$
- `{上位公式 \choose 下位公式}` : 上下符号一样大，上下符号被包括在圆弧内，例如: `${n+1 \choose k}={n \choose k}+{n \choose k-1}$` 表示 ${n+1 \choose k}={n \choose k}+{n \choose k-1}$
- `{上位公式 \atop 下位公式}` : 上下符号一样大，例如: `$\sum_{k_0,k_1,\ldots>0 \atop k_0+k_1+\cdots=n}A_k$` 表示 $\sum_{k_0,k_1,\ldots>0 \atop k_0+k_1+\cdots=n}A_k$

### 给公式加一个方框

`\boxed` 命令给公式加一个方框，例如: `$$\boxed{E=mc^2}$$`表示
$$\boxed{E=mc^2}$$

### 给公式加点颜色

`\color{color}{text}` 命令给 `text` 部分渲染 `color` , 例如:

|命令|效果|
|:-:|:-:|
| `$\color{black}{text}$` | $\color{black}{text}$ |
| `$\color{gray}{text}$` | $\color{gray}{text}$ |
| `$\color{silver}{text}$` | $\color{silver}{text}$ |
| `$\color{white}{text}$` | $\color{white}{text}$ |
| `$\color{maroon}{text}$` | $\color{maroon}{text}$ |
| `$\color{red}{text}$` | $\color{red}{text}$ |
| `$\color{yellow}{text}$` | $\color{yellow}{text}$ |
| `$\color{lime}{text}$` | $\color{lime}{text}$ |
| `$\color{olive}{text}$` | $\color{olive}{text}$ |
| `$\color{green}{text}$` | $\color{green}{text}$ |
| `$\color{teal}{text}$` | $\color{teal}{text}$ |
| `$\color{aqua}{text}$` | $\color{aqua}{text}$ |
| `$\color{blue}{text}$` | $\color{blue}{text}$ |
| `$\color{navy}{text}$` | $\color{navy}{text}$ |
| `$\color{purple}{text}$` | $\color{purple}{text}$ |
| `$\color{fuchsia}{text}$` | $\color{fuchsia}{text}$ |

### 添加刪除线

在公式内使用 `\require{cancel}` 来允许 `片段删除线` 的显示。
声明片段删除线后，使用 `\cancel{字符}` , `\bcancel{字符}` , `\xcancel{字符}` 和 `\cancelto{字符}` 来实现各种片段删除线效果，例如:

```latex
$$
\require{cancel}\begin{array}{rl}
\verb|y+\cancel{x}| & y+\cancel{x}\\
\verb|\cancel{y+x}| & \cancel{y+x}\\
\verb|y+\bcancel{x}| & y+\bcancel{x}\\
\verb|y+\xcancel{x}| & y+\xcancel{x}\\
\verb|y+\cancelto{0}{x}| & y+\cancelto{0}{x}\\
\verb+\frac{1\cancel9}{\cancel95} = \frac15+& \frac{1\cancel9}{\cancel95} = \frac15 \\
\end{array}
$$
```

$$
\require{cancel}\begin{array}{rl}
\verb|y+\cancel{x}| & y+\cancel{x}\\
\verb|\cancel{y+x}| & \cancel{y+x}\\
\verb|y+\bcancel{x}| & y+\bcancel{x}\\
\verb|y+\xcancel{x}| & y+\xcancel{x}\\
\verb|y+\cancelto{0}{x}| & y+\cancelto{0}{x}\\
\verb+\frac{1\cancel9}{\cancel95} = \frac15+& \frac{1\cancel9}{\cancel95} = \frac15 \\
\end{array}
$$

使用 `\require{enclose}` 来允许 `整段删除线` 的显示。
声明整段删除线后，使用 `\enclose{删除线效果}{字符}` 来实现各种整段删除线效果。
其中，删除线效果有 `horizontalstrike` , `verticalstrike` , `updiagonalstrike` 和 `downdiagonalstrike`，可叠加使用，例如:

```latex
$$
\require{enclose}\begin{array}{rl}
\verb|\enclose{horizontalstrike}{x+y}| & \enclose{horizontalstrike}{x+y}\\
\verb|\enclose{verticalstrike}{\frac xy}| & \enclose{verticalstrike}{\frac xy}\\
\verb|\enclose{updiagonalstrike}{x+y}| & \enclose{updiagonalstrike}{x+y}\\
\verb|\enclose{downdiagonalstrike}{x+y}| & \enclose{downdiagonalstrike}{x+y}\\
\verb|\enclose{horizontalstrike,updiagonalstrike}{x+y}| & \enclose{horizontalstrike,updiagonalstrike}{x+y}\\
\end{array}
$$
```

$$
\require{enclose}\begin{array}{rl}
\verb|\enclose{horizontalstrike}{x+y}| & \enclose{horizontalstrike}{x+y}\\
\verb|\enclose{verticalstrike}{\frac xy}| & \enclose{verticalstrike}{\frac xy}\\
\verb|\enclose{updiagonalstrike}{x+y}| & \enclose{updiagonalstrike}{x+y}\\
\verb|\enclose{downdiagonalstrike}{x+y}| & \enclose{downdiagonalstrike}{x+y}\\
\verb|\enclose{horizontalstrike,updiagonalstrike}{x+y}| & \enclose{horizontalstrike,updiagonalstrike}{x+y}\\
\end{array}
$$

### 字体转换

若要对公式的某一部分字符进行字体转换，可以用 `{\字体 {需转换的部分字符} }` 命令，其中 `\字体` 部分可以参照下表选择合适的字体，一般情况下，公式默认为意大利体。

| 命令          | 说明       | 效果            |
| :-----------: | :--------: | :-------------: |
| `\rm`         | 罗马体     | $\rm D$         |
| `\cal`        | 花体       | $\cal D$        |
| `\it`         | 意大利体   | $\it D$         |
| `\Bbb`        | 黑板粗体   | $\Bbb D$        |
| `\bf`         | 粗体       | $\bf D$         |
| `\mit`        | 数学斜体   | $\mit D$        |
| `\sf`         | 等线体     | $\sf D$         |
| `\scr`        | 手写体     | $\scr D$        |
| `\tt`         | 打字机体   | $\tt D$         |
| `\frak`       | 旧德式字体 | $\frak D$       |
| `\boldsymbol` | 黑体       | $\boldsymbol D$ |

### 交换图表使用

使用一行 `$\require{AMScd}$` 语句来允许交换图表的显示。
声明交换图表后，语法与矩阵相似，在开头使用 `begin{CD}` , 在结尾使用 `end{CD}` , 在中间插入图表元素，每个元素之间插入 `&` , 并在每行结尾处使用 `\\` , 例如:

```latex
$\require{AMScd}$
$$
\begin{CD}
    A @>a>> B\\
    @V b V V\# @VV c V\\
    C @>>d> D
\end{CD}
$$
```

$\require{AMScd}$
$$
\begin{CD}
    A @>a>> B\\
    @V b V V\# @VV c V\\
    C @>>d> D
\end{CD}
$$

其中，`@>>>` 代表右箭头, `@<<<` 代表左箭头, `@VVV` 代表下箭头, `@AAA` 代表上箭头, `@=` 代表水平双实线, `@|` 代表竖直双实线, `@.` 代表没有箭头。
在 `@>>>` 的 `>>>` 之间任意插入文字即代表该箭头的注释文字。

```latex
$$
\begin{CD}
    A @>>> B @>{\text{very long label}}>> C \\
    @. @AAA @| \\
    D @= E @<<< F
\end{CD}
$$
```

$$
\begin{CD}
    A @>>> B @>{\text{very long label}}>> C \\
    @. @AAA @| \\
    D @= E @<<< F
\end{CD}
$$

## LaTeX 常用数学符号整理

### 表1 数学模式重音符号

| 符号        | 命令          | 符号        | 命令           | 符号            | 命令              |
| :---------: | :-----------: | :---------: | :------------: | :-------------: | :---------------: |
| $\hat{a}$   | `$\hat{a}$`   | $\check{a}$ | `$\check{a}$`  | $\tilde{a}$     | `$\tilde{a}$`     |
| $\grave{a}$ | `$\grave{a}$` | $\dot{a}$   | `$\dot{a}$`    | $\ddot{a}$      | `$\ddot{a}$`      |
| $\bar{a}$   | `$\bar{a}$`   | $\vec{a}$   | `$\vec{a} $`   | $\widehat{A}$   | `$\widehat{A}$`   |
| $\acute{a}$ | `$\acute{a}$` | $\breve{a}$ | `$\breve{a} $` | $\widetilde{A}$ | `$\widetilde{A}$` |

### 表2 希腊字母

| 符号          | 命令            | 符号        | 命令          | 符号        | 命令          | 符号       | 命令         |
| :-----------: | :-------------: | :---------: | :-----------: | :---------: | :-----------: | :--------: | :----------: |
| $\alpha$      | `$\alpha$`      | $\theta$    | `$\theta$`    | $o$         | `$o$`         | $\upsilon$ | `$\upsilon$` |
| $\beta$       | `$\beta$`       | $\vartheta$ | `$\vartheta$` | $\pi$       | `$\pi$`       | $\phi$     | `$\phi$`     |
| $\gamma$      | `$\gamma$`      | $\iota$     | `$\iota$`     | $\varpi$    | `$\varpi$`    | $\varphi$  | `$\varphi$`  |
| $\delta$      | `$\delta$`      | $\kappa$    | `$\kappa$`    | $\rho$      | `$\rho$`      | $\chi$     | `$\chi$`     |
| $\epsilon$    | `$\epsilon$`    | $\lambda$   | `$\lambda$`   | $\varrho$   | `$\varrho$`   | $\psi$     | `$\psi$`     |
| $\varepsilon$ | `$\varepsilon$` | $\mu$       | `$\mu$`       | $\sigma$    | `$\sigma$`    | $\omega$   | `$\omega$`   |
| $\zeta$       | `$\zeta$`       | $\nu$       | `$\nu$`       | $\varsigma$ | `$\varsigma$` |            |              |
| $\eta$        | `$\eta$`        | $\xi$       | `$\xi$`       | $\tau$      | `$\tau$`      |            |              |

> **Tips:**
>
> 1. 如果使用大写的希腊字母，把命令的首字母变成大写即可，例如: `$\Gamma$` 表示 $\Gamma$ .
>
> 2. 如果使用斜体大写希腊字母，再在大写希腊字母的 LaTeX 命令前加上 var , 例如:  `$\varGamma$` 表示 $\varGamma$ .

### 表3 二元关系符

| 符号          | 命令                | 符号          | 命令                 | 符号      | 命令                |
| :-----------: | :-----------------: | :-----------: | :------------------: | :-------: | :-----------------: |
| $<$           | `$<$`               | $>$           | `$>$`                | $=$       | `$=$`               |
| $\le$         | `$\leq$` 或 `$\le$` | $\ge$         | `$\geq$` 或 `$\ge$`  | $\equiv$  | `$\equiv$`          |
| $\ll$         | `$\ll$`             | $\gg$         | `$\gg$`              | $\doteq$  | `$\doteq$`          |
| $\prec$       | `$\prec$`           | $\succ$       | `$\succ$`            | $\sim$    | `$\sim$`            |
| $\preceq$     | `$\preceq$`         | $\succeq$     | `$\succeq$`          | $\simeq$  | `$\simeq$`          |
| $\subset$     | `$\subset$`         | $\supset$     | `$\supset$`          | $\approx$ | `$\approx$`         |
| $\subseteq$   | `$\subseteq$`       | $\supseteq$   | `$\supseteq$`        | $\cong$   | `$\cong$`           |
| $\sqsubset$   | `$\sqsubset$`       | $\sqsupset$   | `$\sqsupset$`        | $\Join$   | `$\Join$`           |
| $\sqsubseteq$ | `$\sqsubseteq$`     | $\sqsupseteq$ | `$\sqsupseteq$`      | $\bowtie$ | `$\bowtie$`         |
| $\in$         | `$\in$`             | $\ni$         | `$\ni$` 或 `$\owns$` | $\propto$ | `$\propto$`         |
| $\vdash$      | `$\vdash$`          | $\dashv$      | `$\dashv$`           | $\models$ | `$\models$`         |
| $\mid$        | `$\mid$`            | $\parallel$   | `$\parallel$`        | $\perp$   | `$\perp$`           |
| $\smile$      | `$\smile$`          | $\frown$      | `$\frown$`           | $\asymp$  | `$\asymp$`          |
| $:$           | `$:$`               | $\notin$      | `$\notin$`           | $\ne$     | `$\neq$` 或 `$\ne$` |

> **Tips:** 可以在上述符号的相应命令前加上 `\not` , 得到其否定形式，例如: `$\not\subset$` 表示 $\not\subset$ .

### 表4 二元运算符

| 符号| 命令 | 符号 | 命令 | 符号 | 命令|
|:-:|:-:|:-:|:-:|:-:|:-:|
| $+$| `$+$` | $-$ | `$-$` | | |
| $\pm$ | `$\pm$` | $\mp$ | `$\mp$` | $\triangleleft$ | `$\triangleleft$` |
| $\cdot$ | `$\cdot$` | $\div$ | `$\div$` | $\triangleright$ | `$\triangleright$` |
| $\times$ | `$\times$` | $\setminus$ | `$\setminus$` | $\star$ | `$\star$` |
| $\cup$ | `$\cup$` | $\cap$ | `$\cap$` | $\ast$ | `$\ast$` |
| $\sqcup$ | `$\sqcup$` | $\sqcap$ | `$\sqcap$` | $\circ$ | `$\circ$` |
| $\vee$ | `$\vee$` | $\wedge$ | `$\wedge$` 或 `$\land$` | $\bullet$ | `$\bullet$` |
| $\oplus$ | `$\oplus$` | $\ominus$ | `$\ominus$` | $\diamond$ | `$\diamond$` |
| $\odot$ | `$\odot$` | $\oslash$ | `$\oslash$` | $\uplus$ | `$\uplus$` |
| $\otimes$ | `$\otimes$` | $\bigcirc$ | `$\bigcirc$` | $\amalg$ | `$\amalg$` |
| $\bigtriangleup$ | `$\bigtriangleup$` | $\bigtriangledown$ | `$\bigtriangledown$` | $\dagger$ | `$\dagger$` |
| $\lhd$ | `$\lhd$` | $\rhd$ | `$\rhd$` | $\ddagger$ | `$\ddagger$` |
| $\unlhd$ | `$\unlhd$` | $\unrhd$ | `$\unrhd$` | $\wr$ | `$\wr$` |

### 表5 大尺寸运算符

| 符号      | 命令        | 符号        | 命令          | 符号        | 命令          | 符号         | 命令           |
| :-------: | :---------: | :---------: | :-----------: | :---------: | :-----------: | :----------: | :------------: |
| $\sum$    | `$\sum$`    | $\bigcup$   | `$\bigcup$`   | $\bigvee$   | `$\bigvee$`   | $\bigoplus$  | `$\bigoplus$`  |
| $\prod$   | `$\prod$`   | $\bigcap$   | `$\bigcap$`   | $\bigwedge$ | `$\bigwedge$` | $\bigotimes$ | `$\bigotimes$` |
| $\coprod$ | `$\coprod$` | $\bigsqcup$ | `$\bigsqcup$` |             |               | $\bigodot$   | `$\bigodot$`   |
| $\int$    | `$\int$`    | $\oint$     | `$\oint$`     |             |               | $\biguplus$  | `$\biguplus$`  |

### 表6 箭头

| 符号| 命令 | 符号 | 命令 | 符号 | 命令|
|:-:|:-:|:-:|:-:|:-:|:-:|
| $\leftarrow$ | `$\leftarrow$` 或 `$\gets$` | $\longleftarrow$ | `$\longleftarrow$` | $\uparrow$ | `$\uparrow$` |
| $\rightarrow$ | `$\rightarrow$` 或 `$\to$` | $\longrightarrow$ | `$\longrightarrow$` | $\downarrow$ | `$\downarrow$` |
| $\leftrightarrow$ | `$\leftrightarrow$` | $\longleftrightarrow$ | `$\longleftrightarrow$` | $\updownarrow$ | `$\updownarrow$` |
| $\Leftarrow$ | `$\Leftarrow$` | $\Longleftarrow$ | `$\Longleftarrow$` | $\Uparrow$ | `$\Uparrow$` |
| $\Rightarrow$ | `$\Rightarrow$` | $\Longrightarrow$ | `$\Longrightarrow$` |  $\Downarrow$ | `$\Downarrow$` |
| $\Leftrightarrow$ | `$\Leftrightarrow$` | $\Longleftrightarrow$ | `$\Longleftrightarrow$`  | $\Updownarrow$ | `$\Updownarrow$` |
| $\mapsto$ | `$\mapsto$` | $\longmapsto$ | `$\longmapsto$` | $\nearrow$ | `$\nearrow$` |
| $\hookleftarrow$ | `$\hookleftarrow$` | $\hookrightarrow$ | `$\hookrightarrow$` | $\searrow$ | `$\searrow$` |
| $\leftharpoonup$ | `$\leftharpoonup$` | $\rightharpoonup$ | `$\rightharpoonup$` | $\swarrow$ | `$\swarrow$` |
| $\leftharpoondown$ | `$\leftharpoondown$` | $\rightharpoondown$ | `$\rightharpoondown$` | $\nwarrow$ | `$\nwarrow$` |
| $\rightleftharpoons$ | `$\rightleftharpoons$` | $\iff$ | `$\iff$` | $\leadsto$ | `$\leadsto$` |

### 表7 定界符

| 符号      | 命令                  | 符号         | 命令                    | 符号           | 命令             | 符号           | 命令             |
| :-------: | :-------------------: | :----------: | :---------------------: | :------------: | :--------------: | :------------: | :--------------: |
| $($       | `$($`                 | $)$          | `$)$`                   | $\uparrow$     | `$\uparrow$`     | $\Uparrow$     | `$\Uparrow$`     |
| $[$       | `$[$` 或 `$\lbrack$`  | $]$          | `$]$` 或 `$\rbrack$`    | $\downarrow$   | `$\downarrow$`   | $\Downarrow$   | `$\Downarrow$`   |
| $\{$      | `$\{$` 或 `$\lbrace$` | $\}$         | `$\}$` 或 `$ rlbrace $` | $\updownarrow$ | `$\updownarrow$` | $\Updownarrow$ | `$\Updownarrow$` |
| $\langle$ | `$\langle$`           | $\rangle$    | `$\rangle$`             | $\vert$        | `$\vert$`        | $\Vert$        | `$\Vert$`        |
| $\lfloor$ | `$\lfloor$`           | $\rfloor$    | `$\rfloor$`             | $\lceil$       | `$\lceil$`       | $\rceil$       | `$\rceil$`       |
| $/$       | `$/$`                 | $\backslash$ | `$\backslash$`          |                |                  |                |                  |

### 表8 大尺寸定界符

| 符号          | 命令            | 符号          | 命令            |
| :-----------: | :-------------: | :-----------: | :-------------: |
| $\lgroup$     | `$\lgroup$`     | $\rgroup$     | `$\rgroup$`     |
| $\lmoustache$ | `$\lmoustache$` | $\rmoustache$ | `$\rmoustache$` |
| $\arrowvert$  | `$\arrowvert$`  | $\Arrowvert$  | `$\Arrowvert$`  |
| $\bracevert$  | `$\bracevert$`  |               |                 |

### 表9 其它符号

| 符号| 命令 | 符号 | 命令 | 符号 | 命令| 符号| 命令 |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\dots$ | `$\dots$` | $\cdots$ | `$\cdots$` | $\vdots$ | `$\vdots$` | $\ddots$ | `$\ddots$` |
| $\hbar$ | `$\hbar$` | $\imath$ | `$\imath$` | $\jmath$ | `$\jmath$` | $\ell$ | `$\ell$` |
| $\Re$ | `$\Re$` | $\Im$ | `$\Im$` | $\aleph$ | `$\aleph$` | $\wp$ | `$\wp$` |
| $\forall$ | `$\forall$` | $\exists$ | `$\exists$` | $\mho$ | `$\mho$` | $\partial$ | `$\partial$` |
| $'$ | `$'$` | $\prime$ | `$\prime$` | $\emptyset$ | `$\emptyset$` | $\infty$ | `$\infty$` |
| $\nabla$ | `$\nabla$` | $\triangle$ | `$\triangle$` | $\Box$ | `$\Box$` | $\Diamond$ | `$\Diamond$` |
| $\bot$ | `$\bot$` | $\top$ | `$\top$` | $\angle$ | `$\angle$` | $\surd$ | `$\surd$` |
| $\diamondsuit$ | `$\diamondsuit$` | $\heartsuit$ | `$\heartsuit$` | $\clubsuit$ | `$\clubsuit$` | $\spadesuit$ | `$\spadesuit$` |
| $\lnot$ | `$\neg$` 或 `$\lnot$` | $\flat$ | `$\flat$` | $\natural$ | `$\natural$` | $\sharp$ | `$\sharp$` |

### 表10 AMS 定界符

| 符号| 命令 | 符号 | 命令 | 符号 | 命令 | 符号 | 命令 |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\ulcorner$ | `$\ulcorner$` | $\urcorner$ | `$\urcorner$` | $\llcorner$ | `$\llcorner$` | $\lrcorner$ | `$\lrcorner$` |
| $\lvert$ | `$\lvert$` | $\rvert$ | `$\rvert$` | $\lVert$ | `$\lVert$` | $\rVert$ | `$\rVert$` |

### 表11 AMS 希腊和希伯来字母

| 符号| 命令 | 符号 | 命令 | 符号 | 命令 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| $\digamma$ | `$\digamma$` | $\varkappa$ | `$\varkappa$` | $\beth$ | `$\beth$` |
| $\daleth$ | `$\daleth$` | $\gimel$ | `$\gimel$` |

### 表12 AMS 二元关系符

| 符号| 命令 | 符号 | 命令 | 符号 | 命令 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| $\lessdot$ | `$\lessdot$` | $\gtrdot$ | `$\gtrdot$` | $\doteqdot$ | `$\doteqdot$` |
| $\leqslant$ | `$\leqslant$` | $\geqslant$ | `$\geqslant$` | $\risingdotseq$ | `$\risingdotseq$` |
| $\eqslantless$ | `$\eqslantless$` | $\eqslantgtr$ | `$\eqslantgtr$` | $\fallingdotseq$ | `$\fallingdotseq$` |
| $\leqq$ | `$\leqq$` | $\geqq$ | `$\geqq$` | $\eqcirc$ | `$\eqcirc$` |
| $\lll$ | `$\lll$` 或 `$\llless$` | $\ggg$ | `$\ggg$` | $\circeq$ | `$\circeq$` |
| $\lesssim$ | `$\lesssim$` | $\gtrsim$ | `$\gtrsim$` | $\triangleq$ | `$\triangleq$` |
| $\lessapprox$ | `$\lessapprox$` | $\gtrapprox$ | `$\gtrapprox$` | $\bumpeq$ | `$\bumpeq$` |
| $\lessgtr$ | `$\lessgtr$` | $\gtrless$ | `$\gtrless$` | $\Bumpeq$ | `$\Bumpeq$` |
| $\lesseqgtr$ | `$\lesseqgtr$` | $\gtreqless$ | `$\gtreqless$` | $\thicksim$ | `$\thicksim$` |
| $\lesseqqgtr$ | `$\lesseqqgtr$` | $\gtreqqless$ | `$\gtreqqless$` | $\thickapprox$ | `$\thickapprox$` |
| $\preccurlyeq$ | `$\preccurlyeq$` | $\succcurlyeq$ | `$\succcurlyeq$` | $\approxeq$ | `$\approxeq$` |
| $\curlyeqprec$ | `$\curlyeqprec$` | $\curlyeqsucc$ | `$\curlyeqsucc$` | $\backsim$ | `$\backsim$` |
| $\precsim$ | `$\precsim$` | $\succsim$ | `$\succsim$` | $\backsimeq$ | `$\backsimeq$` |
| $\precapprox$ | `$\precapprox$` | $\succapprox$ | `$\succapprox$` | $\vDash$ | `$\vDash$` |
| $\subseteqq$ | `$\subseteqq$` | $\supseteqq$ | `$\supseteqq$` | $\Vdash$ | `$\Vdash$` |
| $\Subset$ | `$\Subset$` | $\Supset$ | `$\Supset$` | $\Vvdash$ | `$\Vvdash$` |
| $\sqsubset$ | `$\sqsubset$` | $\sqsupset$ | `$\sqsupset$` | $\backepsilon$ | `$\backepsilon$` |
| $\therefore$ | `$\therefore$` | $\because$ | `$\because$` | $\varpropto$ | `$\varpropto$` |
| $\shortmid$ | `$\shortmid$` | $\shortparallel$ | `$\shortparallel$` | $\between$ | `$\between$` |
| $\smallsmile$ | `$\smallsmile$` | $\smallfrown$ | `$\smallfrown$` | $\pitchfork$ | `$\pitchfork$` |
| $\vartriangleleft$ | `$\vartriangleleft$` | $\vartriangleright$ | `$\vartriangleright$` | $\blacktriangleleft$ | `$\blacktriangleleft$` |
| $\trianglelefteq$ | `$\trianglelefteq$` | $\trianglerighteq$ | `$\trianglerighteq$` | $\blacktriangleright$ | `$\blacktriangleright$` |

### 表13 AMS 箭头

| 符号| 命令 | 符号 | 命令 | 符号 | 命令 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| $\dashleftarrow$ | `$\dashleftarrow$` | $\dashrightarrow$ | `$\dashrightarrow$` | $\multimap$ | `$\multimap$` |
| $\leftleftarrows$ | `$\leftleftarrows$` | $\rightrightarrows$ | `$\rightrightarrows$` | $\upuparrows$ | `$\upuparrows$` |
| $\leftrightarrows$ | `$\leftrightarrows$` | $\rightleftarrows$ | `$\rightleftarrows$` | $\downdownarrows$ | `$\downdownarrows$` |
| $\Lleftarrow$ | `$\Lleftarrow$` | $\Rrightarrow$ | `$\Rrightarrow$` | $\upharpoonleft$ | `$\upharpoonleft$` |
| $\twoheadleftarrow$ | `$\twoheadleftarrow$` | $\twoheadrightarrow$ | `$\twoheadrightarrow$` | $\upharpoonright$ | `$\upharpoonright$` |
| $\leftarrowtail$ | `$\leftarrowtail$` | $\rightarrowtail$ | `$\rightarrowtail$` | $\downharpoonleft$ | `$\downharpoonleft$` |
| $\leftrightharpoons$ | `$\leftrightharpoons $` | $\rightleftharpoons$ | `$\rightleftharpoons$` | $\downharpoonright$ | `$\downharpoonright$` |
| $\Lsh$ | `$\Lsh$` | $\Rsh$ | `$\Rsh$` | $\rightsquigarrow$ | `$\rightsquigarrow$` |
| $\looparrowleft$ | `$\looparrowleft$` | $\looparrowright$ | `$\looparrowright$` | $\leftrightsquigarrow$ | `$\leftrightsquigarrow$` |
| $\curvearrowleft$ | `$\curvearrowleft$` | $\curvearrowright$ | `$\curvearrowright$` | | |
| $\circlearrowleft$ | `$\circlearrowleft$` | $\circlearrowright$ | `$\circlearrowright$` | | |

### 表14 AMS 二元否定关系符和箭头

| 符号| 命令 | 符号 | 命令 | 符号 | 命令 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| $\nless$ | `$\nless$` | $\ngtr$ | `$\ngtr$` | $\varsubsetneqq$ | `$\varsubsetneqq$` |
| $\lneq$ | `$\lneq$` | $\gneq$ | `$\gneq$` | $\varsupsetneqq$ | `$\varsupsetneqq$` |
| $\nleq$ | `$\nleq$` | $\ngeq$ | `$\ngeq$` | $\nsubseteqq$ | `$\nsubseteqq$` |
| $\nleqslant$ | `$\nleqslant$` | $\ngeqslant$ | `$\ngeqslant$` | $\nsupseteqq$ | `$\nsupseteqq$` |
| $\lneqq$ | `$\lneqq$` | $\gneqq$ | `$\gneqq$` | $\nmid$ | `$\nmid$` |
| $\lvertneqq$ | `$\lvertneqq$` | $\gvertneqq$ | `$\gvertneqq$` | $\nparallel$ | `$\nparallel$` |
| $\nleqq$ | `$\nleqq$` | $\ngeqq$ | `$\ngeqq$` | $\nshortmid$ | `$\nshortmid$` |
| $\lnsim$ | `$\lnsim$` | $\gnsim$ | `$\gnsim$` | $\nshortparallel$ | `$\nshortparallel$` |
| $\lnapprox$ | `$\lnapprox$` | $\gnapprox$ | `$\gnapprox$` | $\nsim$ | `$\nsimd$` |
| $\nprec$ | `$\nprec$` | $\nsucc$ | `$\nsucc$` | $\ncong$ | `$\ncong$` |
| $\npreceq$ | `$\npreceq$` | $\nsucceq$ | `$\nsucceq$` | $\nvdash$ | `$\nvdash$` |
| $\precneqq$ | `$\precneqq$` | $\succneqq$ | `$\succneqq$` | $\nvDash$ | `$\nvDash$` |
| $\precnsim$ | `$\precnsim$` | $\succnsim$ | `$\succnsim$` | $\nVdash$ | `$\nVdash$` |
| $\precnapprox$ | `$\precnapprox$` | $\succnapprox$ | `$\succnapprox$` | $\nVDash$ | `$\nVDash$` |
| $\subsetneq$ | `$\subsetneq$` | $\supsetneq$ | `$\supsetneq$` | $\ntriangleleft$ | `$\ntriangleleft$` |
| $\varsubsetneq$ | `$\varsubsetneq$` | $\varsupsetneq$ | `$\varsupsetneq$` | $\ntriangleright$ | `$\ntrianglerightt$` |
| $\nsubseteq$ | `$\nsubseteq$` | $\nsupseteq$ | `$\nsupseteq$` | $\ntrianglelefteq$ | `$\ntrianglelefteq$` |
| $\subsetneqq$ | `$\subsetneqq$` | $\supsetneqq$ | `$\supsetneqq$` | $\ntrianglerighteq$ | `$\ntrianglerighteq$` |
| $\nleftarrow$ | `$\nleftarrow$` | $\nrightarrow$ | `$ nrightarrow $` | $\nleftrightarrow$ | `$\nleftrightarrow$` |
| $\nLeftarrow$ | `$\nLeftarrow$` | $\nRightarrow$ | `$ nRightarrow $` | $\nLeftrightarrow$ | `$\nLeftrightarrow$` |

### 表15 AMS 二元运算符

| 符号| 命令 | 符号 | 命令 | 符号 | 命令 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| $\dotplus$ | `$\dotplus$` | $\centerdot$ | `$\centerdot$` | $\intercal$ | `$\intercal$` |
| $\ltimes$ | `$\ltimes$` | $\rtimes$ | `$\rtimes$` | $\divideontimes$ | `$\divideontimes$` |
| $\Cup$ | `$\Cup$` 或 `$\doublecup$` | $\doublecap$ | `$\Cap$` 或 `$\doublecap$` | $\smallsetminus$ | `$\smallsetminus$` |
| $\veebar$ | `$\veebar$` | $\barwedge$ | `$\barwedge$` | $\doublebarwedge$ | `$\doublebarwedge$` |
| $\boxplus$ | `$\boxplus$` | $\boxminus$ | `$\boxminus$` | $\circleddash$ | `$\circleddash$` |
| $\boxtimes$ | `$\boxtimes$` | $\boxdot$ | `$\boxdot$` | $\circledcirc$ | `$\circledcirc$` |
| $\leftthreetimes$ | `$\leftthreetimes$` | $\rightthreetimes$ | `$\rightthreetimes$` | $\circledast$ | `$\circledast$` |
| $\curlyvee$ | `$\curlyvee$` | $\curlywedge$ | `$\curlywedge$` | | |

### 表16 AMS 其它符号

| 符号| 命令 | 符号 | 命令 | 符号 | 命令 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| $\hbar$ | `$\hbar$` | $\hslash$ | `$\hslash$` | $\Bbbk$ | `$\Bbbk$` |
| $\square$ | `$\square$` | $\blacksquare$ | `$\blacksquare$` | $\circledS$ | `$\circledS$` |
| $\vartriangle$ | `$\vartriangle$` | $\blacktriangle$ | `$\blacktriangle$` | $\complement$ | `$\complement$` |
| $\triangledown$ | `$\triangledown$` | $\blacktriangledown$ | `$\blacktriangledown$` | $\Game$ | `$\Game$` |
| $\lozenge$ | `$\lozenge$` | $\blacklozenge$ | `$\blacklozenge$` | $\bigstar$ | `$\bigstar$` |
| $\angle$ | `$\angle$` | $\measuredangle$ | `$\measuredangle$` | $\sphericalangle$ | `$\sphericalangle$` |
| $\diagup$ | `$\diagup$` | $\diagdown$ | `$\diagdown$` | $\backprime$ | `$\backprime$` |
| $\nexists$ | `$\nexists$` | $\Finv$ | `$\Finv$` | $\varnothing$ | `$\varnothing$` |
| $\eth$ | `$\eth$` | $\mho$ | `$\mho$` | | |

### 表17 数学字母

| 符号| 命令 |
|:-:|:-:|
| $\mathrm{ABCdef}$ | `$\mathrm{ABCdef}$` |
| $\mathit{ABCdef}$ | `$\mathit{ABCdef}$` |
| $\mathcal{ABCdef}$ | `$\mathcal{ABCdef}$` |
| $\mathscr{ABCdef}$ | `$\mathscr{ABCdef}$` |
| $\mathfrak{ABCdef}$ | `$\mathfrak{ABCdef}$` |
| $\mathbb{ABCdef}$ | `$\mathbbk{ABCdef}$` |

## 参考文献

1. [Markdown中LaTeX 数学公式基本语法](https://blog.csdn.net/u014630987/article/details/70156489)
2. [Markdown公式编辑学习笔记](https://www.cnblogs.com/q735613050/p/7253073.html)
3. [一份不太简短的 LATEX 介绍](https://wenku.baidu.com/view/754ab741a8956bec0975e3b2.html?pn=51)
4. [Cmd Markdown 公式指导手册](https://www.zybuluo.com/codeep/note/163962#5%E6%B7%BB%E5%8A%A0%E5%88%A0%E9%99%A4%E7%BA%BF)
