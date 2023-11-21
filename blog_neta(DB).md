
# MySQL(Host追加する方法)
- CREATE USER park@localhost IDENTIFIED BY '1234';
- GRANT ALL ON db1.* TO park@localhost;

### Mysql rootでログイン
(base)  ✘ park@sv1  ~  mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 5.7.42 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

### Mysql User作成
mysql> CREATE USER park@localhost IDENTIFIED BY '1234';
Query OK, 0 rows affected (0.00 sec)

mysql> GRANT ALL ON db1.* TO park@localhost
    -> ;
Query OK, 0 rows affected (0.00 sec)

mysql> exit
Bye

### 作成したUserでログイン
(base)  park@sv1  ~  mysql -u park -p1234
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 5.7.42 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status<br>
\--------------<br>
mysql  Ver 14.14 Distrib 5.7.24, for Linux (x86_64) using  EditLine wrapper <br>

Connection id:		9 <br>
Current database:	 <br>
Current user:		park@localhost <br>
SSL:			Not in use <br>
Current pager:		less <br>
Using outfile:		''  <br>
Using delimiter:	; <br>
Server version:		5.7.42 MySQL Community Server (GPL) <br>
Protocol version:	10 <br>
Connection:		Localhost via UNIX socket <br>
Server characterset:	latin1 <br>
Db     characterset:	latin1 <br>
Client characterset:	utf8 <br>
Conn.  characterset:	utf8 <br>
UNIX socket:		/var/run/mysqld/mysqld.sock <br>
Uptime:			23 min 52 sec <br>

Threads: 2  Questions: 21  Slow queries: 0  Opens: 113  Flush tables: 1  Open tables:  106  Queries per second avg: 0.014 <br>
\-----------

# Mysql データベース作成関連
- CREATE DATABASE データベース名; (CREATE DATABASEのところは小文字でもいいし、大文字でも大丈夫)
- SHOW DATABASES; (データベースの情報を確認することができる。)
- use データベース名 (データベースを指定することができる。)
- SELECT DATABASE(); (現在使用しているデータベースの表示)
- CREATE TABLE テーブル名 (カラム名1 データ型1, カラム名2 データ型2);
- SHOW TABLES; (データベースに存在するテーブルを閲覧することができる。)

### データベース作成
mysql> CREATE DATABASE db1; <br>
Query OK, 1 row affected (0.00 sec)

### データベースの情報確認
mysql> SHOW DATABASES; <br>
+--------------------+ <br>
| Database           | <br>
+--------------------+ <br>
| information_schema | <br>
| db1                | <br>
+--------------------+ <br>
2 rows in set (0.00 sec)


### データベース指定
mysql> use db1 <br>
Database changed

### 使用しているデータベースの表示
mysql> SELECT DATABASE(); <br>
+------------+ <br>
| DATABASE() | <br>
+------------+ <br>
| db1        | <br>
+------------+ <br>
1 row in set (0.00 sec)

### db1にテーブルtb1を作成
- テーブルを構成する項目は **「フィールド」**、実際に入力されたレコードを構成する項目のデータを **「カラム」** という。
- カラムに補完するデータの種類をデータ型と言う。また、**INT**は1、2、3のような整数のデータを保管できることを意味する。**VARCHAR**は、文字データを保管することである。

mysql> CREATE TABLE tb1(bang VARCHAR(10),name VARCHAR(1),tosi INT); <br>
Query OK, 0 rows affected (0.02 sec)

### すべてのテーブルを表示する
mysql> SHOW TABLES; <br>
+---------------+ <br>
| Tables_in_db1 | <br>
+---------------+ <br>
| tb1           | <br>
+---------------+ <br>
1 row in set (0.00 sec)

- MySQLでテーブルに文字を入れたら、文字化けを起こすことがある。この場合には、文字エンコーディングを指定してテーブルを作成すると解決することができる。<br>
mysql> CREATE TABLE tb2(bang VARCHAR(10),name VARCHAR(1),tosi INT) <br>
    -> CHARSET=utf8; <br>
Query OK, 0 rows affected (0.03 sec)
- 他のデータベースへのアクセスするためには、次のように入力すれば大丈夫である。(データベースdb2のテーブルtableを参照したい時) <br>
SELECT * FROM db2.table; <br>
つまり、「データベース名.テーブル名」のように感じである。

# テーブルのカラム構造を確認する方法
- DESC テーブル名; (テーブルのカラム構造を表示するコマンドである。)

### tb1のカラム構造を確認する
mysql> DESC tb1; <br>
+-------+-------------+------+-----+---------+-------+ <br>
| Field | Type        | Null | Key | Default | Extra | <br>
+-------+-------------+------+-----+---------+-------+ <br>
| bang  | varchar(10) | YES  |     | NULL    |       | <br>
| name  | varchar(1)  | YES  |     | NULL    |       | <br>
| tosi  | int(11)     | YES  |     | NULL    |       | <br>
+-------+-------------+------+-----+---------+-------+ <br>
3 rows in set (0.01 sec)

# テーブルにデータを挿入する
- INSERT INTO テーブル名 VALUES(データ1,データ2);
- INSERT INTO テーブル名 (カラム名1,カラム名2) VALUES(データ1,データ2);
- INSERT INTO テーブル名 (カラム名1,カラム名2) VALUES(データ1,データ2),(データ1,データ2)....;(複数で宣言することができる。)

# データを表示する
- SELECT カラム名1,カラム2 FROM テーブル名;
- SELECT * FROM テーブル名; (該当するテーブルのすべてのレコードを表示することができる。)

# テーブルをコピーする方法
- CREATE TABLE テーブル名 SELECT * FROM コピーしたいテーブル名;

# データベースのデータ型
### 数値型の種類
|データ型|意味|
|--------|----|
|INT|右の範囲の整数|
|TINYINT|とても小さな整数|
|SMALLINT|小さな整数|
|MEDIUMINT|中くらいの整数|
|BIGINT|大きい整数|
|FLOAT|単精度浮動小数点数|
|DOUBLE|倍精度浮動小数点数|
|DECIMAL|固定小数点数|

### 文字列の種類
|データ型|意味|
|--------|----|
|CHAR|固定長の文字列|
|VARCHAR|可変長の文字列|
|TEXT|長い文字列|
|LONGTEXT|とても長い文字列|

### 日付・時刻型の種類
|データ型|意味|
|--------|----|
|DATETIME|日付と時刻|
|DATE|日付|
|YEAR|年|
|TIME|時刻|

#### プロンプトの文字列を変更する
- prompt プロンプトとして表示するもの


# テーブルを削除する
- drop table テーブル名;
### 例題
mysql> DESC tb3; <br>
+-------+-------------+------+-----+---------+-------+ <br>
| Field | Type        | Null | Key | Default | Extra | <br>
+-------+-------------+------+-----+---------+-------+ <br>
| ban   | varchar(10) | YES  |     | NULL    |       | <br>
| name  | varchar(10) | YES  |     | NULL    |       | <br>
| tosi  | int(11)     | YES  |     | NULL    |       | <br>
+-------+-------------+------+-----+---------+-------+ <br>
3 rows in set (0.00 sec) <br>

mysql> drop table tb1; <br>
Query OK, 0 rows affected (0.02 sec) <br>

mysql> drop table tb2; <br>
Query OK, 0 rows affected (0.01 sec) <br>


mysql> SHOW TABLES; <br>
+---------------+ <br>
| Tables_in_db1 | <br>
+---------------+ <br>
| tb3           | <br>
+---------------+ <br>
1 row in set (0.00 sec) 
