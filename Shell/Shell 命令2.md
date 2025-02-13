## 1. ``(command)``

命令在子 shell 中执行，命令的执行结果对当前 shell 环境不会有影响

## 2. 输入

### 2.1. read 命令

read 命令一个一个词组地接收输入的参数，每个词组需要使用空格进行分隔。

```shell
read [options] variable1 variable2 ... variableN
```

+ **变量接收输入**
	`read` 按顺序将用户输入按空格拆分，并赋值给指定变量。
+ **词组超限规则**
	如果用户输入的词组数量多于指定变量数量，超出的词组会作为整体赋值给最后一个变量。
+ **默认变量**
	如果未指定变量，`read` 会将输入赋值给默认变量 `REPLY`。
+ **判空**
	如果用户未输入内容，变量值为空。
	可以通过条件检查输入是否为空：

	```shell
	if [ -z "$variable" ]; then
	  echo "Input is empty!"
	fi
	```

## 3. 输出

### 3.1. echo 命令

Shell 的 echo 指令与 PHP 的 echo 指令类似，都是用于字符串的输出。

语法

```shell
echo [options] string
```

options:

+ `-e`：开启转义字符处理，**默认禁用转义字符**。
+ `-n`：不在输出末尾添加换行符。**默认在字符串末尾添加换行符。**
+ `-E`：禁用转义字符处理。默认行为。

arguments:

+ `string`：
	+ 普通字符串直接输出
	+ 如果字符串中包含特殊字符，需要使用双引号包裹。（推荐）
	+ 用反引号包裹后，会将内容作为命令执行，并输出其结果。
		+ 反引号可以使用`$()`替代。（推荐）

#### 3.1.1. 颜色输出文本

```shell
echo -e "\e[nm文本"

# 删除所有属性
echo -e "\e[0m文本"
```

+ `-e` ： 开始转义字符
+ 符号释义
	+ `\e`：转义起始符号，可以使用`\033` 和 `\x1B` 代替
	+ `[` ：表示开始定义颜色
	+ `n` ：数字字符
	+ `m`：转义终止符，表示颜色定义完毕
+ 可以同时使用多个颜色方案
	+ 相同类型的颜色方案，会只保留最后一条
	+ 不同类型的颜色方案，会同时生效
+ `\e[0m`：重设所有属性。

##### n 对应格式

```shell
# 格式
# 1 粗体
# 2 黯淡
# 4 下划线
# 5 闪烁
# 7 反转
# 8 隐藏

# 字体颜色
# 30 - 37 
# 90 - 97 
# 39 默认颜色

# 背景色
# 40 - 47
# 100 - 107
# 49 默认背景色
```

##### 88-256色

```shell
# 字体颜色
# n 为 1 - 256
echo -e "\e[38;5;nm文本"

# 背景色
# n 为 1 - 256
echo -e "\e[48;5;nm"
```

+ 字体颜色的代码为`\e[38;5;` + `n` + `m`
+ 背景颜色的代码为`\e[48;5;` + `n`+ `m`
+ `n`：范围为$[1-26]$

### 3.2. printf 命令

`printf` 是一种模仿 C 程序库中 `printf()` 函数的命令，具有强大的格式化输出功能。与 `echo` 命令相比，`printf` 的主要优点是可精确控制输出的格式，因此它比 `echo` 更加灵活和移植性强。

语法

```shell
printf format-string [arguments...]
```

+ format-string：一个格式字符串，包含普通文本和格式说明符
+ arguments：用于填充格式说明符

常见格式化占位符

| 占位符       | 描述               |
| --------- | ---------------- |
| `%s`      | 输出字符串            |
| `%d`      | 输出十进制整数          |
| `%f`      | 输出浮点数            |
| `%e`      | 科学计数法浮点数         |
| `%x`/`%X` | 输出十六进制数          |
| `%o`      | 输出八进制数           |
| `%b`      | 输出二进制数           |
| `%c`      | 输出字符，参数为 ASCII 值 |
| `%%`      | 输出一个百分号 %        |

格式控制案例

```shell
printf "%s" "string" # 输出字符串
printf "%10s" "string" # 输出宽度为10的字符，超过截断，不足则补齐，默认右对齐
printf "%-10s" "string" # 输出宽度为10 的字符串， - 表示左对齐，没有则表示右对齐

printf "%s" "string1" "string2" # 格式占位符数量少于字符串数量，printf 循环使用 format-string

printf "%s %d" # 格式占位符没有对应的 argument，则将默认值输出
```

详情看C语言printf函数

### 3.3. tee 命令

读取标准输入中的内容，并写入到标准输出和文件中

语法

```bash
tee file

# 重定向输入
command | tee file

# 重定向输入
tee file < input_file
```

## 4. 磁盘相关

### 4.1. fdisk

查看挂载磁盘

```shell
sudo fdisk -l
```

### 4.2. mount

挂载磁盘

```shell
mount 磁盘路径 目录路径
```

### 4.3. df

1 作用

查看硬盘空间信息

2 语法

```shell
df [-h]
```

+ `[-h]`：更人性化的单位显示

## 5. 网络相关

### 5.1. ping

向指定 IP 发送 ping 包

```shell
ping [options] URL
```

+ URL 网址

options：

+ `-cn`：发送ping包的次数

## 6. 服务相关

```shell
ps aux
```

## 7. 文件相关

### 7.1. basename 命令

返回路径中最后的文件名

```shell
bashename /path/of/yourfile
```

### 7.2. dirname命令

返回文件的路径

```shell
dirname /path/of/yourfile
```

## 8. 环境相关

查看某一个环境变量

 ```shell
 echo $变量名
```

查看全部环境变量

```shell
env
```

新增一个环境变量

```shell
source export variable=value
```

重新加载配置文件

```shell
source /path/of/file
# or
. /path/of/file
```

## 9. 随机数生成函数 `shuf`

语法

```shell
shuf -i n1-n2 -n num3
```

+ `-i num1-num2`：指定生成随机数的范围
+ `-n num3` ：指定生成随机数的个数

### 9.1. $RANDOM

随机生成一个 0-32767之间的数字

let

指定编码

```shell
# coding:utf-8
```
