# Oracle
实验一<br>
<b>查询1</b><br>
SELECT d.department_name，count(e.job_id)as "部门总人数"，<br>
avg(e.salary)as "平均工资"<br>
from hr.departments d，hr.employees e<br>
where d.department_id = e.department_id<br>
and d.department_name in ('IT'，'Sales')<br>
GROUP BY department_name;<br>
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A21.1.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A21.2.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A21.3.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/1.1.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/1.2.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A22.1.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A22.2.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/%E6%9F%A5%E8%AF%A22.3.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/2.1.png)
![Alt](https://github.com/fangqi201610414409/Oracle/blob/master/test1/2.2.png)
