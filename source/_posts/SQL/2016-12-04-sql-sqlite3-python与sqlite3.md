title: sql-sqlite3~python与sqlite3
date: 2016-12-04 10:32:33
category: [SQL, Sqlite3]
tags:
    - python
description:
---
[Toc]

参考
1. [python_library_reference sqlite3](https://docs.python.org/3.5/library/sqlite3.html)

2. [Syntax Diagrams For SQLite](http://www.sqlite.org/syntaxdiagrams.html)

### 在内存中操作测试sqlite3

``` python
import sqlite3
conn = sqlite3.connect(":memory:")

conn.close()
```
