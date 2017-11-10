## 1、bash简介：
shell，英文含义是：壳，它是Unix系统下一个命令解析器。  
Unix系统有多种Shell，如：  
- Bourne Shell（/usr/bin/sh或/bin/sh）  
- Bourne Again Shell（/bin/bash）  
- C Shell（/usr/bin/csh）  
- K Shell（/usr/bin/ksh）  
- Shell for Root（/sbin/sh）    

比较通用和强大的是bash，是很多Linux平台内定的shell，所以我们主要学习它。
## 2、bash建立和执行调试
### 2.1 新建文件
```
touch test.sh
```
### 2.2、添加文件权限可执行
``` 
chmod +x test.sh
```
### 2.3、编辑命令文件
``` 
#!/bin/bash
echo "hello bash"
exit 0
```
### 2.4、执行命令
``` 
./test.sh
```
### 2.5、代码注释
- 单行注释 

```
# 这里被注释掉
```
- 多行注释 

```
:||{
	这里将被注释掉
	这里也会被注释掉
}
```
### 2.6、命令调试
- bash -n ，不执行脚本，只检查语法问题。
- bash -v ，先打印脚本内容，再执行脚本。
- bash -x ，查看bash调用流程。
- echo -n ，输出内容之后不换行。


## 3、基础语法规则
### 3.1、变量声明
- shell语言属于弱类型，不需要申明变量类型，使用时，以美元符号($)进行引用。  
- 变量赋值时，'='左右两边不允许出现空格。  
- bash语句结尾不需要分号(;)。  
- 严格的引用变量应该是：${STR}。  
- 单引号之内不解析成变量。  

```
#!/bin/sh
#print hello world in the console window
a="hello world"
echo $a
echo "Hi,${a}s"
```
### 3.2、条件判断
条件判断有2种格式，分别是`test EXPRESSION`和`[ EXPRESSION ]`
#### 3.2.1、test判断
`$?`表示上一条命令的执行结果（0-成功,其它-失败）。

```
test -f /home/skywang/123.txt
echo $?
```

#### 3.2.2、[]判断
中括号的左右扩弧和EXPRESSION之间都必须有空格，如下：

```
$ [ -f /home/skywang/123.txt ]
$ [ "$num" -eq "100" ]
```
#### 3.2.3、逻辑表达式
操作符|含义
:---:|:---
-a   | 逻辑与
-o   | 逻辑或
!    | 逻辑否
#### 3.2.4、数字和字符串比较 
|含义|比较数值|比较字符串| 
|:---:|:----:|:----:| 
|相同|-eq|=|
|不同|-ne|!=|
|大于|-gt|>|
|小于|-lt|<|
|大于或等于|-ge|
|小于或等于|-le|
|为空||-z|
|不为空||-n|
#### 3.2.5、文件比较
操作符|含义
:--------:|:-----
-e file| 文件 file 已经存在
-f file| 文件 file 是普通文件
-s file| 文件 file 大小不为零
-d file| 文件 file 是一个目录  
-r file| 文件 file 对当前用户可以读取  
-w file| 文件 file 对当前用户可以写入  
-x file| 文件 file 对当前用户可以执行  
-g file| 文件 file 的 GID 标志被设置  
-u file| 文件 file 的 UID 标志被设置  
-O file| 文件 file 是属于当前用户的  
-G file| 文件 file 的组 ID 和当前用户相同

### 3.3、流程控制语句
#### 3.3.1、if 语句
- `[]` 表示条件测试。  
- 方括号内左右两边，以及比较符号两边都必须加入空格。  
- 条件之间用 `-a` 表示同时成立，`-o`表示一方成立。  

```
#!/bin/sh
if [ -f 123.txt ]; then
	echo "123.txt exist"
elif [ -f 321.txt ]; then
	echo "321.txt exist"
else
	echo "file not exist"
fi
exit 0
```
#### 3.3.2、for 语句
- 在 for 所在那行的变量 day 是没有加 "$" 符号的  
- [list] 可以是：{1..5} 表示 1、2、3、4、5
- 如果列表被包含在一对双引号中，则被认为是一个元素  
- 如果写成 for day 而没有后面的 in [list] 部分，则 day 将取遍命令行的所有参数

```
#!/bin/sh
for day in [list]
do
　# statments
done
```
#### 3.3.3、while 语句
```
#!/bin/sh
while [ condition ]
do
　　statments
done
```
#### 3.3.4、until 语句
```
#!/bin/sh
until [ condition is TRUE ]
do
　　statments
done
```
#### 3.3.5、case 语句
- 两个分号(;;)表示着语句结束。

```
#!/bin/bash
echo "Hit a key, then hit return."
read Keypress
case "$Keypress" in
        [a-z] ) echo "Lowercase letter";;
        [A-Z] ) echo "Uppercase letter";;
        [0-9] ) echo "Digit";;
        * ) echo "Punctuation, whitespace, or other";;
esac
```
- 另外还有两种非标准写法，不建议使用：
    - 以 ;& 结束，表示会继续执行下边所有语句，无论下面条件匹配不匹配，都会执行。
    - 以 ;;& 结束，表示会继续执行下边所有语句，但是必须条件匹配。（bash中的条件支持模糊匹配）

```
#!/bin/bash  
read -p "你喜欢编程么：（y/n）：" ans  
case $ans in  
    y);&  
    Y) echo "我也是";;  
    n);&  
    N) echo "sorry,跟你没什么好谈的";;  
esac  
```
```
#!/bin/bash  
read -p "请输入一个区号:" num  
case $num in  
    *)echo -n "中国";;&  
    03*)echo -n "河北省";;&  
        ??10)echo "邯郸市";;  
        ??11)echo "石家庄";;  
        ??17)echo "沧州市";;  
    07*)echo -n "江西省";;&  
        ??91)echo "南昌市";;  
        ??92)echo "九江市";;  
        ??97)echo "赣州市";;  
esac   
```
#### 3.3.6、break和continue
- break命令跳出循环，不再执行；
- continue命令跳过本次循环，开始下次循环；

### 3.4、数组
#### 3.4.1 数组定义
一对括号表示是数组，数组元素用“空格”符号分割开，引用数组时从序号0开始。

```
array=(10 20 30 40 50)
# 或
array[0]=10
array[1]=20
array[2]=30
array[3]=40
array[4]=50
# 或
var="10 20 30 40 50"
array=($var)
```

#### 3.4.1 数组操作
- 显示数组中第2项

```
echo ${array[i]}
```
- 显示数组中的所有元素

```
echo ${array[@]}
# 或
echo ${array[*]}
```
- 显示数组长度

```
echo ${#array[@]}
# 或
echo ${#array[*]}
```
- 删除数组第2项元素，unset仅仅只清除值，并没有将该元素删除，不影响数组长度

```
unset array[1]
```
- 删除整个数组

```
unset array
```
- 输出数组的第1-3项

```
echo ${array[@]:0:3}
```
### 3.5、字符运算
#### 3.5.1、字符串定义
|符号|含义| 
|:--------------:|:----| 
${var}				|对变量var的引用，与$var一致
${var-DEFAULT}	|如果var没有申明，则以DEFAULT作为其值
${var:-DEFAULT}	|如果var没有申明，则以DEFAULT作为其值
${var=DEFAULT}	|如果var没有申明，则以DEFAULT作为其值
${var:=DEFAULT}	|如果var没有申明，则以DEFAULT作为其值
${var+DEFAULT}	|如果var没有申明，则以DEFAULT作为其值

#### 3.5.2、字符串操作
- 定义字符串，

```
str="hello world"
```
- 字符串长度，

```
echo ${#str}
```
- 截取字符串，

```
echo ${string:position:length}
```
- 删除子字符串

```
echo ${str#substr}
echo ${str##substr}
echo ${str%substr}
echo ${str%%substr}

```
- 替换字符串 

```
echo ${str/substr/replacement}  #替换第一个匹配的substr
echo ${str//substr/replacement}  #替换所有匹配的substr
```

### 3.5、数值运算
有以下四种方式可以实现

|符号 |效率 |备注 | 
|:----:|:----| :----| 
|(()) |高 |只支持整数 | 
| let | 高 |只支持整数 | 
| expr |中 |只支持整数 | 
| bc |低 |支持整数、浮点数 | 
<!---->
### 3.6、BASH 中的特殊保留字
|符号|含义| 
|:----:|:----| 
$?| 上一个sheel命令执行的结果，0-正常，非0-不正常
$#| 该命令的参数个数，不包含命令本身
$@| 所有的参数,变量之间独立
$*| 所有的参数,所有变量算一个整体
$$| 当前shell的进程号
$!| 上一个子进程的进程号
$0| 当前shell名
$1、$2、...| 第2个参数