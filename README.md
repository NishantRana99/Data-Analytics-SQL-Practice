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


# Q5- Compare each employee's salary with the average salary of the corresponding department.
Output the department, first name, and salary of employees along with the average salary of that department.

sol- select department, first_name, salary,
avg(salary) over( PARTITION BY department) as avg_salary
from employee;

# Q6- What is the total sales revenue of Samantha and Lisa?
sol- select sum(sales_revenue) as total_rvenue from sales_performance where salesperson in ('Samantha', 'Lisa')

# Q7- Find wine varieties tasted by 'Roger Voss' and with a value in the 'region_1' column of the dataset. Output unique variety names only.
sol- select distinct variety  from winemag_p2 where taster_name = 'Roger Voss'and region_1 is not null;

# Q8- Find the hour with the highest gasoline cost. Assume there's only 1 hour with the highest gas cost.
sol- SELECT hour 
FROM lyft_rides 
ORDER BY gasoline_cost DESC 
LIMIT 1;

# Q9- Find all Lyft rides that happened on rainy days before noon.
sol- select * from lyft_rides where weather = 'rainy' and hour < '12' ;

# Q10- You are given a dataset of health inspections that includes details about violations. Each row represents an inspection, and if an inspection resulted in a violation, the violation_id column will contain a value. Count the total number of violations that occurred at 'Roxanne Cafe' for each year, based on the inspection date. Output the year and the corresponding number of violations in ascending order of the year.
sol- select extract(year from inspection_date )as n_Year,
count(violation_id) as n_violation
from sf_restaurant_health_violations
where business_name = 'Roxanne Cafe'
group by  extract(year from inspection_date)
ORDER BY n_Year ASC;
# Q11- Find how many times each artist appeared on the Spotify ranking list. Output the artist name along with the corresponding number of occurrences.Order records by the number of occurrences in descending order.
sol- select  artist, count(position) as n_occurences
    from spotify_worldwide_daily_song_ranking
    group by artist
    order by n_occurences desc;

# Q12-Find songs that have ranked in the top position. Output the track name and the number of times it ranked at the top. Sort your records by the number of times the song was in the top position in descending order.
sol- select  trackname,
sum(case when position = '1' then 1 else 0 end ) as tims_top1
from spotify_worldwide_daily_song_ranking 
group by trackname
HAVING SUM(CASE WHEN position = '1' THEN 1 ELSE 0 END) > 0
order by tims_top1 desc;

# Q13- Find the lowest, average, and the highest ages of athletes across all Olympics. HINT: If an athlete participated in more than one discipline at one Olympic game, consider it as a separate athlete; no need to remove such edge cases.
sol- select 
    min(age) as lowest_age,
    avg(age)as mean_age, 
    max(age)as highest_age

# Q14- Find order details made by Jill and Eva. Consider Jill and Eva as first names of customers. Output the order date, details, and cost along with the first name. Order records based on the customer id in ascending order.
sol- select c.first_name ,o.order_date, o.order_details, o.total_order_cost
from customers c
join orders o 
on c.id = o.cust_id
where first_name in ('Jill','Eva')
order by o.cust_id;
    from olympics_athletes_events;
    
