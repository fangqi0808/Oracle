# Oracle
## 实验一<br>
查询1：

```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
```

![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A21.1.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A21.2.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A21.3.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/1.1.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/1.2.png)

- 查询2：
```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```

![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A22.1.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A22.2.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A22.3.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/2.1.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/2.2.png)
