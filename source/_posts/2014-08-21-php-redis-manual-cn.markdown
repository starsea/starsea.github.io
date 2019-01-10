---
layout: post
title: "PHP Redis 中文手册增强版"
date: 2014-08-21 10:51:56 +0800
comments: true
categories: redis
---

本文是参考《redis中文手册》，将示例代码用php来实现，注意php-redis与redis_cli的区别（主要是返回值类型和参数用法）。

<!--more-->
返回值类型和参数用法一般来说有几个规律:

* redis-cli 返回 nil 的 php-redis 都是 `false`
* `hash` 里面的数据取出来的都是**字符串** 不管你client存放的是 int 还是其他类型
* `set` 存的数字都是 `double`
* `hmset` `hmget` `mset` `mget` 等 在 php-redis 中第二个参数都是数组 需要注意的是 lpush 不能用数组 只能一个一个 `$redis->lpush($key,$value)` 如果有需求可以考虑 `pipeline`

下面是各项API列表:


<table class="docutils" style="width: 90%;" border="1">
<colgroup>
	<col width="20%" />
    <col width="20%" />
    <col width="20%" />
    <col width="20%" />
    <col width="20%" />
</colgroup>
<thead align="center">
<tr class="row-odd">
	<th class="head">Key</th>
    <th class="head">String</th>
    <th class="head">Hash</th>
    <th class="head">List</th>
    <th class="head">Set</th>
</tr>
</thead>
<tbody valign="top">
<tr class="row-even">

<td>
<div style="padding: 0px; margin: 0px;">
<ul style="list-style-type:none;">
<li ><a class="reference internal" href="#key_del">DEL</a></li>
<li ><a class="reference internal" href="#key_keys">KEYS</a></li>
<li ><a class="reference internal" href="#key_RANDOMKEY">RANDOMKEY</a></li>
<li ><a class="reference internal" href="#key_TTL">TTL</a></li>
<li ><a class="reference internal" href="#key_EXISTS">EXISTS</a></li>
<li ><a class="reference internal" href="#key_move">MOVE</a></li>
<li ><a class="reference internal" href="#key_RENAME">RENAME</a></li>
<li ><a href="#key_RENAMENX">RENAMENX</a></li>
<li ><a class="reference internal" href="#key_TYPE">TYPE</a></li>
<li ><a class="reference internal" href="#key_EXPIRE">EXPIRE</a></li>
<li ><a href="#key_EXPIREAT">EXPIREAT</a></li>
<li ><a href="#key_OBJECT">OBJECT</a></li>
<li ><a href="#key_PERSIST">PERSIST</a></li>
<li ><a href="#key_SORT">SORT</a></li>
</ul>
</div>
</td>

<td>
<div style="padding: 0px; margin: 0px;">


<ul style="list-style-type:none;">
<li ><a href="#string_SET">SET</a></li>
<li ><a href="#string_SETNX">SETNX</a></li>
<li ><a href="#string_SETEX">SETEX</a></li>
<li ><a href="#string_SETRANGE">SETRANGE</a></li>
<li ><a href="#string_MSET">MSET</a></li>
<li ><a href="#string_MSETNX">MSETNX</a></li>
<li ><a href="#string_APPEND">APPEND</a></li>
<li ><a href="#string_GET">GET</a></li>
<li ><a href="#string_MGET">MGET</a></li>
<li ><a href="#string_GETRANGE">GETRANGE</a></li>
<li ><a href="#string_GETSET">GETSET</a></li>
<li ><a href="#string_STRLEN">STRLEN</a></li>
<li ><a href="#string_INCR">INCR</a></li>
<li ><a href="#string_INCRBY">INCRBY</a></li>
<li ><a href="#string_DECR">DECR</a></li>
<li ><a href="#string_DECRBY">DECRBY</a></li>
<li ><a href="#string_SETBIT">SETBIT</a></li>
<li ><a href="#string_GETBIT">GETBIT</a></li>
</ul>

</div>
</td>
<td>
<div style="padding: 0px; margin: 0px;">
<ul style="list-style-type:none;">
<li ><a href="#hash_HSET">HSET</a></li>
<li ><a href="#hash_HSETNX">HSETNX</a></li>
<li ><a href="#hash_HMSET">HMSET</a></li>
<li ><a href="#hash_HGET">HGET</a></li>
<li ><a href="#hash_HMGET">HMGET</a></li>
<li ><a href="#hash_HGETALL">HGETALL</a></li>
<li ><a href="#hash_HDEL">HDEL</a></li>
<li ><a href="#hash_HLEN">HLEN</a></li>
<li ><a href="#hash_HEXISTS">HEXISTS</a></li>
<li ><a href="#hash_HINCRBY">HINCRBY</a></li>
<li ><a href="#hash_HKEYS">HKEYS</a></li>
<li ><a href="#hash_HVALS">HVALS</a></li>
</ul>
</div>
</td>
<td>
<div style="padding: 0px; margin: 0px;">
<ul style="list-style-type:none;">
<li ><a href="#list_LPUSH">LPUSH</a></li>
<li ><a href="#list_LPUSHX">LPUSHX</a></li>
<li ><a href="#list_RPUSH">RPUSH</a></li>
<li ><a href="#list_RPUSHX">RPUSHX</a></li>
<li ><a href="#list_LPOP">LPOP</a></li>
<li ><a href="#list_RPOP">RPOP</a></li>
<li ><a href="#list_BLPOP">BLPOP</a></li>
<li ><a href="#list_BRPOP">BRPOP</a></li>
<li ><a href="#list_LLEN">LLEN</a></li>
<li ><a href="#list_LRANGE">LRANGE</a></li>
<li ><a href="#list_LREM">LREM</a></li>
<li ><a href="#list_LSET">LSET</a></li>
<li ><a href="#list_LTRIM">LTRIM</a></li>
<li ><a href="#list_LINDEX">LINDEX</a></li>
<li ><a href="#list_LINSERT">LINSERT</a></li>
<li ><a href="#list_RPOPLPUSH">RPOPLPUSH</a></li>
<li ><a href="#list_BRPOPLPUSH">BRPOPLPUSH</a></li>
</ul>
</div>
</td>
<td>
<div style="padding: 0px; margin: 0px;">
<ul style="list-style-type:none;">
<li ><a href="#set_SADD">SADD</a></li>
<li ><a href="#set_SREM">SREM</a></li>
<li ><a href="#set_SMEMBERS">SMEMBERS</a></li>
<li ><a href="#set_SISMEMBER">SISMEMBER</a></li>
<li ><a href="#set_SCARD">SCARD</a></li>
<li ><a href="#set_SMOVE">SMOVE</a></li>
<li ><a href="#set_SPOP">SPOP</a></li>
<li ><a href="#set_SRANDMEMBER">SRANDMEMBER</a></li>
<li ><a href="#set_SINTER">SINTER</a></li>
<li ><a href="#set_SINTERSTORE">SINTERSTORE</a></li>
<li ><a href="#set_SUNION">SUNION</a></li>
<li ><a href="#set_SUNIONSTORE">SUNIONSTORE</a></li>
<li ><a href="#set_SDIFF">SDIFF</a></li>
<li ><a href="#set_SDIFFSTORE">SDIFFSTORE</a></li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


<h2><a name="hash_HSET">HSET</a></h2>

HSET key field value

将哈希表key中的域field的值设为value。

如果key不存在，一个新的哈希表被创建并进行HSET操作。

如果域field已经存在于哈希表中，旧值将被覆盖。

**时间复杂度**：

* O(1)

**返回值：**

- 如果field是哈希表中的一个新建域，并且值设置成功，返回1。
- 如果哈希表中域field已经存在且旧值已被新值覆盖，返回0。

```php
$a = array(
    'key' => 'myhash',
    'field' => 'field1',
    'value' => 'field1 value'
);
var_dump($redis->hSet($input));
```