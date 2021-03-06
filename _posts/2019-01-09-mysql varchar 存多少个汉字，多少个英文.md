---
layout: mypost
title: varchar 存多少个汉字，多少个英文
categories: [database, mysql]
---

#### 1. 关于UTF-8
**UTF-8**（Unicode Transformation Format-8bit）。是用以解决国际上字符的一种多字节编码。
它对英文使用8位（即一个字节) ，中文使用24位（三个字节）来编码。

#### 2. 关于GBK
GBK 是国家标准GB2312基础上扩容后兼容GB2312的标准。
GBK的文字编码是用双字节来表示的，即不论中、英文字符均使用双字节来表示，为了区分中文，将其最高位都设定成1。
GBK包含全部中文字符，是国家编码，通用性比UTF8差，不过UTF8占用的数据库比GBK大。

#### 3. 关于utf8mb4
MySql 5.5 之前，UTF8 编码只支持1-3个字节。从MySQL 5.5 开始，可支持4个字节UTF编码utf8mb4，一个字符最多能有4字节，所以能支持更多的字符集。
utf8mb4兼容utf8，且比utf8能表示更多的字符。
普通的常用的汉字用utf8mb4作为字符集时占3个字节，不常用的汉字占4个字节。
emoji表情，必须用utf8mb4，否则会导致插入数据库异常。
#### 4. 汉字长度与编码有关
MySql 5.0 以上的版本：
1. 一个汉字占多少长度与编码有关：
UTF-8：一个汉字占3个字节；不能存储不常用的汉字和emoji表情；英文是一个字节
utf8mb4：一个汉字占3个字节；能存储不常用的汉字和emoji表情，占4个字节；英文是一个字节
GBK： 一个汉字 = 2个字节，英文是一个字节
2. varchar(n) 表示n个字符，无论汉字和英文，MySql都能存入 n 个字符，仅实际字节长度有所区别。
3. MySQL检查长度，可用SQL语言
SELECT LENGTH(fieldname) FROM tablename

#### 5. 关于varchar 最多能存多少值
- mysql的记录行长度是有限制的，不是无限长的，这个长度是64K，即65535个字节，对所有的表都是一样的。
MySQL对于变长类型的字段会有1-2个字节来保存字符长度。
- 当字符数小于等于255时，MySQL只用1个字节来记录，因为2的8次方减1只能存到255。
- 当字符数多余255时，就得用2个字节来存长度了。
- 在utf-8状态下的varchar，最大只能到 (65535 - 2) / 3 = 21844 余 1。
- 在gbk状态下的varchar, 最大只能到 (65535 - 2) / 2 = 32766 余 1
