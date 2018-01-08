# 算术运算

在Linux下做算术运算时你是如何进行的呢？是不是还在用expr呢？你会说我还会bc还有其它的呢！  
闲话不多扯，干正事！

## expr

expr在使用中要注意一些书写，如表达式中量和运算符号之间的空格及一些运算符号需要转义，还有一点需要记住，expr只适用于整数之间的运算！

### 1.1 表达式

expr的help文档中关于表达式部分如下：

```
  ARG1 | ARG2       若ARG1 的值不为0 或者为空，则返回ARG1，否则返回ARG2

  ARG1 & ARG2       若两边的值都不为0 或为空，则返回ARG1，否则返回 0

  ARG1 < ARG2       ARG1 小于ARG2
  ARG1 <= ARG2      ARG1 小于或等于ARG2
  ARG1 = ARG2       ARG1 等于ARG2
  ARG1 != ARG2      ARG1 不等于ARG2
  ARG1 >= ARG2      ARG1 大于或等于ARG2
  ARG1 > ARG2       ARG1 大于ARG2

  ARG1 + ARG2       计算 ARG1 与ARG2 相加之和
  ARG1 - ARG2       计算 ARG1 与ARG2 相减之差

  ARG1 * ARG2       计算 ARG1 与ARG2 相乘之积
  ARG1 / ARG2       计算 ARG1 与ARG2 相除之商
  ARG1 % ARG2       计算 ARG1 与ARG2 相除之余数
```

这一部分相信大家用的最多，也对这些比较了解了，下面我们用一个表达式来说明：

```
$expr 9 + 8 - 7 \* 6 / 5 + \( 4 - 3 \) \* 2
11
```

通过结果相信你已知道expr的计算规律，它与我们日常所理解的数学表达式一样，括号的优先级最高，然后是“\*”、“/”，而且每个数或符号都需要用空格分隔，结果也是整数。

### 1.2 字符串

expr还可以对字符串进行操作：  
  match  
  字符串 表达式等于"字符串 :表达式"  
  substr 字符串 偏移量 长度替换字符串的子串，偏移的数值从 1 起计  
  index 字符串 字符在字符串中发现字符的地方建立下标，或者标0  
  length 字符串字符串的长度

#### 1）match

expr中的expr match $string substring命令在string字符串中匹配substring字符串（substring字符串可以是正则表达式），然后返回匹配到的substring字符串的长度，若找不到则返回0。  
下面我们来个实例：

```
$str="123 456 789"
$expr match "$str" .*5
6
```

.\*5匹配了6个字符。

#### 2）substr

在shell中可以用{string:position}和{string:position:length}进行对string字符串中字符的抽取。第一种是从position位置开始抽取直到字符串结束，第二种是从position位置开始抽取长度为length的子串。而用expr中的expr substr stringstringposition $length同样能实现上述功能。  
实例：

```shell
str="123 456 789"
echo ${str:5}
56 789
echo ${str:5:3}
56
expr substr "$str" 5 3
456
```

从中可以看出{string:position}和{string:position:length}从0开始计数，而expr substr stringstringposition $length从1开始。

#### 3）index

expr中的expr index stringsubstring索引命令功能在字符串stringsubstring索引命令功能在字符串string上找出substring中字符第一次出现的位置，若找不到则expr index返回0。注意它匹配的是字符而非字符串。  
实例：

```
$str="123 456 789"
$expr index "$str" b
0
$expr index "$str" 9
11
```

#### 4）length

计算字符串的长度。我们可以用awk中的length\(s\)进行计算。我们也可以用echo中的echo {\#string}进行计算，当然也可以expr中的expr length{\#string}进行计算，当然也可以expr中的expr lengthstring 求出字符串的长度。

```
$str="123 456 789"
$echo ${#str}
11
$expr length "$str"
11
```



