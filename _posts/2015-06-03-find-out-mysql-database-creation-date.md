---
layout: post
title: "Find out the creation date of a MySQL table"
excerpt: "If you want to find the creation time of a particular MySQL database you can use the following command."
categories: [database]
comments: true

---

If you want to find the creation time of a particular MySQL database you can use the following command.




```bash

SELECT create_time FROM INFORMATION_SCHEMA.TABLES WHERE table_schema = 'DB_NAME'
```

If you are interested in the creation date of a particular table simply add the appropriate where clause

```bash
SELECT create_time
FROM INFORMATION_SCHEMA.TABLES WHERE table_schema = 'DB_NAME'
AND table_name = 'TABLE_NAME'

+---------------------+
| create_time         |
+---------------------+
| 2015-06-03 08:15:06 |
+---------------------+
1 row in set (0.00 sec)
```

If you want to find the creation date of the original database rather than the tables within it you need to access the directory structure of MySQL. If you navigate to the specific database data directory
and check the timestamp of db.opt, this will be the original database creation date

```bash
ls -la db.opt
-rw-rw---- 1 27 sudo       65 Mar 30  2014 db.opt
```

