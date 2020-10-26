## PHP错误级别
`Parse error` 、`Fatal Error` 、`Waning` 、`Notice` 、`Deprecated`
    
* Deprecated 最低级别的错误(不推荐，不建议)
`使用一些过期函数的时候会出现，程序继续执行`
* Notice 通知级别的错误
`使用一些未定义变量、常量或者数组key没有加引号的时候会出现，程序继续执行`
* Waning 警告级别的错误
`程序出问题了，需要修改代码！！！程序继续执行`
* Fatal Error 错误级别的错误
`程序直接报错，需要修改代码！！！中断程序执行，可使用register_shutdown_function()函数在程序终止前触发一个函数`
* Parse error 语法解析错误
`语法检查阶段报错，需要修改代码！！！中断程序执行，除了修改ini文件，将错误信息写到日志中，什么也做不了`
* E_USER_相关的错误
`用户定义的错误，用户手动抛出错误，进行自定义错误处理`

## PHP错误相关函数

* `ini_set('display_errors', 0);` //关闭错误输出(开发环境开启，生产环境关闭)
* `error_reporting(E_ALL&~E_NOTICE);` //设置错误报告级别
* `ini_set('error_reporting',0);` //设置错误报告级别

## PHP错误配置

* 除了在脚本里设置，还可以在php.ini配置文件里配置
* `error_reporting = E_ALL&~E_NOTICE;` //设置错误报告级别
* `display_errors = 1;` //开发环境开启，生产环境关闭

## PHP异常

* PHP的异常是后来新增特性，与JAVA/C#的异常不同，PHP异常需要手动抛出`throw new Exception`，而不是系统自动抛出
* PHP错误与异常的区别，他们是2个`不同的概念`，但有共同的地方：
```
如果异常不捕获处理，程序将会终止，并报出FatalError 错误，看到这里大家就会觉得异常是不是错误的一种，这是一种错觉，但这样理解也可以。但异常捕获后程序可以继续执行，而真正的FatalError错误出现后程序就必须终止
```
* 异常可以使用`try{}catch(){}` 来捕获捕获，捕获之后后续代码可以继续执行；而错误是无法使用`try{}catch(){}` 捕获的
* 如果抛出了异常，就必须捕获它,否则程序终止执行。

## PHP异常与错误的抛出

1.异常抛出：`throw new Exception('Some Error Message');`
2.错误抛出：`trigger_error()`
3.`trigger_error()` 触发的错误不会被`try-catch` 异常捕获语句捕获
    

## PHP错误处理

* `set_error_handler()`

只能处理`Deprecated`、`Notice`、`Waning`这三种级别错误，而且处理后，脚本将会继续执行发生错误的后一行
    
* `register_shutdown_function()`

这个方法是脚本结束前的最后一个回调函数，所以无论是die()/错误（异常）/还是脚本正常结束都会调用。
通过这种方法就可以巧妙的打印出程序结束前所有的错误信息。但是我在测试的时候我发现并不是所有的错误终止后都会调用这个函数，可以看下面的一个测试文件，内容是：
```
<?php
    register_shutdown_function('zyfshutdownfunc');
    function zyfshutdownfunc()
    {
        if ($error = error_get_last()) {
            var_dump('<b>register_shutdown_function: Type:' . $error['type'] . ' Msg: ' . $error['message'] . ' in ' . $error['file'] . ' on line ' . $error['line'] . '</b>');
        }
    }
    var_dump(23+-+); //此处语法错误
?>
```
可以看到根本就不会触发zyfshutdownfunc()函数，其实这是一个语法错误，直接报了一个：
```
<?php
     Parse error: syntax error, unexpected ')' in /www/mytest/exception/try-catch.php on line 71
 ?>
```
由此引出一个奇葩的问题：问什么不能触发，为什么框架中是可以的？其实原因很简单，只在parse-time出错时是不会调用本函数的。只有在run-time出错的时候，才会调用本函数

我的理解是语法检查器前没有执行register_shutdown_function()去把需要注册的函数放到调用的堆栈中，所以就根本不会运行。那框架中为什么任何错误都能进入到register_shutdown_function()中呢，其实在框架中一般会有统一的入口index.php，然后每个类库文件都会通过include ** 的方式加载到index.php中，相当与所有的程序都会在index.php中聚集，
## PHP异常处理

* `set_exception_handler()`

设置默认的异常处理程序，有try/catch捕获的话这个函数就不会执行，反之就会执行，而且执行的话，脚本将不会继续执行发生异常的后一行代码，程序马上中止
    
* `set_exception_handler()`注意事项

set_exception_handler(“myException”) 不仅可以接受**函数名**，还可以接受**类的方法**（*公开的静态方法* 及*公开的非静态方法* 都可以），但需要以**数组形式** 传递，数组的第一值为“**类名**”，第二个参数为“**方法名**”，如下代码所示：
```
<?php
classApp{
function myException($exception){
echo"<b>Exception:</b> " , $exception->getMessage();
    }
}

set_exception_handler(array('App','myException'));

throw new Exception('Uncaught Exception occurred');
?>
```
## PHP异常分类

* 由于用户行为导致出现异常
```
1、没有通过验证器
2、没查询到结果
3、需要向用户返回具体信息
4、不需要记录日志
5、可作为异常或者不作为异常，根据需求和个人情况而定
```
* 由于服务器自身导致出现异常
```
1、代码出错
2、调用第三方接口错误
3、不需要向用户返回具体信息
4、需要记录日志
```

## 在程序中PHP异常的自动抛出

* 由于PHP异常是后面版本新增的特性，设计上与JAVA/C#的异常不一样，JAVA的异常大部分是系统自动抛出，而`PHP异常不是系统自动抛出`，需要`手动抛出`，`导致PHP异常在程序中的作用减半`(异常就是意料之外的事情，根本我们意料不到的，如果用手动抛出，证明已经预先预料到了，那异常的意义就变味了)
* 在PHP中`异常是手动抛出的`，而`错误是系统自动抛出的`（也可手动抛）
* 我们需要把`异常做成系统自动抛出接`（例如JAVA）就必须**借助**`错误`（这三种错误`Deprecated`、`Notice`、`Waning`，其他的错误不行，因为会终止程序运行）

```
<?php

    set_error_handler('error_handler');

function error_handler($errno, $errstr, $errfile, $errline){
        throw new Exception($errstr);
    }

try {
        $num =100 /0;
    }catch(Exception $e) {
echo $e -> getMessage() .'<br/>';
    }

echo"end";
?>

执行结果：
Division by zero
end
```
## PHP7 异常处理的大变化

* 一段TP5源代码引出`PHP7异常`的变化![图片描述](/img/bVN61t?w=765&h=662 "图片描述")明明set_exception_handler()函数只可以捕获`Exception类或派生类的对象`，为何还需要捕获的对象做**判断**呢？结果引出了`PHP7`的变化，请看下面分析
* 前面已经讲过异常是需要`手动抛出`，及时上面所说的方法最多也是把`Deprecated`、`Notice`、`Waning`这3类错误**封装成系统自动抛出的异常**，但致命错误仍然还是**无法封装**成系统自动抛出的异常，因为致命错误（Fatel Error）仍然无法捕获
* 在PHP7之前，`Deprecated`、`Notice`、`Waning`这类错误是可以捕获的（使用set_error_handler()函数），而`Fatel Error`无法捕获的
* 在PHP7之后，出现了一个异常与错误通用的接口Throwable，Exception类与Error类都实现了该接口，导致`Error类或Error类的派生类的错误对象`（大部分Fatel Error,而之前三类错误不变）也可以像Exception一样被捕获（2种捕获方法：1、try/catch  2、set_exception_handler（））
* 示例代码

```
try{
    go();//该函数未定义
}catch(Exception $e){
//捕获异常
}catch(Error $er){
//捕获错误
}
```