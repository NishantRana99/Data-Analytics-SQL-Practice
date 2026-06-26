# Data-Analytics-SQL-Practice
Solved SQL interview questions covering JOINs, GROUP BY, Subqueries, CTEs, Window Functions, and analytical problem-solving for Data Analyst interview preparation

# Q1-Find all posts that were reacted to with a heart
sol- select p.post_id, p.poster,	p.post_text, p.post_keywords, p.post_date
from facebook_posts p
join facebook_reactions r 
on p.post_id = r.post_id
where r.reaction = 'heart'
group by p.post_id;

# Q2-We have a table with employees and their salaries; however, some of the records are old and contain outdated salary information. Since there is no timestamp, assume salary is non-decreasing over time. You can consider the current salary for an employee is the largest salary value among their records. If multiple records share the same maximum salary, return any one of them. Output their id, first name, last name, department ID, and current salary. Order your list by employee ID in ascending order.
sol- SELECT id,
       first_name,
       last_name,
       department_id,
       salary
FROM (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY id ORDER BY salary DESC) AS rn
    FROM ms_employee_salary
) t
WHERE rn = 1
ORDER BY id ASC;

# Q3-Find the total cost of each customer's orders. Output the customer's ID, first name, and the total order cost. Order records by the customer's first name alphabetically.
sol-SELECT  c.id, c.first_name, 
SUM(o.total_order_cost) AS total_order_cost
FROM customers c
JOIN 
orders o ON c.id = o.cust_id 
GROUP BY c.id, c.first_name 
ORDER BY c.first_name ASC;

# Q4-Management wants to analyse only employees with official job titles. Find the title(s) of the worker(s) with the highest salary among workers who have a corresponding record in the  title  table. If multiple employees have the same highest salary, include all their job titles.
sol- select t.worker_title as best_paid_title
from title t
join worker w
on t.worker_ref_id = w.worker_id
order by w.salary desc limit 2;


# Q4- Compare each employee's salary with the average salary of the corresponding department.
Output the department, first name, and salary of employees along with the average salary of that department.

sol- select department, first_name, salary,
avg(salary) over( PARTITION BY department) as avg_salary
from employee;


# Q5- What is the total sales revenue of Samantha and Lisa?
sol- select sum(sales_revenue) as total_rvenue from sales_performance where salesperson in ('Samantha', 'Lisa')

# Q6- Find wine varieties tasted by 'Roger Voss' and with a value in the 'region_1' column of the dataset. Output unique variety names only.
sol- select distinct variety  from winemag_p2 where taster_name = 'Roger Voss'and region_1 is not null;
