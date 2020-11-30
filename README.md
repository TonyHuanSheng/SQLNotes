# SQLNotes

### 參考網址:https://ithelp.ithome.com.tw/articles/10234683

SQL 主要分為三個類別。
DDL(Data Definition Languages):資料定義語言
DML(Data Manipulation Languages):資料操作語言
DCL(Data Control Languages):資料控制語言

### DDL(資料定義語言)

用來定義資料庫內部物件，可以對資料庫、表、列、索引進行建立、移除、修改等操作
，大部分是DBA(Database Administrator)資料庫管理員會使用到。

常用指令為: CREATE(建立)、DROP(移除)、ALTER(修改)

MySQL 
EX:建立資料表
```ruby
CREATE TABLE test(id    INT,
                  name  VARCHAR(20));
```

EX:移除資料表
```ruby
DROP TABLE test;
```

EX:資料表名稱修改
```ruby
ALTER TABLE test RENAME test2;
```

### DML(資料庫操作語言)

可以對資料表紀錄作新增、刪除、更新、查詢等操作，開發人員最常使用的。

常用指令: INSERT(新增)、DELETE(刪除)、UPDATA(更新)、SELECT(查詢)。

EX: 在資料表內新增紀錄
```ruby
INSERT INTO test VALUES (1,'Nini');
```

EX:將資料表內name為"Nini"紀錄刪除
```ruby
DELETE FROM test WHERE name= 'Nini' ;
```

EX:更新資料表內name為"Nini"紀錄的id
```ruby
UPDATA test SET id=2 WHERE name='Nini';
```

EX:查詢資料表
```ruby
SELECT * FROM test;
```

### DCL(資料控制語言)
用來控制不同資料庫、表、使用者的存取權限

常用指令:GRANT(給予權限)、REVOKE(收回權限)

EX:給於使用者「Andy」有查詢資料庫「test01」中的「test」資料表權限
```ruby
GRANT SELECT ON test01.test to 'andy';
```
EX: 收回使用者「Andy」對資料庫「test01」中「test」表的查詢權限
```ruby
REVOKE SELECT ON test01.test from 'andy';
```
