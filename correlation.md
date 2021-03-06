---
layout:     post
title:      "相关性分析及其可视化"
subtitle:   "Correlation with R"
date:       2017-05-11
author:     "Matrix"
header-img: "img/post-bg-rmarkdown-2.png"
tags:
    - R语言
    - 统计学
    - 数据处理
    - 可视化
---

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-9-1.png)

<h2 id="1">1. 相关性</h2>

在概率论和统计学中，**相关**（Correlation，或称**相关系数**或**关联系数**），显示两个随机变量之间线性关系的强度和方向。在统计学中，相关的意义是用来衡量两个变量相对于其相互独立的距离。在这个广义的定义下，有许多根据数据特点而定义的用来衡量数据相关的系数。

**1.1 各种相关系数：**

对于不同测量尺度的变数，有不同的相关系数可用：

-   Pearson相关系数（Pearson's r）：衡量两个**等距尺度**或**等比尺度**变数之相关性。是最常见的，也是学习统计学时第一个接触的相关系数。
-   净相关（英语：partial correlation）：在模型中有多个自变数（或解释变数）时，去除掉其他自变数的影响，只衡量特定一个自变数与因变数之间的相关性。自变数和因变数皆为连续变数。
-   相关比（英语：correlation ratio）：衡量两个**连续变数**之相关性。
-   Gamma相关系数：衡量两个**次序尺度**变数之相关性。
-   Spearman等级相关系数：衡量两个次序尺度变数之相关性。
-   Kendall等级相关系数（英语：Kendall tau rank correlation coefficient）：衡量两个人为次序尺度变数（原始资料为等距尺度）之相关性。
-   Kendall和谐系数：衡量两个**次序尺度**变数之相关性。
-   Phi相关系数（英语：Phi coefficient）：衡量两个真正**名目尺度**的二分变数之相关性。
-   列联相关系数（英语：contingency coefficient）：衡量两个真正**名目尺度**变数之相关性。
-   四分相关（英语：tetrachoric correlation）：衡量两个人为**名目尺度**（原始资料为**等距尺度**）的二分变数之相关性。
-   Kappa一致性系数（英语：K coefficient of agreement）：衡量两个**名目尺度**变数之相关性。
-   点二系列相关系数（英语：point-biserial correlation）：X变数是真正**名目尺度**二分变数。Y变数是连续变数。
-   二系列相关系数（英语：biserial correlation）：X变数是人为名目尺度二分变数。Y变数是连续变数。

其中，**名目尺度**指的是我们一般所说的衡量**名义变量**和**类别变量**的尺度，**次序尺度**指的是衡量**顺序变量**、**序列变量**以及**等级变量**的尺度，**等距尺度**指的是衡量**间隔变量**和**区间变量**的尺度，**等比变量**指的是衡量**比率和比例变量**的尺度。

下面简要介绍**皮尔逊相关系数**和**Spearman相关系数**：

**1.2 皮尔逊相关系数**

皮尔逊相关系数描述两个变量的线性相关性，常简称为线性相关系数，取值为[-1,1]。其公式定义为：

$$
\rho \left( X,Y\right) =\dfrac {E\left( XY\right) -E\left( X\right) E\left( Y\right) }{\sqrt {E\left( X^{2}\right) -\left[ E\left( X\right) \right] ^{2}}\sqrt {E\left( Y^{2}\right) -\left[ E\left( Y\right) \right] ^{2}}}
$$

若$\rho = \pm 1$，说明数据具有完全相关性，即所有的数据都在同一条直线上。我们经常使用$t$检验来判断数据是否相关，样本$(x_{1},y_{1}),(x_{2},y_{2}),...,(x_{n},y_{n})$来自正态分布，相关性未知，令原假设$H_{0}:r_{xy}=0$，备择假设$H_{1}:r_{xy}\neq 0$。在原假设成立的条件下，$t$统计量

$$
t=r\sqrt {\dfrac {n-2} {1-r^{2}}} \sim t(n-2)
$$

如果样本不服从正态分布，在样本量充分大时，$t$统计量近似服从$t$分布。$r$越大$t$统计量越大，越应该拒绝原假设。

1.3 Spearman相关系数

Spearman相关系数是一种常用的秩相关系数，这里的秩表示数据的大小顺序。具体定义如下：假设有随机样本$x_{1},x_{2},...,x_{n}$，若对任意$i,j$满足$x_{i}\neq x_{j}$，则第$i$小的样本的秩为$i$，若存在$i,j$满足$x_{i}= x_{j}$，则称这个样本是打结的（tied)，先按照上面的定义计算秩，对于打结样本求平均即可。比如，不打结样本$5,4,8,9$，他们的秩依次是$2,1,3,4$。打结样本$5,5,1,1,1,7$，先按照原定义计算秩：$4,5,1,2,3,6$，再对打结样本求平均数，最终$5$的秩为$4,5$，$1$的秩为$2$，$7$的秩为$6$。

假设数据$x_{i}$的秩为$a_{i}$，数据$y_{i}$的秩为$b_{i}$，则$x$的平均秩为$\overline {a}=\sum ^{n}_{i=1}a_{i}=n$，$x$的秩方差为$s_{x}=\sum^{n}_{i=1}(a_{i}-\overline{a })^{2}/n$，类似的可以对$y$定义平均秩和秩方差。Spearman相关系数为:

$$
\rho_{xy}=\dfrac {\sum^{n}_{i=1}(a_{i}-\overline {a})(b_{i}-\overline {b})}{\sqrt{s_{x}s_{y}}}=1-\dfrac {6\sum^{n}_{i=1}(a_{i}-b_{i})^{2}}{n^{3}-n}
$$

对于没有结或结不多的样本，Pearson相关系数的$t$检验仍然适用。

<h2 id="2">2. 使用R语言进行相关分析</h2>

在R自带的程序包stats中，使用cor()可以计算相关系数。参数method控制计算相关系数的方法。

``` r
data("trees")
cor(trees, method = "pearson")
```

    ##            Girth    Height    Volume
    ## Girth  1.0000000 0.5192801 0.9671194
    ## Height 0.5192801 1.0000000 0.5982497
    ## Volume 0.9671194 0.5982497 1.0000000

``` r
cor(trees, method = "spearman")
```

    ##            Girth    Height    Volume
    ## Girth  1.0000000 0.4408387 0.9547151
    ## Height 0.4408387 1.0000000 0.5787101
    ## Volume 0.9547151 0.5787101 1.0000000

相关性检验使用程序包stats中的cor.test()，用法和cor()类似，参数alternative = "two.sided","less","greater"分别表示双侧检验、右侧检验、左侧检验。参数conf.level控制置信水平。

``` r
cor.test(trees[,1], trees[,2], method = "pearson")
```

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  trees[, 1] and trees[, 2]
    ## t = 3.2722, df = 29, p-value = 0.002758
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  0.2021327 0.7378538
    ## sample estimates:
    ##       cor 
    ## 0.5192801

程序包pspearman中的spearman,test()在不打结时提供了更精确的p值。cor.test(, method = "spearman")根据数据量的不同分别使用了AS89和t-distribution两种近似方法。在spearman,test()中参数approximation = c("exact","AS89","t-distribution")可以选择近似方法，默认是精确值。

``` r
library(pspearman)
x <- 1:10
y <- c(5:1, 6, 10:7)
out1 <- spearman.test(x, y)
out2 <- spearman.test(x, y, approximation = "AS89")
out3 <- cor.test(x, y, method = "spearman")
out1$p.value
```

    ## [1] 0.05443067

``` r
out2$p.value
```

    ## [1] 0.05444507

``` r
out3$p.value
```

    ## [1] 0.05444507

<h2 id="3">3. 相关分析的可视化</h2>

虽然**cor()函数**可以非常方便快捷的计算出连续变量之间的相关系数，但当变量非常多时，返回的相关系数一定时读者看的眼花缭乱。 下面就以R自带的mtcars数据集为例，讲讲相关系数图的绘制：

``` r
cor(mtcars[1:7])
```

    ##             mpg        cyl       disp         hp        drat         wt
    ## mpg   1.0000000 -0.8521620 -0.8475514 -0.7761684  0.68117191 -0.8676594
    ## cyl  -0.8521620  1.0000000  0.9020329  0.8324475 -0.69993811  0.7824958
    ## disp -0.8475514  0.9020329  1.0000000  0.7909486 -0.71021393  0.8879799
    ## hp   -0.7761684  0.8324475  0.7909486  1.0000000 -0.44875912  0.6587479
    ## drat  0.6811719 -0.6999381 -0.7102139 -0.4487591  1.00000000 -0.7124406
    ## wt   -0.8676594  0.7824958  0.8879799  0.6587479 -0.71244065  1.0000000
    ## qsec  0.4186840 -0.5912421 -0.4336979 -0.7082234  0.09120476 -0.1747159
    ##             qsec
    ## mpg   0.41868403
    ## cyl  -0.59124207
    ## disp -0.43369788
    ## hp   -0.70822339
    ## drat  0.09120476
    ## wt   -0.17471588
    ## qsec  1.00000000

**很显然，这么多数字堆在一起肯定很难快速的发现变量之间的相关性大小**，如果可以将相关系数可视化，就能弥补一大堆数字的缺陷了。这里介绍**corrplot包中的corrplot()函数**进行相关系数的可视化，首先来看看该函数的一些重要参数：

- corr：需要可视化的相关系数矩阵
- method：指定可视化的方法，可以是圆形、方形、椭圆形、数值、阴影、颜色或饼图形
- type：指定展示的方式，可以是完全的、下三角或上三角
- col：指定图形展示的颜色，默认以均匀的颜色展示
- bg：指定图的背景色
- title：为图形添加标题
- is.corr：是否为相关系数绘图，默认为TRUE，同样也可以实现非相关系数的可视化，只需使该参数设为FALSE即可
- diag：是否展示对角线上的结果，默认为TRUE
- outline：是否绘制圆形、方形或椭圆形的轮廓，默认为FALSE
- mar：具体设置图形的四边间距
- addgrid.col：当选择的方法为颜色或阴影时，默认的网格线颜色为白色，否则为灰色
- addCoef.col：为相关系数添加颜色，默认不添加相关系数，只有方法为number时，该参数才起作用
- addCoefasPercent：为节省绘图空间，是否将相关系数转换为百分比格式，默认为FALSE
- order：指定相关系数排序的方法，可以是原始顺序(original)、特征向量角序(AOE)、第一主成分顺序(FPC)、层次聚类顺序(hclust)和字母顺序，一般”AOE”排序结果都比”FPC”要好
- hclust.method：当order为hclust时，该参数可以是层次聚类中ward法、最大距离法等7种之一
- addrect：当order为hclust时，可以为添加相关系数图添加矩形框，默认不添加框，如果想添加框时，只需为该参数指定一个整数即可
- rect.col：指定矩形框的颜色
- rect.lwd：指定矩形框的线宽
- tl.pos：指定文本标签(变量名称)的位置，当type=full时，默认标签位置在左边和顶部(lt)，当type=lower时，默认标签在左边和对角线(ld)，当type=upper时，默认标签在顶部和对角线，d表示对角线，n表示不添加文本标签
- tl.cex：指定文本标签的大小
- tl.col：指定文本标签的颜色
- cl.pos：图例（颜色）位置，当type=upper或full时，图例在右表(r)，当type=lower时，图例在底部，不需要图例时，只需指定该参数为n
- addshade：只有当method=shade时，该参数才有用，参数值可以是negtive/positive和all，分表表示对负相关系数、正相关系数和所有相关系数添加阴影。注意：正相关系数的阴影是45度，负相关系数的阴影是135度
- shade.lwd：指定阴影的线宽
- shade.col：指定阴影线的颜色


虽然该函数的参数比较多，但可以组合各种参数，灵活实现各种各样的相关系数图。下面就举几个例子：

``` r
library(corrplot)
corr <- cor(mtcars[,1:7])
#参数全部默认情况下的相关系数图
corrplot(corr = corr)
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-5-1.png)

``` r
#指定数值方法的相关系数图
corrplot(corr = corr, method="number", col="black", cl.pos="n")
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-6-1.png)

``` r
#按照特征向量角序(AOE)排序相关系数图
corrplot(corr = corr, order = 'AOE')
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-7-1.png)

``` r
#同时添加相关系数值
corrplot(corr = corr, order ="AOE", addCoef.col="grey")
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-8-1.png)

``` r
#选择方法为color
corrplot(corr = corr, method = 'color', order ="AOE", addCoef.col="grey")
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-9-1.png)

``` r
#绘制圆形轮廓相关系数图
corrplot(corr = corr, col = c("white","black"), order="AOE", outline=TRUE, cl.pos="n")
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-10-1.png)

这个图看起来非常像围棋

``` r
#自定义背景色
corrplot(corr = corr, col = c("white","black"), bg="gold2",  order="AOE", cl.pos="n")
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-11-1.png)

``` r
#混合方法之上三角为圆形，下三角为数字
corrplot(corr = corr,order="AOE",type="upper",tl.pos="d")
corrplot(corr = corr,add=TRUE, type="lower", method="number",
         order="AOE",diag=FALSE,tl.pos="n", cl.pos="n")
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-12-1.png)

这幅图将颜色、圆的大小和数值型相关系数相结合，更容易发现变量之间的相关性/

``` r
#混合方法之上三角为圆形，下三角为方形
corrplot(corr = corr,order="AOE",type="upper",tl.pos="d")
corrplot(corr = corr,add=TRUE, type="lower", method="square",
         order="AOE",diag=FALSE,tl.pos="n", cl.pos="n")
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-13-1.png)

``` r
#混合方法之上三角为圆形，下三角为黑色数字
corrplot(corr = corr,order="AOE",type="upper",tl.pos="tp")
corrplot(corr = corr,add=TRUE, type="lower", method="number",
         order="AOE", col="black",diag=FALSE,tl.pos="n", cl.pos="n")
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-14-1.png)

``` r
#以层次聚类法排序
corrplot(corr = corr, order="hclust")
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-15-1.png)

``` r
#以层次聚类法排序，并绘制3个矩形框
corrplot(corr = corr, order="hclust", addrect = 3, rect.col = "black")
```

![](/img/in-post/correlation_files/figure-markdown_github/unnamed-chunk-15-2.png)

有关更多相关系数图的绘制可参见corrplot()函数的帮助文档，文档中还包括了很多案例，感兴趣的可以去参考的看看。

<h2 id="4">4. 参考文献</h2>

- Wikipedia
- 统计之都
- R语言实战（第二版）
- 概率论与数量统计教程
