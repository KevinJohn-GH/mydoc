## shell编辑四剑客

### find

find工具主要用户操作系统文件、目录的查找

`find path -option [ -print] [ -exec -ok command] {} \ `

常用参数

- `name filename`：查找名为filename的文件
- `-type b/d/c/p/l/f `：查找块设备、目录、字符设备、管道、符号链接、普通文件
- `-size n[c]`：查找长度为n块[或n字节]的文件。
- `-perm`：按执行权限来查找。
- `-user username`：按文件属主来查找。
- `-group groupname`：按组来查找。
- `-mtime -n +n`：按文件修改时间来查找，-n指n天以内，+n值n天以前。

- `-atime -n +n`：按照文件访问时间来查找文件。
- `-ctime -n +n`：按照文件创建时间来查找文件。
- `-mmin -n +n`
- `-amin -n +n`
- `-cmin -n +n`
- `-nouser`：查找无效属主的文件。

##### 案例

```shell
# -name
find /data/ -name "[A-Z]*" # 查找data目录以大写字母开头的文件


# -type
find /data/ ! -type d #查找data目录下的非文件目录
find /data/ -type d | xargs chmod 755 -R # 查找目录类型并将权限设置为755

# -size
find /data/ -size +10M # 文件大小大于10M的文件
find /data/ -size 10M # 文件大小为10M的文件
find /data/ -size -10M # 文件大小小于10M的文件

# -perm
find /data/ -perm 755 #查找文件权限为755的文件或目录

# -mitme
find /data/ -mtime +30-name "*.log" # 查找30天以前的log文件
find /data/ -mtime -30name "*.log" # 查找30天内的log文件

#查找/data目录以.log结尾，文件大于10kb的文件，同时cp到/tmp目录
find /data -name "*.log" -type f -size + 10k -exec cp {} /tmp/ \;
```

⭐`-exec`以`\;`结尾，`{}`相当于前面传来的文件



### sed

非交互式文本编辑器。

```shell
sed [- Optios] ['Commands'] filename;
```

- `x`: 指定行号。
- `x,y`: 指定从x到y的行号范围。
- `/pattern/`: 查询包含模式的行。
- `/pattern/pattern/`: 查询包含两个模式的行
- `/pattern/,x`: 从与pattern的匹配行到x号行之间的行。
- `x,/pattern/`: 从x号行到与pattern的匹配行之间的行。
- `x,y!`: 查询不包含x和y行号的行。
- `p`: 打印匹配行。
- `=`: 打印文件行号。
- `a\`: 在定位行号之后追加文本信息。
- `i\`: 在定位行号之前插入文本信息。
- `d`: 删除指定行。
- `c\`: 用新文本替换定位文本。
- `x`: 互换模式缓冲区和保持缓冲区的内容

##### 案例

```shell
# 打印yes文本1~3行
sed -n '1,3p' yes.txt

# 打印yes文本第一行与最后一行
sed -n '1p;$p' yes.txt

# 打印yes文本第一行至第三行、删除匹配行至最后一行
sed '1,3d' yes.txt
sed '/50/, $d' yes.txt

# 删除yes文本最后6行及删除最后一行
for i in `seq 1 6`;do sed  '$d' yes.txt; done

# 查找50所在的行，并在后面添加05字符串
sed '/50/a05' yes.txt

# 查找50所在的行，并在前面添加11111字符串
sed '/50/i11111' yes.txt

# 查找0结尾所在的行，并在结尾添加11111
sed 's/0$/&11111/g' yes.txt
sed '/0$/s/^/11111/g' yes.txt

# 查找0结尾所在的行，并在行首添加11111
sed '/0$/s/^/&11111/' yes.txt

# 多个sed命令组合，使用-e参数
sed -e 'pattern1' -e 'pattern2' yes.txt

# 多个sed命令祝贺，使用;分割
sed -e 'pattern1' ; 'pattern2'

# sed读取系统变量，进行变量替换
WEBSITE=WWW.BAIDU.COM
sed 's/WWW.GOOLGE.COM/$WEBSITE/g' yes.txt
```

⭐使用`s///g`，g表示全局搜索，不带g则是匹配每行的第一个

#### sed高级命令

- `N、D、P`: 处理多行模式空间的问题。
- `H、h、G、g、x`: 将模式空间的内容放入存储空间以便接下来的编辑。
	- g：[address[,address]]g 将hold space中的内容拷贝到pattern space中，原来pattern space里的内容清除

	- G：[address[,address]]G 将hold space中的内容append到pattern space\n后

	- h：[address[,address]]h 将pattern space中的内容拷贝到hold space中，原来的hold space里的内容被清除

	- H：[address[,address]]H 将pattern space中的内容append到hold space\n后

	- d：[address[,address]]d 删除pattern中的所有行，并读入下一新行到pattern中

	- D：[address[,address]]D 删除multiline pattern中的第一行，不读入下一行
- `:、b、t`: 在脚本中实现分支与条件结构。


##### 保持空间与模式空间

```
sed ‘1!G;h;$!d’mm
```

ps：1!G第1行不 执行“G”命令，从第2行开始执行。

​    $!d，最后一行不删除（保留最后1行）

图解分析过程

P：Pattern Space

H：Hold Space

蓝色：Hold Space中的数据

绿色：Pattern Space中的数据

![img](https://cdn.jsdelivr.net/gh/KevinJohn-GH/pictures/img/20201117170513.png)

##### 案例

```shell
# 每行后面插入空行、每行后插入两行空行、前三行每行后插入空行
sed '/^$/d;G' yes.txt	# /^$/d删除空行, 此时保持空间为空，所以插入的为空行
sed '/^$/d;G;G' yes.txt # 连续两次插入保持空间的内容
sed '/^$/d;1,3G;' yes.txt

# 将yes文本偶数行删除及隔两行删除一行
sed 'n;d' yes.txt	# n将下一行也添加进模式空间，后面的命令都对下一行进行
sed 'n;n;d' yes.txt

# 在yes匹配行前一行、后一行插入空行、同时在匹配前后插入空行
sed '/50/{x;p;x;}' yes.txt
sed '/50/G' yes.txt
sed '/50/{x;p;x;G;}' yes.txt

# 每行前加入顺序数字序号、加上制表符\t和.
sed = yes.txt | sed 'N;s/\n/ /'
sed = yes.txt | sed 'N;s/\n/\t/'
sed = yes.txt | sed 'N;s/\n/./'

# 删除行前和行尾的任意空格
sed 's/^[\t]*//;s/[\t]/*$//' yes.txt

# 打印关键字old和new之间的内容
sed -n '/old,/new/'p yes.txt

# 打印及删除最后两行
sed '$!N;$!D' yes.txt
sed 'N;$!P;$!D;$d' yes,txt

# 
```

⭐`sed  '/5/{n;d}' yes.txt`删除指定字符串的下一行

⭐N命令：将下一行添加到pattern space中。将当前读入行和用N命令添加的下一行看成“一行”。

⭐n命令：读取下一行到pattern space。

⭐P命令： 打印模板块的第一行

