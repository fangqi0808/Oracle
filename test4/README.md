# 实验四 对象管理
### 登录系统用户更改用户名
```sql
ALTER USER STUDY QUOTA UNLIMITED ON USERS;
ALTER USER STUDY QUOTA UNLIMITED ON USERS02;
ALTER USER STUDY ACCOUNT UNLOCK;
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/23.png)
### 对用户授权
```sql
GRANT "CONNECT" TO STUDY WITH ADMIN OPTION;
GRANT "RESOURCE" TO STUDY WITH ADMIN OPTION;
ALTER USER STUDY DEFAULT ROLE "CONNECT","RESOURCE";
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/24.png)
```sql
declare num number;
begin
      select count(1) into num from user_tables where TABLE_NAME = 'DEPARTMENTS';
      if num=1 then execute immediate 'drop table DEPARTMENTS cascade constraints PURGE';
      end if;
      select count(1) into num from user_tables where TABLE_NAME = 'EMPLOYEES';
      if num=1 then execute immediate 'drop table EMPLOYEES cascade constraints PURGE';
      end if;
      select count(1) into num from user_tables where TABLE_NAME = 'ORDER_ID_TEMP';
      if num=1 then execute immediate 'drop table ORDER_ID_TEMP cascade constraints PURGE';
      end if;
      select count(1) into num from user_tables where TABLE_NAME = 'ORDER_DETAILS';
      if num=1 then execute immediate 'drop table ORDER_DETAILS cascade constraints PURGE';
      end if;
      select count(1) into num from user_tables where TABLE_NAME = 'ORDERS';
      if num=1 then execute immediate 'drop table ORDERS cascade constraints PURGE';
      end if;
      select count(1) into num from user_tables where TABLE_NAME = 'PRODUCTS';
      if num=1 then execute immediate 'drop table PRODUCTS cascade constraints PURGE';
      end if;
      select count(1) into num from user_sequences where SEQUENCE_NAME = 'SEQ_ORDER_DETAILS_ID';
      if num=1 then execute immediate 'drop  SEQUENCE SEQ_ORDER_DETAILS_ID';
      end if;
      select count(1) into num from user_sequences where SEQUENCE_NAME = 'SEQ_ORDER_ID';
      if num=1 then execute immediate 'drop  SEQUENCE SEQ_ORDER_ID';
      end if;
      select count(1) into num from user_views where VIEW_NAME = 'VIEW_ORDER_DETAILS';
      if num=1 then execute immediate 'drop VIEW VIEW_ORDER_DETAILS';
      end if;
      SELECT count(object_name)  into num FROM user_objects_ae WHERE object_type = 'PACKAGE' and OBJECT_NAME='MYPACK';
      if num=1 then execute immediate 'DROP PACKAGE MYPACK';
      end if;
end;
/
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/1.png)
### 创建表DEPARTMENTS
```sql
CREATE TABLE DEPARTMENTS
(
  DEPARTMENT_ID NUMBER(6, 0) NOT NULL
, DEPARTMENT_NAME VARCHAR2(40 BYTE) NOT NULL
, CONSTRAINT DEPARTMENTS_PK PRIMARY KEY
  (
    DEPARTMENT_ID
  )
  USING INDEX
  (
      CREATE UNIQUE INDEX DEPARTMENTS_PK ON DEPARTMENTS (DEPARTMENT_ID ASC)
      NOLOGGING
      TABLESPACE USERS
      PCTFREE 10
      INITRANS 2
      STORAGE
      (
        INITIAL 65536
        NEXT 1048576
        MINEXTENTS 1
        MAXEXTENTS UNLIMITED
        BUFFER_POOL DEFAULT
      )
      NOPARALLEL
  )
  ENABLE
)
NOLOGGING
TABLESPACE USERS
PCTFREE 10
INITRANS 1
STORAGE
(
  INITIAL 65536
  NEXT 1048576
  MINEXTENTS 1
  MAXEXTENTS UNLIMITED
  BUFFER_POOL DEFAULT
)
NOCOMPRESS NO INMEMORY NOPARALLEL;
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/2.png)
### 创建表EMPLOYEES
```sql
CREATE TABLE EMPLOYEES
(
  EMPLOYEE_ID NUMBER(6, 0) NOT NULL
, NAME VARCHAR2(40 BYTE) NOT NULL
, EMAIL VARCHAR2(40 BYTE)
, PHONE_NUMBER VARCHAR2(40 BYTE)
, HIRE_DATE DATE NOT NULL
, SALARY NUMBER(8, 2)
, MANAGER_ID NUMBER(6, 0)
, DEPARTMENT_ID NUMBER(6, 0)
, PHOTO BLOB
, CONSTRAINT EMPLOYEES_PK PRIMARY KEY
  (
    EMPLOYEE_ID
  )
  USING INDEX
  (
      CREATE UNIQUE INDEX EMPLOYEES_PK ON EMPLOYEES (EMPLOYEE_ID ASC)
      NOLOGGING
      TABLESPACE USERS
      PCTFREE 10
      INITRANS 2
      STORAGE
      (
        INITIAL 65536
        NEXT 1048576
        MINEXTENTS 1
        MAXEXTENTS UNLIMITED
        BUFFER_POOL DEFAULT
      )
      NOPARALLEL
  )
  ENABLE
)
NOLOGGING
TABLESPACE USERS
PCTFREE 10
INITRANS 1
STORAGE
(
  INITIAL 65536
  NEXT 1048576
  MINEXTENTS 1
  MAXEXTENTS UNLIMITED
  BUFFER_POOL DEFAULT
)
NOCOMPRESS
NO INMEMORY
NOPARALLEL
LOB (PHOTO) STORE AS SYS_LOB0000092017C00009$$
(
  ENABLE STORAGE IN ROW
  CHUNK 8192
  NOCACHE
  NOLOGGING
  TABLESPACE USERS
  STORAGE
  (
    INITIAL 106496
    NEXT 1048576
    MINEXTENTS 1
    MAXEXTENTS UNLIMITED
    BUFFER_POOL DEFAULT
  )
);
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/3.png)
### 创建索引EMPLOYEES_INDEX1_NAME
```sql
CREATE INDEX EMPLOYEES_INDEX1_NAME ON EMPLOYEES (NAME ASC)
NOLOGGING
TABLESPACE USERS
PCTFREE 10
INITRANS 2
STORAGE
(
  INITIAL 65536
  NEXT 1048576
  MINEXTENTS 1
  MAXEXTENTS UNLIMITED
  BUFFER_POOL DEFAULT
)
NOPARALLEL;
ALTER TABLE EMPLOYEES
ADD CONSTRAINT EMPLOYEES_FK1 FOREIGN KEY
(
  DEPARTMENT_ID
)
REFERENCES DEPARTMENTS
(
  DEPARTMENT_ID
)
ENABLE;
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/4.png)
### 创建外键<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/5.png)<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/7.png)<br>
### 创建表PRODUCTS<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/6.png)<br>
### 创建表ORDER_ID_TEMP<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/8.png)<br>
### 创建表ORDERS
```sql
CREATE TABLE ORDERS
(
  ORDER_ID NUMBER(10, 0) NOT NULL
, CUSTOMER_NAME VARCHAR2(40 BYTE) NOT NULL
, CUSTOMER_TEL VARCHAR2(40 BYTE) NOT NULL
, ORDER_DATE DATE NOT NULL
, EMPLOYEE_ID NUMBER(6, 0) NOT NULL
, DISCOUNT NUMBER(8, 2) DEFAULT 0
, TRADE_RECEIVABLE NUMBER(8, 2) DEFAULT 0
)
TABLESPACE USERS
PCTFREE 10
INITRANS 1
STORAGE
(
  BUFFER_POOL DEFAULT
)
NOCOMPRESS
NOPARALLEL
PARTITION BY RANGE (ORDER_DATE)
(
  PARTITION PARTITION_BEFORE_2016 VALUES LESS THAN (TO_DATE(' 2016-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
  NOLOGGING
  TABLESPACE USERS
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    INITIAL 8388608
    NEXT 1048576
    MINEXTENTS 1
    MAXEXTENTS UNLIMITED
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY
, PARTITION PARTITION_BEFORE_2017 VALUES LESS THAN (TO_DATE(' 2017-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
  NOLOGGING
  TABLESPACE USERS02
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    INITIAL 8388608
    NEXT 1048576
    MINEXTENTS 1
    MAXEXTENTS UNLIMITED
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY
);
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/9.png)
### 创建本地分区索引ORDERS_INDEX_DATE：
```sql
CREATE INDEX ORDERS_INDEX_DATE ON ORDERS (ORDER_DATE ASC)
LOCAL
(
  PARTITION PARTITION_BEFORE_2016
    TABLESPACE USERS
    PCTFREE 10
    INITRANS 2
    STORAGE
    (
      INITIAL 8388608
      NEXT 1048576
      MINEXTENTS 1
      MAXEXTENTS UNLIMITED
      BUFFER_POOL DEFAULT
    )
    NOCOMPRESS
, PARTITION PARTITION_BEFORE_2017
    TABLESPACE USERS02
    PCTFREE 10
    INITRANS 2
    STORAGE
    (
      INITIAL 8388608
      NEXT 1048576
      MINEXTENTS 1
      MAXEXTENTS UNLIMITED
      BUFFER_POOL DEFAULT
    )
    NOCOMPRESS
)
STORAGE
(
  BUFFER_POOL DEFAULT
)
NOPARALLEL;
```
### 创建本地分区索引ORDERS_INDEX_CUSTOMER_NAME：
```sql
CREATE INDEX ORDERS_INDEX_CUSTOMER_NAME ON ORDERS (CUSTOMER_NAME ASC)
NOLOGGING
TABLESPACE USERS
PCTFREE 10
INITRANS 2
STORAGE
(
  INITIAL 65536
  NEXT 1048576
  MINEXTENTS 1
  MAXEXTENTS UNLIMITED
  BUFFER_POOL DEFAULT
)
NOPARALLEL;
```
### 创建本地分区索引ORDERS_PK：
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/10.png)<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/11.png)<br>
### 创建主键和外键
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/12.png)
### 创建表ORDER_DETAILS
```sql
CREATE TABLE ORDER_DETAILS
(
  ID NUMBER(10, 0) NOT NULL
, ORDER_ID NUMBER(10, 0) NOT NULL
, PRODUCT_NAME VARCHAR2(40 BYTE) NOT NULL
, PRODUCT_NUM NUMBER(8, 2) NOT NULL
, PRODUCT_PRICE NUMBER(8, 2) NOT NULL
, CONSTRAINT ORDER_DETAILS_FK1 FOREIGN KEY
  (
  ORDER_ID
  )
  REFERENCES ORDERS
  (
  ORDER_ID
  )
  ENABLE
)
TABLESPACE USERS
PCTFREE 10
INITRANS 1
STORAGE
(
  BUFFER_POOL DEFAULT
)
NOCOMPRESS
NOPARALLEL
PARTITION BY REFERENCE (ORDER_DETAILS_FK1)
(
  PARTITION PARTITION_BEFORE_2016
  NOLOGGING
  TABLESPACE USERS --必须指定表空间，否则会将分区存储在用户的默认表空间中
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    INITIAL 8388608
    NEXT 1048576
    MINEXTENTS 1
    MAXEXTENTS UNLIMITED
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY,
  PARTITION PARTITION_BEFORE_2017
  NOLOGGING
  TABLESPACE USERS02
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    INITIAL 8388608
    NEXT 1048576
    MINEXTENTS 1
    MAXEXTENTS UNLIMITED
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY
);
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/13.png)
### 创建索引ORDER_DETAILS_PK
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/29.png)
### 创建主键
```sql
ALTER TABLE ORDER_DETAILS
ADD CONSTRAINT ORDER_DETAILS_PK PRIMARY KEY
(
  ID
)
USING INDEX ORDER_DETAILS_PK
ENABLE;
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/14.png)
### 创建索引ORDER_DETAILS_ORDER_ID
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/30.png)
### 创建主键
```sql
ALTER TABLE ORDER_DETAILS
ADD CONSTRAINT ORDER_DETAILS_PRODUCT_NUM CHECK
(Product_Num>0)
ENABLE;
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/15.png)
### 创建3个触发器
```sql
CREATE OR REPLACE EDITIONABLE TRIGGER "ORDERS_TRIG_ROW_LEVEL"
BEFORE INSERT OR UPDATE OF DISCOUNT ON "ORDERS"
FOR EACH ROW --行级触发器
declare
  m number(8,2);
BEGIN
  if inserting then
       :new.TRADE_RECEIVABLE := - :new.discount;
  else
      select sum(PRODUCT_NUM*PRODUCT_PRICE) into m from ORDER_DETAILS where ORDER_ID=:old.ORDER_ID;
      if m is null then
        m:=0;
      end if;
      :new.TRADE_RECEIVABLE := m - :new.discount;
  end if;
END;
/
```
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/16.png)
### 批量插入订单数据之前，禁用触发器
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/17.png)<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/18.jpg)<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/19.png)
### 创建序列与视图创建<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/20.png)
### 插入departments，employees数据<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/21.png)<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/22.png)<br>
### 递归查询某个员工及其所有下属，子下属员工
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/25.png)<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/26.png)<br>
### 查询订单表，并且包括订单的订单应收货款: Trade_Receivable= sum(订单详单表.ProductNum*订单详单表.ProductPrice)- Discount。
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/27.png)<br>
### 查询部门表，同时显示部门的负责人姓名。
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test4/28.png)<br>
