ユーザリスト
:   `mysql> SELECT User FROM mysql.user;`

データベースリスト
:   `mysql> show databases;`

ユーザとデータベースの関係を調べる
:   `mysql> SELECT Db, User FROM mysql.db;`

データベースを削除
:   `mysql> DROP DATABASE IF EXISTS database_name;`

ユーザを削除
:   `mysql> DROP USER username@localhost;`
