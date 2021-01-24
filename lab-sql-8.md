-- Lab | SQL Queries 8
-- 1. Rank films by length (filter out the rows that have nulls or 0s in length column). In your output, only select the columns title, length, and the rank.

select  title, length, rank() over (order by length desc) as "Rank" from sakila.film
where length is not null and length > 0
limit 50;
-- from manshu
select title, length, RANK() over (ORDER BY length) ranks
from sakila.film
where length is not null and length > 0;

-- 2. Rank films by length within the rating category (filter out the rows that have nulls or 0s in length column). In your output, only select the columns title, length, rating and the rank.

select  title, length, rating, rank() over (partition by rating order by rating) as "Rank" from sakila.film
where length is not null
order by rating
limit 50;
-- from manshu
select title, length, rating, rank() over (partition by rating order by length desc) as ranks
from sakila.film
where length is not null and length > 0;



-- 3. How many films are there for each of the categories in the category table. Use appropriate join to write this query

select count(*), fc.category_id -- 3
from sakila.film_category as fc -- 1
left join sakila.film as f 
on fc.film_id = f.film_id
group by fc.category_id -- 2
order by fc.category_id; -- 4
-- from manshu
select name as category_name, count(*) as num_films
from sakila.category as cats 
inner join sakila.film_category film_cats
on cats.category_id = film_cats.category_id
group by category_name
order by num_films desc;


-- 4. Which actor has appeared in the most films?
-- from manshu
select actor.actor_id, actor.first_name, actor.last_name,
count(actor_id) as film_count
from sakila.actor join sakila.film_actor using (actor_id)
group by actor.actor_id
order by film_count desc
limit 1;

-- 5. Most active customer (the customer that has rented the most number of films)
-- frmo manshu
select customer.*,
count(rental_id) as rental_count
from sakila.customer join sakila.rental using (customer_id)
group by customer_id
order by rental_count desc
limit 1;

-- BONUS (from manshu)
select film.title, count(rental_id) as rental_count
from sakila.film inner join sakila.inventory using (film_id)
inner join sakila.rental using (inventory_id)
group by film_id
order by rental_count desc
limit 1;