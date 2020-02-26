---
title: 九个PHP很有用的功能
top: false
cover: false
date: 2020-02-25 21:27:31
password:
summary:
tags:
 - php
categories: 后端
---

水一篇，网上看到一篇时间久远的文章，特地搬过来。[九个PHP很有用的功能]("https://coolshell.cn/articles/2394.html")

#### 1. 函数的任意数目的参数

> php定义的一个可以接收任意参数的函数：```func_get_args```，下面请看示例

````
function test(){
  $args = func_get_args();
      $i = 1;
      foreach($args as $value){
          echo '第'.$i++.'个参数:'.$value;
          echo PHP_EOL;
      }
}
test();
//result null
test('hello');
//result 
// 第1个参数:hello
test('hello','world','!');
//result 
// 第1个参数:hello
// 第2个参数:world
// 第3个参数:!
````
#### 2.使用 Glob() 查找文件


> [glob()](http://us.php.net/manual/en/function.glob.php "") 函数比较不常见，可能都不知道这个函数是用来干什么的，除非你对它已经很熟悉了。
这里，和[scandir()](http://php.net/manual/en/function.scandir.php "") 一样，其可以用来查找文件。

````
// 取得所有的后缀为PHP的文件
$files = glob('*.php');
 
print_r($files);
/* 输出:
Array
(
    [0] => phptest.php
    [1] => post_output.php
    [2] => test.php
)
*/
````
还可以查找多种后缀名格式文件
````
// 取PHP文件和TXT文件
$files = glob('*.{php,txt}', GLOB_BRACE);
 
print_r($files);
/* 输出:
Array
(
    [0] => phptest.php
    [1] => pi.php
    [2] => post_output.php
    [3] => test.php
    [4] => log.txt
    [5] => test.txt
)
*/
````
还可以加上路径
````
$files = glob('../images/a*.jpg');
 
print_r($files);
/* 输出:
Array
(
    [0] => ../images/apple.jpg
    [1] => ../images/art.jpg
)
*/
````

#### 3.内存使用信息

> PHP 是有垃圾回收机制的，而且有一套很复杂的内存管理机制。想知道你的脚本所使用的内存情况。可以尝试下[memory_get_usage()](http://us2.php.net/manual/en/function.memory-get-usage.php "") 函数，如果你想知道使用内存的峰值，还可以调用[memory_get_peak_usage()](http://us2.php.net/manual/en/function.memory-get-peak-usage.php "") 函数。

````
echo "Initial: ".memory_get_usage()." bytes \n";
/* 输出
Initial: 361400 bytes
*/
 
// 使用内存
for ($i = 0; $i < 100000; $i++) {
    $array []= md5($i);
}
 
// 删除一半的内存
for ($i = 0; $i < 100000; $i++) {
    unset($array[$i]);
}
 
echo "Final: ".memory_get_usage()." bytes \n";
/* prints
Final: 885912 bytes
*/
 
echo "Peak: ".memory_get_peak_usage()." bytes \n";
/* 输出峰值
Peak: 13687072 bytes
*/
````

#### 4.CPU使用信息

> [getrusage()](http://us2.php.net/manual/en/function.getrusage.php "") 函数可以让你知道CPU的使用情况。注意，这个功能在Windows下不可用。

````
print_r(getrusage());
/* 输出
Array
(
    [ru_oublock] => 0
    [ru_inblock] => 0
    [ru_msgsnd] => 2
    [ru_msgrcv] => 3
    [ru_maxrss] => 12692
    [ru_ixrss] => 764
    [ru_idrss] => 3864
    [ru_minflt] => 94
    [ru_majflt] => 0
    [ru_nsignals] => 1
    [ru_nvcsw] => 67
    [ru_nivcsw] => 4
    [ru_nswap] => 0
    [ru_utime.tv_usec] => 0
    [ru_utime.tv_sec] => 0
    [ru_stime.tv_usec] => 6269
    [ru_stime.tv_sec] => 0
)
 
*/
````

这个结构看上出很晦涩，除非你对CPU很了解。下面一些解释：
* ru_oublock: 块输出操作
* ru_inblock: 块输入操作
* ru_msgsnd: 发送的message
* ru_msgrcv: 收到的message
* ru_maxrss: 最大驻留集大小
* ru_ixrss: 全部共享内存大小
* ru_idrss:全部非共享内存大小
* ru_minflt: 页回收
* ru_majflt: 页失效
* ru_nsignals: 收到的信号
* ru_nvcsw: 主动上下文切换
* ru_nivcsw: 被动上下文切换
* ru_nswap: 交换区
* ru_utime.tv_usec: 用户态时间 (microseconds)
* ru_utime.tv_sec: 用户态时间(seconds)
* ru_stime.tv_usec: 系统内核时间 (microseconds)
* ru_stime.tv_sec: 系统内核时间?(seconds)

要看到你的脚本消耗了多少CPU，我们需要看看“用户态的时间”和“系统内核时间”的值。秒和微秒部分是分别提供的，您可以把微秒值除以100万，并把它添加到秒的值后，可以得到有小数部分的秒数。
````
// sleep for 3 seconds (non-busy)
sleep(3);
 
$data = getrusage();
echo "User time: ".
    ($data['ru_utime.tv_sec'] +
    $data['ru_utime.tv_usec'] / 1000000);
echo "System time: ".
    ($data['ru_stime.tv_sec'] +
    $data['ru_stime.tv_usec'] / 1000000);
 
/* 输出
User time: 0.011552
System time: 0
*/
````
sleep是不占用系统时间的，我们可以来看下面的一个例子：
````
// loop 10 million times (busy)
for($i=0;$i<10000000;$i++) {
 
}
 
$data = getrusage();
echo "User time: ".
    ($data['ru_utime.tv_sec'] +
    $data['ru_utime.tv_usec'] / 1000000);
echo "System time: ".
    ($data['ru_stime.tv_sec'] +
    $data['ru_stime.tv_usec'] / 1000000);
 
/* 输出
User time: 1.424592
System time: 0.004204
*/
````
这花了大约14秒的CPU时间，几乎所有的都是用户的时间，因为没有系统调用。
系统时间是CPU花费在系统调用上的上执行内核指令的时间。下面是一个例子：
````
$start = microtime(true);
// keep calling microtime for about 3 seconds
while(microtime(true) - $start < 3) {
 
}
 
$data = getrusage();
echo "User time: ".
    ($data['ru_utime.tv_sec'] +
    $data['ru_utime.tv_usec'] / 1000000);
echo "System time: ".
    ($data['ru_stime.tv_sec'] +
    $data['ru_stime.tv_usec'] / 1000000);
 
/* prints
User time: 1.088171
System time: 1.675315
*/
````
我们可以看到上面这个例子更耗CPU。
#### 5.系统常量
PHP 提供非常有用的[系统常量](http://php.net/manual/en/language.constants.predefined.php "") 可以让你得到当前的行号 (__LINE__)，文件 (__FILE__)，目录 (__DIR__)，函数名 (__FUNCTION__)，类名(__CLASS__)，方法名(__METHOD__) 和名字空间 (__NAMESPACE__)，很像C语言。下面是一个例子。
````
namespace src;
class Test{
  //行号
  public function getLine(){
    return __LINE__;
  }
  //当前文件名(包含路径)
  public function getFile(){
    return __FILE__;
  }
  //当前目录
  public function getDir(){
    return __DIR__;
  }
  //当前函数名称
  public function getFunction(){
    return __FUNCTION__;
  }
  //当前类名
  public function getClass(){
    return __CLASS__;
  }
  //当前方法(带命名空间)
  public function getMethod(){
    return __METHOD__;
  }
  //当前命名空间(带命名空间)
  public function getNamespace(){
    return __NAMESPACE__;
  }
}

$obj = new Test();

echo $obj->getLine().PHP_EOL;
echo $obj->getFile().PHP_EOL;
echo $obj->getDir().PHP_EOL;
echo $obj->getFunction().PHP_EOL;
echo $obj->getClass().PHP_EOL;
echo $obj->getMethod().PHP_EOL;
echo $obj->getNamespace().PHP_EOL;
//result:
//6
//D:\workspace\damon\test.php
//D:\workspace\damon
//getFunction
//src\Test
//src\Test::getMethod
//src
````

#### 6.生成唯一的ID

> 生成唯一ID的场景还是应用挺多的，[uniqid()](http://us2.php.net/manual/en/function.uniqid.php "")函数不失为一个solution
````
// generate unique string
echo uniqid();
/* 输出
4bd67c947233e
*/
 
// generate another unique string
echo uniqid();
/* 输出
4bd67c9472340
*/
````
加上前缀避免重名：
````
// 前缀
echo uniqid('foo_');
/* 输出
foo_4bd67d6cd8b8f
*/
 
// 有更多的熵
echo uniqid('',true);
/* 输出
4bd67d6cd8b926.12135106
*/
 
// 都有
echo uniqid('bar_',true);
/* 输出
bar_4bd67da367b650.43684647
*/
````
#### 7.序列化

这里本来是要讲[serialize()](http://php.net/manual/en/function.serialize.php "") 和[unserialize()](http://www.php.net/manual/en/function.unserialize.php "")函数的，但是数据存储json更流行。网上也就serialize和json单独拿出来讨论过：[传送门]("https://blog.csdn.net/qq_16241043/article/details/53261042"),这里也就讲讲他们的优劣：

* 序列化使用serialize，支持对象的存储，json不支持

* JSON支持UTF-8编码的数据
* JSON在数据量大的情况下，效率会比serialize高

#### 8.字符串压缩
当我们说到压缩，我们可能会想到文件压缩，其实，字符串也是可以压缩的。PHP提供了?[gzcompress()](http://php.net/manual/en/function.gzcompress.php "") 和[gzuncompress()](http://www.php.net/manual/en/function.gzuncompress.php "") 函数：
````
$string =
"Lorem ipsum dolor sit amet, consectetur
adipiscing elit. Nunc ut elit id mi ultricies
adipiscing. Nulla facilisi. Praesent pulvinar,
sapien vel feugiat vestibulum, nulla dui pretium orci,
non ultricies elit lacus quis ante. Lorem ipsum dolor
sit amet, consectetur adipiscing elit. Aliquam
pretium ullamcorper urna quis iaculis. Etiam ac massa
sed turpis tempor luctus. Curabitur sed nibh eu elit
mollis congue. Praesent ipsum diam, consectetur vitae
ornare a, aliquam a nunc. In id magna pellentesque
tellus posuere adipiscing. Sed non mi metus, at lacinia
augue. Sed magna nisi, ornare in mollis in, mollis
sed nunc. Etiam at justo in leo congue mollis.
Nullam in neque eget metus hendrerit scelerisque
eu non enim. Ut malesuada lacus eu nulla bibendum
id euismod urna sodales. ";
 
$compressed = gzcompress($string);
 
echo "Original size: ". strlen($string)."\n";
/* 输出原始大小
Original size: 800
*/
 
echo "Compressed size: ". strlen($compressed)."\n";
/* 输出压缩后的大小
Compressed size: 418
*/
 
// 解压缩
$original = gzuncompress($compressed);
````
几乎有50% 压缩比率。同时，你还可以使用?[gzencode()](http://www.php.net/manual/en/function.gzencode.php "") 和[gzdecode()](http://www.php.net/manual/en/function.gzdecode.php "") 函数来压缩，只不用其用了不同的压缩算法。
#### 9. 注册停止函数
有一个函数叫做?[register_shutdown_function()](http://www.php.net/manual/en/function.register-shutdown-function.php "")，可以让你在整个脚本停时前运行代码。让我们看下面的一个示例：
````
// capture the start time
$start_time = microtime(true);
 
// do some stuff
// ...
 
// display how long the script took
echo "execution took: ".
        (microtime(true) - $start_time).
        " seconds.";
 ````
上面这个示例只不过是用来计算某个函数运行的时间。然后，如果你在函数中间调用?[exit()](http://php.net/manual/en/function.exit.php "") 函数，那么你的最后的代码将不会被运行到。并且，如果该脚本在浏览器终止（用户按停止按钮），其也无法被运行。
而当我们使用了register_shutdown_function()后，你的程序就算是在脚本被停止后也会被运行：
````
$start_time = microtime(true);
 
register_shutdown_function('my_shutdown');
 
// do some stuff
// ...
 
function my_shutdown() {
    global $start_time;
 
    echo "execution took: ".
            (microtime(true) - $start_time).
            " seconds.";
}
````
这里，还有类似的函数fastcgi_finish_request有同样妙用，这里使用场景会有不同。
* register_shutdown_function在一个请求request执行完毕时，调用这个注册的方法，可以捕捉error信息。

* fastcgi_finish_request 在调用这个方法后，再有任何输出内容，都不会输出后客户端。