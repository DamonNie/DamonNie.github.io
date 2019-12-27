---
title: laravel实现Form表单场景验证
top: false
cover: false
date: 2019-12-27 12:59:54
password:
summary: 根据自己理解封装了一个类方法实现laravel的Form表单场景验证
tags:
 - laravel
 - 场景验证
categories: 后端
---

## 前言
Laravel没有Tp5那样的form表单验证场景，根据自己理解封装了一个类方法来实现他

## 认知了解
第一步：查找框架中有没有现成的工厂类可以用的，避免重复造轮子
demo：    
    protected function validator(array $data)
    {
        return Validator::make($data, [
            'name' => 'required|max:255',
            'email' => 'required|email|max:255|unique:users',
            'password' => 'required|min:6|confirmed',
        ]);
    }    
    控制器调用：$this->validator($request->all())->validate();

    分析：通过 Facades 的方式创建验证器对象，validate是最终验证方法;进一步深入类方法学习研究一下，
    `config/app.php` 中注册了`'Validator' => Illuminate\Support\Facades\Validator::class`。
    `Validator` 的实际实现类是容器中的 `validator` 对象
    ```php
    <?php
    namespace Illuminate\Support\Facades;

    /**
    * @see \Illuminate\Validation\Factory
    */
    class Validator extends Facade
    {
        /**
        * Get the registered name of the component.
        *
        * @return string
        */
        protected static function getFacadeAccessor()
        {
            return 'validator';
        }
    }
    ```
    创建验证器是通过实现 `\Illuminate\Contracts\Validation\Factory` 接口的 `\Illuminate\Validation\Factory` 类创建
    ```
    protected function resolve(array $data, array $rules, array $messages, array $customAttributes)
    {
        if (is_null($this->resolver)) {
            return new Validator(
                $this->translator, 
                $data,
                $rules, 
                $messages, 
                $customAttributes
            );
        }

        return call_user_func(
            $this->resolver, 
            $this->translator, 
            $data, 
            $rules, 
            $messages, 
            $customAttributes
        );
    }
    ```
    以上便是Validator类的实现逻辑，这里我们只需要知道如何使用即可： `Validator::make($data,$rule,$message)。`
## 实现

### 分析过程
>defined方法getInput： 获取验证数据
>defined方法getRules： 获取验证规则
>defined方法getMessage： 返回验证message,
在check方法中，验证场景表单
```
$validator = Validator::make($input, $rules,$messages);
//返回错误信息
if ($validator->fails()) {
    return $validator->errors();
}
return false;
```
### 具体类
```
use Illuminate\Support\Facades\Validator;
use Illuminate\Validation\Rules;

class AbstractValidate extends Validator
{
    /***
     * 验证字段属性
     *
     * @param $all
     * @param $rules
     * @param bool $message
     * @return array|bool|string
     */
    protected $input;
    protected $scenes = [];
    protected $messages = [];
    protected $rules = [];

    /**
     * 重写验证场景
     * @param $inputs
     * @param $scene
     * @return bool|string
     */
    public function check($inputs, $scene)
    {
        $input = $this->getInput($inputs, $scene);
        $rules = $this->getRules($scene);
        $messages = $this->getMessage($rules);
        $validator = Validator::make($input, $rules,$messages);
        //返回错误信息
        if ($validator->fails()) {
            return $validator->errors(); //返回错误信息
        }
        return false;
    }

    //获取验证数据
    public function getInput($inputs, $scene)
    {
        $input = [];
        if(!isset($this->scenes[$scene]) || !is_array($this->scenes[$scene])){
            return true;
        }
        foreach ($this->scenes[$scene] as $key => $v) {
            if (array_key_exists($v, $inputs)) {
                $input[$v] = $inputs[$v];
            }
        }
        return $input;
    }

    /**
     * 获取验证规则
     * @param $scene
     * @return mixed
     */
    public function getRules($scene)
    {
        $rules = [];
        if (isset($this->scenes[$scene]) && $this->scenes[$scene]) {
            foreach ($this->scenes[$scene] as $field) {
                if (array_key_exists($field, $this->rules)) {
                    $rules[$field] = $this->rules[$field];
                }
            }
        }
        return $rules;
    }


    /***
     * 返回验证message
     * @return array
     */
    public function getMessage($rules)
    {
        $message = [];
        foreach ($rules as $key => $v) {
            $arr = explode('|', $v);
            foreach ($arr as $k => $val) {
                if (strpos($val, ':') !== false) {
                    unset($arr[$k]);
                    $arr[] = substr($val, 0, strpos($val, ':'));
                }
            }
            foreach ($arr as $value) {
                if (array_key_exists($key . '.' . $value, $this->messages)) {
                    $message[$key . '.' . $value] = $this->messages[$key . '.' . $value];
                }
            }
        }
        return $message;
    }
}
```
*登录表单验证示例：*
```
class GuestValidate extends AbstractValidate
{
public $rules = [
        'name'                  => 'required',
        'phone'                 => 'required|regex:"^1\d{10}"',
        'email'                 => 'required|email',
        'password'              => 'required',
        'password_confirm' => 'required',
    ];

    public $messages = [
        'name.required'  => '用户名不能为空',
        'phone.required'  => '手机号不能为空',
        'phone.regex'     => '请输入正确的手机号',
        'email.required' => '请输入邮箱',
        'email.email' => '请输入正确的邮箱号',
        'password.email' => '请输入密码',
    ];

    public $scenes = [
        'login'  => ['email', 'password'],
        'register'  => ['email', 'password','password_confirm'],
    ];
}

Controller:(new GuestValidate())->check($request->all(),'login')
```
