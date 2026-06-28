# SQL-row_number-rank-dense_rank-function
Solving SQL questions based on row_number( ), rank( ), dense_rank( ) function


#creating new database

create database new_topic;
show databases;
use new_topic;

#creating table

CREATE TABLE employee (
    emp_id INT,
    emp_name VARCHAR(50),
    department VARCHAR(30),
    salary INT
);

# inserting values

insert into employee values
(101,'Alice','HR',60000),
(102,'Bob','HR',75000),
(103,'Charlie','HR',75000),
(104,'David','HR',55000),
(105,'Eva','IT',90000),
(106,'Frank','IT',85000),
(107,'Grace','IT',85000),
(108,'Henry','IT',70000),
(109,'Ivy','Sales',65000),
(110,'Jack','Sales',65000),
(111,'Kevin','Sales',80000),
(112,'Lily','Sales',50000);

select * from employee;

#Question 1: Display all employees with a sequential number based on salary in descending order. 
# (Here sequential number asked so we may use row_number() but since it is based on salary, I am using dense_rank() )

select *, dense_rank() over(order by salary desc) as Sequential_number from employee;

#Question 2: Find the employees having the highest salary in each department.
#(rank and dense_rank() both will give the same output, since the rank 1 will be 1 only in both rank or dense_rank() )

select * from 
( select *, rank() over(partition by department order by salary desc) as new from employee)
new2
where new=1;

#Question 3: Display the salary position of every employee across the whole company. 
#Employees with the same salary should share the same position.
#(dense_rank() function will allow duplicate rank and also will not skip gaps between the rank)

select * from 
( select *, dense_rank() over(order by salary desc) as new from employee)
new2;

#Question 4: Find the top 2 highest-paid employees in each department. 
#If multiple employees qualify because of a tie, include all of them.
#(dense_rank() function will allow duplicate rank and also will not skip gaps between the rank.
#That's why if we have two employee having highest salary they will get rank 1 and 
#the next higher salary employee will get rank 2 but if we have used rank() function then that employee will get rank 3 
#since rank() function skip gap)

select * from 
( select *, dense_rank() over(partition by department order by salary desc) as new from employee) as
new2
where new<=2;

#Question 5: Display employees ordered by salary (highest first).
# If two employees have the same salary, their order should still be unique and consistent using emp_id.
#(Read the condition that order should be unique even if employee have same salary)

select *, row_number() over(order by salary desc) as rank_by_salary from employee;

#Question 6 : Find the second highest salary in each department or Find employees whose salary position is exactly 2 in each department..

select * from 
( select *, dense_rank() over(partition by department order by salary desc) as new from employee) as
new2
where new=2;

#Question 7: Display all employees whose salary falls within the top 3 salary positions in each department.

select * from 
( select *, dense_rank() over(partition by department order by salary desc) as new from employee) as
new2
where new<=3;

#Question 8: Assign a sequence number to employees within each department ordered by employee name alphabetically.

select *, row_number() over(partition by department order by emp_name asc) as Seq_by_emp_name from employee;

