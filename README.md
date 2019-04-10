PHP小白工具类
===

PHP懒人专用模块，包含常用（我自己用到）的函数

只是将常用的方法包装一下，比如连接加密、自动连接数据库、自动防SQL注入、统一返回结果等

目前用到的项目：[穷光蛋的忧伤](https://github.com/MRXY001/Poorrow.git)

> 小白（我这样的）专用！（其实是自用）
>  不适合那些大佬！

> 后来我用了ThinkPHP框架，就很少用这个模块了……



## 使用方法 

### 包含模块

```php
require 'public_module.php';
```



### 设置数据库

```php
define("MySQL_servername", "localhost");
define("MySQL_username", "root");
define("MySQL_password", "root");
define("MySQL_database", "poorrow"); // 数据库名称
```



### 设置统一返回值

```php
define("T", "<STATE>OK</STATE>"); // 运行成功输出结果
define("F", "<STATE>Bad</STATE>"); // 运行失败输出结果
```



## 例子

### 获取表单值

这里的表单值都是自动格式化的，防止SQL注入

```php
$name = seize('name'); // 表单中的name值，没有返回NULL
$name = seizeor('name', 'n'); // 获取表单name或者n（左边优先）的值，都没有返回NULL
$name = seize0('name'); // 必须要有name的值且非空，不然直接退出（die）
$name = seize1('name', 0); // 获取name表单，并trim。参数2默认0，非0保留首尾空）
$successed = seize2('name', $name); // 返回表单name是否为空，并将表单值给$name
$name = seizeval('name'); // 获取非空值的name，不存在或者空值都返回NULL
```



### 流程控制

```php
printres(表达式, "false时错误提示", "true时内容"); // 还会输出宏定义的T或者F

die_if(表达式, "错误提示"); // 如果表达式返回false，直接输出错误提示，并终止整个程序
```



### 数值获取

```php
$ip = get_IP(); // 获取IP地址（字符串）
$t = get_time(); // 获取中国地区的当前时间（文本格式）
```



### 数据库

这里的数据库有个全局控制变量 `$con`，如果没有连接的话，会自动连接，因此不必自己手动连接

```php
$sql = "delete from users where user_id = '1'";
query($sql); // 执行SQL语句
```

```php
$sql = "select * from users where username = '$username' and password = '$password'";
if ($row = row($sql)) // 判断有没有这条数据，并且只返回一行的结果
    $user_id = $row['user_id'];
else
    echo '不存在这样的用户名';
```

```php
// 多行数据只能用旧的，只是少了手动判断数据库连接
$result = query("select * from users");
while ($row = mysql_fetch_array($result))
{
    echo XML($row['user_id'], 'USER_ID'); // <USER_ID>123456</USER_ID>
}
```



### 文本操作

```php
$str = XML('text', 'tag'); // 返回 <tag>text</tag>
$str = getMid('ahha', 'a', 'a'); // 取中间文本，返回 hh
```

