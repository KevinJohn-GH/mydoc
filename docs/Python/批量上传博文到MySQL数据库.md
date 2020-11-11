# 批量上传博文到MySQL数据库

由于本人的笔记都存在本地，数量有点多。手动一篇一篇上传会很麻烦。于是用python编写了一个自动上传markdown笔记的脚本。

```python
import mysql.connector
import os
import datetime

"""连接数据库"""
mydb = mysql.connector.connect(
  host="mysqlserver-address",
  user="mysql-user-name",
  passwd="xxxxxx",
  database="xxx",
  auth_plugin='mysql_native_password'	# MySQL8.0更改了密码验证方式
)

mycursor = mydb.cursor()

path = "D:\\uploadMyblog"  # 文件夹目录
files = os.listdir(path)  # 得到文件夹下的所有文件名称

for file in files:  # 遍历文件夹
    if not os.path.isdir(file):  # 判断是否是文件夹，不是文件夹才打开
        f = open(path+'\\' + file, encoding='utf-8')
        data = f.readlines()
        f.close()

        """转义字符"""
        title = ''.join(data[0][1:]).replace("\\", "\\\\")\
                                    .replace("'", "\\'")\
                                    .replace("\"", "\\\"")

        content = ''.join(data[1::]).replace("\\", "\\\\")\
                                    .replace("'", "\\'")\
                                    .replace("\"", "\\\"")

        sql = ("insert into blog2_examplemodel "
               "(title, content, create_time, update_time) "
               "values (%s, %s, %s, %s);")

        sqltime = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        sqldata = (title, content, sqltime, sqltime)

        """执行sql语句"""
        mycursor.execute(sql, sqldata)


"""提交更改"""
mydb.commit()

print('finish')

```

