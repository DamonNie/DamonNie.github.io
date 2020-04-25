---
title: PHP实现一个hash表-拉链法解决hahs冲突
top: false
cover: false
date: 2020-04-26 00:09:13
password:
summary:
tags:
 - PHP
 - 代码审计
categories: 代码审计
---

> 拜读了一篇文章[通过构造Hash冲突实现各种语言的拒绝服务攻击](https://www.laruence.com/2011/12/29/2412.html)，甚是很有兴趣。于是，就解决方案上从网上搜寻了答案，还是很有启发意义的 于是就问题和解决方案思路抄录下来


文中阐述，包括php<=5.3.8版本引入新的配置项
`添加了max_input_vars指令以防止基于哈希冲突的攻击`
这个预防的攻击，就是“通过调用哈希冲突实现各种语言的拒绝服务攻击漏洞”（通过哈希算法冲突实现多种拒绝服务）。
这个预防的攻击，就是“通过调用哈希冲突实现各种语言的拒绝服务攻击漏洞”（通过哈希算法冲突实现多种拒绝服务）。**攻击的原理很简单，目前很多语言，使用哈希来存储kv数据，包括常用的来自用户的POST数据，攻击者可以通过构造请求头，并伴随POST大量的特殊的“ k”值（根据每种语言的哈希算法不同而定制），使用语言来保存POST数据的Hash表因为“冲突”而退化成链表。这样一来，如果数据量<b>足够大**，那么就可以通过语言在计算，查找，插入的时候，造成大量的CPU占用，从而实现拒绝服务攻击。PHP5.4是通过增加一个限制来尽量避免被此类攻击影响：**[![](/medias/2011/12/Untitled3-1.png "哈希冲突")](/medias/2011/12/Untitled3-1.png "")
<b>****```html
-max_input_vars-指定可能有多少个GET / POST / COOKIE输入变量
公认。默认值1000。

> 以下另一种解决思路：post过来的数据都会被缓冲到bucket，只要在bucket流中避免相同hash在同一链表中，这里用拉链法解决
### talk is cheap，show me code

````php
<?php
header('Content-type:text/html;charset=utf-8');

class HashTable
{
    private $buckets;
    private $size = 10;

    public function __construct()
    {
        $this->buckets = new SplFixedArray($this->size);
    }
    private function hash_func($key)
    {
        $strlen   = strlen($key);
        $hash_val = 0;
        for ($i = 0 ; $i < $strlen ; $i++) {
            $hash_val += ord($key[$i]);
        }
        return $hash_val % $this->size;
    }
    public function insert($key, $val)
    {
        $index = $this->hash_func($key);
        if (isset($this->buckets[$index])) {
            $new_code = new HashNode($key, $val, $this->buckets[$index]);
        } else {
            $new_code = new HashNode($key, $val, null);
        }
        $this->buckets[$index] = $new_code;
    }
    public function find($key)
    {
        $index   = $this->hash_func($key);
        $current = $this->buckets[$index];
        while (isset($current)) {
            if ($current->key == $key) {
                return $current->value;
            }
            $current = $current->next_node;
        }
        return null;
    }
    public function responseBuckets()
    {
        return $this->buckets;
    }
}

class HashNode
{
    public $key;
    public $value;
    public $next_node;

    public function __construct($key, $value, $next_node = null)
    {
        $this->key       = $key;
        $this->value     = $value;
        $this->next_node = $next_node;
    }
}

$hash = new HashTable();
$hash->insert('value1', 'A');
$hash->insert('value12', 'B');

echo $hash->find('value1');
echo $hash->find('value12');
print_r($hash->responseBuckets());
````