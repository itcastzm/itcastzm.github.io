# 各种软件工具安装


## mysql安装
### windows下mysql 8.0.15 详细安装使用教程
1. 官网下载zip 
下载community 社区版
2. 解压，复制到指定目录。新建data文件。添加环境变量
注意需要新建data文件  可以在解压后的文件夹里面建也可以在自定义的地方新建data文件夹
3. 将mysql解压目录（安装目录）bin目录路径添加到系统环境变量path中 方便cmd命令行调用
4. 新建my.ini文件
```
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\\MySQL\\mysql-8.0.15-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\MySQL\\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
#mysql_native_password
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```
5. 管理员运行命令行窗口 配置MySQL
打开命令界面 输入：
```shell
mysqld --install --console
//再次  并注意观察控制台 打印出的密码  需要记住
mysqld --initialize --console 
```

6. 启动mysql服务

```shell
net start mysql
//如果说服务名无效
mysqld --install --console根据提示说服务已经存在，但还是服务名无效，后来我把console参数去掉，执行
mysql --install
// 显示服务安装成功，然后启动
net start mysql
//停止服务：net stop mysql
```

7. 启动登录

```shell
mysql -u root -p   //回车，输入默认的那个极其安全的密码。
// 使用数据库：
use mysql
//它会提示你先要重新设置密码。执行
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```