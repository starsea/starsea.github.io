---
layout: post
title: "redis-hash-table-vs-json-encode"
date: 2014-08-17 21:23:35 +0800
comments: true
categories: redis
---

redis 支持多种数据结构  有时候我们在做项目的时候可能会考虑到底是用 hash 还是用普同的 k-v 结构(存储 json)

这里我们排除哪种结构对于程序来说更好 单从速度上来比较下
<!--more-->
```php
<?php
/**
 *
 * @desc 用来比较 redis hash  和 json_decode 的代价
 */

function microtime_float()
{
    list($usec, $sec) = explode(" ", microtime());
    return ((float)$usec + (float)$sec);
}

$redis = new Redis();
$redis->connect('127.0.0.1', 6379);
$raw = array(
    'uid'        => 1,
    'nickName'   => 2,
    'role'       => 3,
    'joinTime'   => 4,
    'score'      => 0,
    'uploadTime' => 0,
    'award'      => 'zzzzzzzzz'
);

var_dump($redis->get('test'));

$t5 = microtime_float();
for ($i = 0; $i < 100000; $i++) {
    $redis->hMset('t' . $i, $raw);
}

$t6 = microtime_float();

for ($i = 0; $i < 100000; $i++) {
    $redis->set('t' . $i . '_json', json_encode($raw));
}

$t7 = microtime_float();



$t1 = microtime_float();
for ($i = 0; $i < 100000; $i++) {
    $redis->hGetAll('t' . $i);
}
$t2 = microtime_float();


for ($i = 0; $i < 100000; $i++) {
    json_decode($redis->get('t' . $i . '_json'), true);

}
$t3 = microtime_float();


echo 'sethash ' . ($t6 - $t5) . PHP_EOL;
echo 'setjson ' . ($t7 - $t6) . PHP_EOL;

echo 'gethash ' . ($t2 - $t1) . PHP_EOL;

echo 'getjson ' . ($t3 - $t2) . PHP_EOL;

```
查看下执行结果
```bash
[haidx@mbp:/usr/local/var/www/test]$ php json.php
bool(false)
sethash 9.2558209896088
setjson 7.8669381141663
gethash 7.9401888847351
getjson 7.5358710289001
You have new mail in /var/mail/haidx
[haidx@mbp:/usr/local/var/www/test]$ redis-memory-for-key t36275
Key				"t36275"
Bytes				170
Type				hash
Encoding			ziplist
Number of Elements		7
Length of Largest Element	10
[haidx@mbp:/usr/local/var/www/test]$ php json.php
bool(false)
sethash 8.133064031601
setjson 8.2587089538574
gethash 7.9228899478912
getjson 7.515007019043
```
第一次set 的时候redis db 是空的. 没多大区别

下面我们用真实的 hash table 测试一下
```php
$raw = array(
    'uid'        => 1,
    'nickName'   => 2,
    'role'       => 3,
    'joinTime'   => 4,
    'score'      => 0,
    'uploadTime' => 0,
    'award'      => 'zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz'
);
```

```bash
[haidx@mbp:/usr/local/var/www/test]$ php json.php
bool(false)
sethash 7.7665259838104
setjson 7.3227639198303
gethash 8.2078440189362
getjson 7.5661609172821
[haidx@mbp:/usr/local/var/www/test]$ php json.php
bool(false)
sethash 8.0299470424652
setjson 7.6386320590973
gethash 8.1137571334839
getjson 7.9209759235382
[haidx@mbp:/usr/local/var/www/test]$ php json.php
bool(false)
sethash 8.6087439060211
setjson 7.4942181110382
gethash 7.9777708053589
getjson 7.8165891170502
[haidx@mbp:/usr/local/var/www/test]$ php json.php
bool(false)
sethash 8.1856729984283
setjson 8.3040089607239
gethash 8.8928000926971
getjson 8.1433188915253
[haidx@mbp:/usr/local/var/www/test]$ !re
redis-memory-for-key t36275
Key				"t36275"
Bytes				925.0
Type				hash
Encoding			hashtable
Number of Elements		7
Length of Largest Element	91

[haidx@mbp:/usr/local/var/www/test]$ redis-memory-for-key t36275_json
Key				"t36275_json"
Bytes				272
Type				string
```
看来从 get set 上的速度来看真的没什么差别.json占的字节少一点. 所以想用什么还是根据需求吧.

