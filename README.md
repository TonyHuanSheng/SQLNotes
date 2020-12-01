# SQLNotes

### 參考網址:https://ithelp.ithome.com.tw/articles/10234683
### 參考網址:https://kknews.cc/zh-tw/tech/ae53e8x.html
### 參考網址:https://www.mysql.tw/2013/03/sqlddldmldcltcl.html

SQL 主要分類個類別。

DDL(Data Definition Languages):資料定義語言

DQL(Data Query Languages):資料查詢語言

DML(Data Manipulation Languages):資料操作語言

TCL(Transaction Control Language):交易控制語言

DCL(Data Control Languages):資料控制語言


### DDL(資料定義語言)

用來定義資料庫內部物件，可以對資料庫、表、列、索引進行建立、移除、修改等操作
，大部分是DBA(Database Administrator)資料庫管理員會使用到。

常用指令為: CREATE(建立)、DROP(移除)、ALTER(修改)

MySQL 
EX:建立資料表
```Mysql
CREATE TABLE test(id    INT,
                  name  VARCHAR(20));
```

EX:移除資料表
```Mysql
DROP TABLE test;
```

EX:資料表名稱修改
```Mysql
ALTER TABLE test RENAME test2;
```

### DQL(資料查詢語言)
用於查詢數據

EX:查詢資料表
```Mysql
SELECT * FROM test;
```

### DML(資料庫操作語言)

可以對資料表紀錄作新增、刪除、更新等操作，開發人員最常使用的。

常用指令: INSERT(新增)、DELETE(刪除)、UPDATA(更新)。

EX: 在資料表內新增紀錄
```Mysql
INSERT INTO test VALUES (1,'Nini');
```

EX:將資料表內name為"Nini"紀錄刪除
```Mysql
DELETE FROM test WHERE name= 'Nini' ;
```

EX:更新資料表內name為"Nini"紀錄的id
```Mysql
UPDATA test SET id=2 WHERE name='Nini';
```
### TCL(交易控制語言)
* Transaction
* 事務:一個最小的不可再分的工作單位，通常一個事務對應一個完整的業務(例如銀行帳戶轉帳業務，該業務就是最小的工作單元
* 一個完整的業務需要批量的DML(insert、updata、delete)語句共同聯合完成
* 事務只和DML語句相關，或者說DML語句才有事務，這和業務邏輯有關，業務邏輯不同，DML語句個數不同
#### 參考網址:https://blog.wu-boy.com/2012/11/innodb-as-the-default-mysql-storage-engine/
* 只有InnDB儲存引擎支援交易管理，MySQL預設是AutoCommit，即自動更新資料庫

常用指令: START TRANSACTION(開始交易)、COMMIT(資料永久變更)、ROLLBACK(復原)、SAVEPOINT(設立儲存點)
#### 參考網址:https://dev.mysql.com/doc/refman/5.6/en/commit.html
#### 參考網址:https://www.mysql.tw/2018/05/mysqlrollbackcommit.html
外顯示交易 or 明確交易(Explicit Transaction)
* START TRANSACTION或 BEGIN開始新交易。
* COMMIT 提交當前事務，使其更改永久生效。
* ROLLBACK 回滾當前事務，取消其更改。
* SET autocommit 禁用或啟用當前會話的默認自動提交模式。
還沒ROLLBACK，紀錄不會回到資料表
```Mysql
START TRANSACTION;
INSERT INTO test VALUES(2,'andy');
INSERT INTO test VALUES(3,'Tyler');
SELECT * FROM test;
ROLLBACK;
SELECT * FROM test;
```
隱含式交易(Implicit Transaction)
* SET AUTOCOMMIT=1; 表示每次的指令自動確認 (不需要commit但是無法rollback)
* SET AUTOCOMMIT=0; 表示每次的指令不會自動確認 (需要commit但是可以rollback)

* 使用COMMIT or ROLLBACK結束當前交易，系統會自動啟動另一個新的交易，交易狀態會永遠是open狀態

```Mysql
SET AutoCommit=0;
INSERT INTO test VALUES(2,'andy');
INSERT INTO test VALUES(3,'Tyler');
SELECT * FROM test;
ROLLBACK;
SELECT * FROM test;
INSERT INTO test VALUES(4,'king');
INSERT INTO test VALUES(5,'iris');
SELECT * FROM test;
COMMIT;
SELECT * FROM test;
SET AutoCommit=1;
```
### DCL(資料控制語言)
用來控制不同資料庫、表、使用者的存取權限

常用指令:GRANT(給予權限)、REVOKE(收回權限)

EX:給於使用者「Andy」有查詢資料庫「test01」中的「test」資料表權限
```Mysql
GRANT SELECT ON test01.test to 'andy';
```
EX: 收回使用者「Andy」對資料庫「test01」中「test」表的查詢權限
```Mysql
REVOKE SELECT ON test01.test from 'andy';
```

常見的資料型態:數值類型、日期時間類型、字串類型

數值類型又分為整數類型、浮點數類型、定點數類型、位元類型

|整數類型|Byte|描述|
|--------|----|--------------|
|TINYINT|1|範圍-2^7 ~ 2^7-1 or unsigned:0 ~ 2^8-1|
|SMALLINT|2|範圍-2^15 ~ 2^15-1 or unsigned:0 ~ 2^16-1|
|MEDIUMINT|3|範圍-2^23 ~ 2^23-1 or unsigned: 0 ~ 2^24-1|
|INT、INTEGER|4|範圍-2^31 ~ 2^31-1 or unsigned: 0 ~ 2^32-1|
|BIGINT|8|範圍-2^63 ~ 2^63-1 or unsigned: 0 ~ 2^64-1|

整數屬性
+ Unsigned表示無法使用負號
+ ZEROFILL位數不足以0補足

|浮點數類型|Byte|描述|
|--------|----|--------------|
|FLOAT|4|範圍-3.40E+38 ~ 3.40E+38|
|DOUBLE|8|範圍-1.79E+308 ~ 1.79E+308|
|DECIMAL(M,D)、DEC(M,D)、NUMERIC||M+2 儲存完全精準的數值，D為小數點後幾位，整數位最多只能到(M-D)|
EX：Decimal(5,3) - 12.345(O) 123.34(X)
