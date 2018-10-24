# 实验2：用户管理 - 掌握管理角色、权根、用户的能力，并在用户之间共享对象。

### 第1步：以system登录到pdborcl，创建角色fq和用户qf，并授权和分配空间：

```sql
$ sqlplus system/123@pdborcl
SQL> CREATE ROLE fq;
Role created.
SQL> GRANT connect,resource,CREATE VIEW TO fq;
Grant succeeded.
SQL> CREATE qf IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
User created.
SQL> ALTER USER qf QUOTA 50M ON users;
User altered.
SQL> GRANT fq TO qf;
Grant succeeded.
SQL> exit
```

### 第2步：新用户qf连接到pdborcl，创建表mytable和视图myview，插入数据，最后将myview的SELECT对象权限授予hr用户。

```sql
$ sqlplus qf/123@pdborcl
SQL> show user;
USER is "QF"
SQL> CREATE TABLE mytable (id number,name varchar(50));
Table created.
SQL> INSERT INTO mytable(id,name)VALUES(1,'zhang');
1 row created.
SQL> INSERT INTO mytable(id,name)VALUES (2,'wang');
1 row created.
SQL> CREATE VIEW myview AS SELECT name FROM mytable;
View created.
SQL> SELECT * FROM myview;
NAME
--------------------------------------------------
zhang
wang
SQL> GRANT SELECT ON myview TO hr;
Grant succeeded.
SQL>exit
```

### 第3步：用户hr连接到pdborcl，查询new_user授予它的视图myview

```sql
$ sqlplus hr/123@pdborcl
SQL> SELECT * FROM new_user.myview;
NAME
--------------------------------------------------
zhang
wang
SQL> exit
```

### 第4步：查看数据库的使用情况

```sql
$ sqlplus system/123@pdborcl
SQL>SELECT tablespace_name,FILE_NAME,BYTES/1024/1024 MB,MAXBYTES/1024/1024 MAX_MB,autoextensible FROM dba_data_files  WHERE  tablespace_name='USERS';
SQL>SELECT a.tablespace_name "表空间名",Total/1024/1024 "大小MB",
 free/1024/1024 "剩余MB",( total - free )/1024/1024 "使用MB",
 Round(( total - free )/ total,4)* 100 "使用率%"
 from (SELECT tablespace_name,Sum(bytes)free
        FROM   dba_free_space group  BY tablespace_name)a,
       (SELECT tablespace_name,Sum(bytes)total FROM dba_data_files
        group  BY tablespace_name)b
 where  a.tablespace_name = b.tablespace_name;
```

