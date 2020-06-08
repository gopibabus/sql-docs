# 🔥More about MySQL Database

### ⚡MyISAM vs InnoDB Storage Engines

A major factor in database performance is the storage engine used by the database, and more specifically, its tables. Different storage engines provide better performance in one situation over another. For general use, there are two contenders to be considered. These are **MyISAM**, which is the default MySQL storage engine, or **InnoDB**, which is an alternative engine built-in to MySQL intended for high-performance databases.

?> The main **difference** between MyISAM and INNODB are :

- MyISAM does not support transactions by tables while InnoDB supports.
- There are no possibility of row-level locking(MyISAM has table-level locking), relational integrity in MyISAM but with InnoDB this is possible.
- InnoDB does not support FULLTEXT index while MyISAM supports.
- Performance speed of MyISAM table is much higher as compared with tables in InnoDB.
- InnoDB is better option while you are dealing with larger database because it supports transactions, volume while MyISAM is suitable for small project.
- As InnoDB supports row-level locking which means inserting and updating is much faster as compared with MyISAM.
- InnoDB supports ACID (Atomicity, Consistency, Isolation and Durability) property while MyISAM does not support.
- In InnoDB table,AUTO_INCREMENT field is a part of index.
- Once table in InnoDB is deleted then it can not re-establish.
- InnoDB does not save data as table level so while implementation of select count(\*) from table will again scan the whole table to calculate the number of rows while MyISAM save data as table level so you can easily read out the saved row number.
- MyISAM does not support FOREIGN-KEY referential-integrity constraints while InnoDB supports.

### ✳Setting the Default Storage Engine

```bash
~ $ cat /etc/my.cnf

# Go to following Section in my.cnf file and change following value
[mysqld]
default-storage-engine=innodb
```
