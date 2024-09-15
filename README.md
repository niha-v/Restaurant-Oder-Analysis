--Use Restaurant Data.

use restaurant_db;

-- View Menu_items table.

select * from menu_items;

-- Find number of items on the menu.

select count(*) from menu_items;

-- Least and expensive item.

select * from menu_items
order by price;

select * from menu_items
order by price desc;

-- Italain dishes.

select count(*) from menu_items
where category = 'Italian';

-- Least and expesive italain dish.

select * from menu_items
where category = 'Italian'
order by price;

-- Dishes in each category.

select category, count(menu_item_id) as num_dishes
from menu_items
group by category;

-- AVG dish price in each category
select category, avg(price) as avg_price
from menu_items
group by category;

-- view order details table 
select * from order_details;

-- orders made within a date range 
select min(order_date), max(order_date) from order_details;

-- orders made in the date range
select count(distinct(order_id)) from order_details;

-- items ordered within a date range
select count(*) from order_details;

-- which order has more number if items
select order_id, count(item_id) as num_items 
from order_details
group by order_id
order by num_items DESC;

-- how may items had more than 12 items

select count(*) from
(select order_id, count(item_id) as num_items 
from order_details
group by order_id
having num_items > '12') as num_orders;


-- Analyze Customer Behavior

-- Combine the menu_items and order_details table 

select *
from order_details od Left join menu_items mi
     on od.item_id = mi.menu_item_id;
     
-- What were the least and most ordered items? What categories were they in?

select item_name, count(order_details_id) as num_purchases
from order_details od Left join menu_items mi
     on od.item_id = mi.menu_item_id
group by item_name 
order by num_purchases desc ;

-- Category

select item_name, category, count(order_details_id) as num_purchases
from order_details od Left join menu_items mi
     on od.item_id = mi.menu_item_id
group by item_name, category 
order by num_purchases ;

-- Top 5 orders that spent most money?

select order_id, sum(price) as total_spent
from order_details od Left join menu_items mi
     on od.item_id = mi.menu_item_id
group by order_id
order by total_spent desc
limit 5;   

-- Highest spent order details

select category, count(item_id) as num_items 
from order_details od Left join menu_items mi
     on od.item_id = mi.menu_item_id
where order_id = 440
group by category ;  
     
--  Top 5 highest spent orders
    
select category, count(item_id) as num_items 
from order_details od Left join menu_items mi
     on od.item_id = mi.menu_item_id
where order_id in (440, 2075, 1957, 330, 2675)
group by category; 

 -- using order_id to identify each customer detalils 

select order_id, category, count(item_id) as num_items 
from order_details od Left join menu_items mi
     on od.item_id = mi.menu_item_id
where order_id in (440, 2075, 1957, 330, 2675)
group by order_id, category;
     
-- insights gathered in this - most of the customers like Italian food even though the price is high
-- Most purchased food is hamburger and least is Chicken Tacos(mexican food)
