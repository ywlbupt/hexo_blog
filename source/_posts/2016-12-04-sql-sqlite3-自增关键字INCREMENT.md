title: sql-sqlite3~自增关键字INCREMENT
date: 2016-12-04 10:13:48
category: [SQL, Sqlite3]
tags:
description:
---
[Toc]

1. [SQLite中的自增关键字：AUTO_INCREMENT、INTEGER PRIMARY KEY与AUTOINCREMENT](http://blog.csdn.net/zjy_hll/article/details/38319837)
2. [offical -sqlite - AUTOINCREMENT](http://www.sqlite.org/autoinc.html)


### Sqlite3不支持关键字AUTO_INCREMENT

### Sqlite3自增关键字INTEGER PRIMAY KEY

sqlite3 语句
``` sql
CREATE TABLE todo (
    id INTEGER PRIMARY KEY ,
    title TEXT
);
```
或者
``` sql
create table todo (
    id INTEGER PRIMARY KEY not null ,
    title TEXT
);
```

插入语句INSERT

1.  `INSERT INTO todo (title) VALUES ("shopping"); `
2.  `INSERT INTO todo (id, title) VALUES ("null", "shopping");`
3.  `INSERT INTO todo VALUES ("null", "shopping");`

### 关键字AUTOINCREMENT与内部表sqlite_sequence


SQLite中，在`INTEGER PRIMARY KEY`的基础上添加`AUTOINCREMENT`后（即INTEGER PRIMARY KEY AUTOINCREMENT），可以在表的整个生命周期内保证"自增字段"的唯一性（create keys that are unique over the lifetime of the table）。

SQLite内部用一个叫作`sqlite_sequence`的表来保存所有表的自增字段的取值基准（the largest ROWID），如果清空`sqlite_sequence`的记录(`clr("sqlite_sequence")`)，可以实现将所有表的自增字段的取值归零的效果（这种行为具有破坏性，请谨慎使用）。
