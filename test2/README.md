# 实验2：用户管理 - 掌握管理角色、权根、用户的能力，并在用户之间共享对象。

### 第1步：以system登录到pdborcl，创建角色fq和用户qf，并授权和分配空间：

```sql
$ sqlplus system/123@pdborcl
SQL> CREATE ROLE fq;
Role created.
SQL> GRANT connect,resource,CREATE VIEW TO fq;
Grant succeeded.
SQL> CREATE USER qf IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
User created.
SQL> ALTER USER qf QUOTA 50M ON users;
User altered.
SQL> GRANT fq TO qf;
Grant succeeded.
SQL> exit
