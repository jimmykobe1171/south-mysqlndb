south-mysqlndb
==============

South database adapter for mysqlndb

What does this package do ?
-------------------------
South is a project for django database migration(note that django 1.7 have its own migration method and do not need south anymore).

South implements mysql adapter and works well, but when it comes to mysql cluster, it will raise some exception if you add a column in table, which is described here http://dev.mysql.com/doc/refman/5.1/en/alter-table-online-operations.html. The exception looks like:

```
_mysql_exceptions.Warning: Converted FIXED field to DYNAMIC to enable on-line ADD COLUMN
```

Indeed, it is just a warning and the migration is done successfully. However, the origin mysql adapter does not catch this 'exception', consequently it raise exception and fail the whole migration process.

What's more, since mysqlndb does not support transaction, it can not roll back when migration fails. So, you will get an unsynced database...

**This package simply catch such warning and continue the migration process instead of failing it.**

Requiries
---------

Currently, I only test the package in below enviroment:

1. Django 1.6
2. South 1.0
3. MySQL Cluster 7.3.7

Installation
------------
Use pip to install:

```
pip install south_mysqlndb
``` 

Usage
-----
Set the `SOUTH_DATABASE_ADAPTERS` in django settings

```python
SOUTH_DATABASE_ADAPTERS = {
    'default': 'south_mysqlndb.mysqlndb'
}
```
