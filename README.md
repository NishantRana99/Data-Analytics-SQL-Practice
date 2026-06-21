# Data-Analytics-SQL-Practice
Solved SQL interview questions covering JOINs, GROUP BY, Subqueries, CTEs, Window Functions, and analytical problem-solving for Data Analyst interview preparation

# Q1-Find all posts that were reacted to with a heart
sol- select p.post_id, p.poster,	p.post_text, p.post_keywords, p.post_date
from facebook_posts p
join facebook_reactions r 
on p.post_id = r.post_id
where r.reaction = 'heart'
group by p.post_id;
