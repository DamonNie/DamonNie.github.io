---
title: php反射
top: false
cover: false
date: 2020-02-18 22:54:15
password:
summary: PHP中的反射API就像Java中的java.lang.reflect包一样。它由一系列可以分析属性、方法和类的内置类组成。
 - 设计模式
categories: 后端
---

## 概念
> PHP5 具有完整的反射 API，如Java中的java.lang.reflect包一样，有对类、接口、函数、方法和扩展进行反向工程的能力。在某些方面和对象函数相似，比如get_class_vars()。反射API也可与PHP最新的面向对象特性一起工作，如访问控制、接口和抽象类。

### 反射API的部分类
使用反射API这些类，可以获得在运行时访问对象、函数和脚本中的扩展的信息。

|**类**|**描     述**|
|Reflection|为类的摘要信息提供静态函数export()|
|ReflectionClass|类信息和工具|
|ReflectionMethod|类方法信息和工具|
|ReflectionParameter|方法参数信息|
|ReflectionProperty|类属性信息|
|ReflectionFunction|函数信息和工具|
|ReflectionExtension|PHP扩展信息|
|ReflectionException|错误类|

### 使用
相信大家对php反射已经有一定的了解，在php主流框架中像是依赖注入、对象池、类加载等等，都有比较常见的应用。

这里，我们建一个基础的容器类：
````
class Container{
    public static function getInstance($class_name,$params = []){
        //获取反射实例
        $reflect = new ReflectionClass($class_name);
        //获取反射实例的构造方法
        $construct = $reflect->getConstruct();
        //获取反射实例构造方法的形参
        $di_params = [];
        if($construct){
            foreach($construct->getParams as $param){
                $class = $param->getClass();
                    // 如果参数是一个类，创建实例
                    if ($class) { 
                        $di_params[] = new $class->name;
                    }
            }
        }
        $di_params = array_merge($di_params,$param);
        //通过参数$di_params创建一个新的类实例。
        return $reflect->newInstanceArgs($di_params);
    }
}
````
这里获取构造方法参数是用了ReflectionClass类，具体其他相关类的适用可以参考[PHP官方手册-反射]("https://www.php.net/manual/zh/reflectionclass.newinstanceargs.php")

### 实现自动注入依赖

````
class A{
    protected $num = 10;
    public function getNum(){
        return $this->num;
    }
}
class B{
    protected $total;
    public function __construct(A $obj,$num){
        $this->total = $obj->getNum() + $num;
    }
    public function getTotal(){
        return $this->total;
    }
}

$b = Container::getInstance(B::class, [10]);
var_dump($b->getCount()); // result is 20
````
> #### 进阶

上面代码虽然进行了注入，但是只有一层。如果A类也有注入呢？

````
class Container{
    public static function getInstance($class_name,$params = []){
        //获取反射实例
        $reflect = new ReflectionClass($class_name);
        //获取反射实例的构造方法
        $construct = $reflect->getConstruct();
        //获取反射实例构造方法的形参
        $di_params = [];
        if($construct){
            foreach($construct->getParams as $param){
                $class = $param->getClass();
                    // 如果参数是一个类，创建实例
                    if ($class) { 
                        $di_params[] = self::getInstance($class->name);
                    }
            } 
        }
        $di_params = array_merge($di_params,$param);
        //通过参数$di_params创建一个新的类实例。
        return $reflect->newInstanceArgs($di_params);
    }
}
````
测试一下：
````
class C{
    public $num = 1;
}
class A{
    protected $num = 10;
    public function __construct(C $c){
        $this->num = $this->num + $c->num;
    }
    public function getNum(){
        return $this->num;
    }
}
class B{
    protected $total;
    public function __construct(A $a,$num){
        $this->total = $a->getNum() + $num;
    }
    public function getTotal(){
        return $this->total;
    }
}

$b = Container::getInstance(B::class, [10]);
var_dump($b->getCount()); // result is 21
````

### 单例

有些类在程序的生命周期会频繁的使用，为了在依赖注入中避免经常性的创建实例，我们依靠单例模式的设计，使已经是依赖的对象可以被直接获取
````
class Container{
    protected staic $_singleton = [];
    //增加一个实例到单例中
    public static function singleton($instance){
        if(!is_object($instance)){
            throw new \Exception("not object type");
        }
        $class_name = get_class($instance);
         // 添加到单例中
        if ( ! array_key_exists($class_name, self::$_singleton)) {
            self::$_singleton[$class_name] = $instance;
        }
    }
    //从单例中获取实例
    public static function getSingleton($class_name){
        return isset(self::$_singleton[$class_name]) ? self::$_singleton[$class_name] : [];
    }
    //销毁实例
    public static function delSingleton($class_name){
        unset(self::$_singleton[$class_name]);
    }
}
````
改造：

````
class Container{
    protected staic $_singleton = [];
    //增加一个实例到单例中
    public static function singleton($instance){
        if(!is_object($instance)){
            throw new \Exception("not object type");
        }
        $class_name = get_class($instance);
         // 添加到单例中
        if ( ! array_key_exists($class_name, self::$_singleton)) {
            self::$_singleton[$class_name] = $instance;
        }
    }
    //从单例中获取实例
    public static function getSingleton($class_name){
        return isset(self::$_singleton[$class_name]) ? self::$_singleton[$class_name] : [];
    }
    //销毁实例
    public static function delSingleton($class_name){
        unset(self::$_singleton[$class_name]);
    }
    /**
    *
    */
    public static function getInstance($class_name,$params = []){
        //获取反射实例
        $reflect = new ReflectionClass($class_name);
        //获取反射实例的构造方法
        $construct = $reflect->getConstruct();
        //获取反射实例构造方法的形参
        $di_params = [];
        if($construct){
            foreach($construct->getParams as $param){
                $class = $param->getClass();
                    // 如果参数是一个类，创建实例
                    if ($class) { 
                        //单例实例是否存在
                        $singleton = self::$getSingleton();
                        $di_params[] = $singleton ?? self::getInstance($class->name);
                    }
            } 
        }
        $di_params = array_merge($di_params,$param);
        //通过参数$di_params创建一个新的类实例。
        return $reflect->newInstanceArgs($di_params);
    }
}
````

### 以依赖注入的方式运行方法
由于调用的类和函数是未知的，这里利用回调调用函数及反射满足此功能
````
class Container{
    public static function run($class_name,$method_name,$params=[],$construct_params = []){
        if(!class_exists($class_name)){
            throw new \Exception("$class_name class not fund");
        }
        if(!method_exists($class_name,$method_name)){
            throw new \Exception("$method_name not fund in $class_name");
        }
        //获取实例
        $instance = self::getInstance($class_name,$construct_params); 
        // 获取反射实例
        $reflect = new ReflectionClass($class_name);
        // 获取方法
        $reflectMethod = $reflect->getMethod($method_name);
        //查找方法的参数
        $di_params = [];
        foreach($reflectMethod->getParameters as $param){
            $class = $param->getClass();
            if ($class) { 
                $singleton = self::getSingleton($class);
                $di_params[] = $singleton ?? self::getInstance($class->name);
            }
        }
        // 运行方法
        return call_user_func_array([$instance, $method_name], array_merge($di_params, $params));
    }
}
````
测试一下：
````
class A{
    protected $num = 10;
    public function getNum(){
        return $this->num;
    }
}
class B{
    protected $total;
    public function __construct(A $obj,$num){
        $this->total = $obj->getNum() + $num;
    }
    public function getTotal(){
        return $this->total;
    }
}

Container::run(B::class, 'getTotal' ,[10]);// result is 20
````
这里完整的代码可以转移[github](" https://github.com/DamonNie/IocContainer.git")


此处特感谢[@wazsmwazsm]("https://github.com/wazsmwazsm/IOCContainer/")、[@BNDong]("https://www.cnblogs.com/bndong/p/7988365.html")


