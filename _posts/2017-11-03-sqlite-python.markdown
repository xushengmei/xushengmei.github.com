---
layout: post
author: 徐生梅
title: "Python 使用 sqlite3 操作使用 SQLite 数据库 "
catalog: true
tags: python sqlite
---
SQLite 作为一个轻量数据库，操作非常简单，python 也提供了相应的操作模块 sqlite3，下面介绍一下具体的操作过程

## 操作步骤：
* 导入 sqlite3 模块；
* 调用 connect() 创建数据库连接，返回连接对象 conn;
* 调用 conn.execute() 方法创建表结构。如果设置了手动提交，需要嗲用conn.commit() 提交插入或修改的数据。
* 调用 conn.cursor() 方法返回游标，通过 cur.execute() 方法查询数据库；
* 调用 cur.fetchall()、curfetchmany() 或 cur.fetchone()返回查询结果；
* 关闭 cur 和 conn.

## 代码实现
```python
import  sqlite3
#连接数据库
conn = sqlite3.connect(r"addresses.db")
#创建表
conn.execute("create TABLE if not EXISTS address(id integer PRIMARY key AUTOINCREMENT ,name varchar(128),address varchar(128))")
#插入数据
conn.execute("insert into address(name,address) VALUES ('Tom','Beijing road')")
conn.execute("insert into address(name,address) VALUES ('Jerry','Shanghai road')")
#手动提交数据
conn.commit()
#获取游标对象
cur = conn.cursor()
#使用游标查询数据
cur.execute("SELECT * FROM address")
#获取所有结果
res = cur.fetchall()
print("address:",res)
for line in res:
    for f in line:
        print(f,)
    print()
#关闭连接
cur.close()
conn.close()
```