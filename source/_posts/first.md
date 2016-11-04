---
title:  mysql在创建数据库时指定编码
---

在创建数据时，指定编码，能够很大程度上避免后续产生的乱码问题：下面是分别对gbk和utf-8进行设置
```
GBK: create database test2 DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;  
UTF8: CREATE DATABASE test2 DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci; 
```
# this is my first markdown file.2
## this is second title.
### this is third title.
#### this is fourth title.
##### this is fifth title.
###### this is sixth title.

*this text will be italic*<br>
_this text also be italic_<br>
**this text will be bold**<br>
__this text also be bold__<br>
~~this text will be delete~~<br>
_you **can** combine them_<br>

- first
- second
* third

1. first
2. second
3. third

-[x] FInsh my changes<br>
-[ ] Push my commits to GitHub<br>
-[ ] Open a pull request<br>

[github](http://github.com)<br>

`############`<br>

:smile:<br>
:grinning:<br>

```python
print "123";
```
标题|内容|备注
----|----|----
今天|很热|少穿
昨天|下雨|打伞
