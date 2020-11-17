## shell编辑四剑客

### find

find工具主要用户操作系统文件、目录的查找

`find path -option [ -print] [ -exec -ok command] {} \ `

常用参数

- name filename：查找名为filename的文件
- -type b/d/c/p/l/f ：查找块设备、目录、字符设备、管道、符号链接、普通文件
- -size n[c]：查找长度为n块[或n字节]的文件。
- -perm：按执行权限来查找。
- -user username：按文件属主来查找。
- -group groupname：按组来查找。
- -mtime -n +n：按文件修改时间来查找，-n指n天以内，+n值n天以前。

- -atime -n +n：按照文件访问时间来查找文件。
- -ctime -n +n：按照文件创建时间来查找文件。
- -mmin -n +n
- -amin -n +n
- -cmin -n +n
- -nouser：查找无效属主的文件。

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

