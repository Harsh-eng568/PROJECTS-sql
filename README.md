# PROJECTS-sql
use pizza_sales;

SELECT * FROM order_details;
SELECT * FROM orders;
SELECT * FROM pizzas;
SELECT * FROM pizza_types;

-- QUES 1 : Retrieve total number of order placed?

SELECT COUNT(*) FROM order_details;

-- QUES 2 : Retrieve total revenue generated from pizza sales ?

SELECT ROUND(SUM(od.quantity * p.price),2) AS Total_revenue  FROM order_details od INNER JOIN pizzas p ON p.pizza_id = od .pizza_id;

-- QUES 3 : Identify the highest priced pizza

SELECT pizza_id, price FROM pizzas ORDER BY price DESC LIMIT 1 ;

-- QUES 4 : Identify the most common  pizza size ordered.

SELECT p.size, COUNT(od.order_details_id) AS Order_Count FROM order_details od INNER JOIN pizzas p 
ON p.pizza_id = od .pizza_id
GROUP BY p.size 
ORDER BY Order_Count DESC
LIMIT 1;

-- QUES 5 : LIST THE TOP 5 MOST ORDERED PIZZA TYPES ALONG WITH THERE QUANTITY

SELECT pizza_type_id, SUM(quantity)  FROM order_details od INNER JOIN pizzas p
ON p.pizza_id = od .pizza_id
GROUP BY pizza_type_id
ORDER BY SUM(quantity) DESC
LIMIT 5 ;

-- QUES 6 : Determine the distribution of orders by hour of the day

SELECT Hour(time) as Hour,count(*) AS ORDER_COUNT
FROM orders
GROUP BY Hour(time)
ORDER BY count(*) DESC;

-- QUES 7 : Determine the top 3 most ordered pizza types based on revenue

SELECT pt.name, SUM(od.quantity * pp.price) as revenue
FROM pizza_types pt INNER JOIN pizzas pp
 ON pt.pizza_type_id = pp.pizza_type_id
INNER JOIN  order_details od 
ON od.pizza_id = pp.pizza_id
GROUP BY pt.name
ORDER BY revenue DESC
LIMIT 3;

-- QUES 8 : Calculate the % contribution of each pizza category to the total revenue

SELECT 
    pizza_types.category,
    ROUND(
        (SUM(order_details.quantity * pizzas.price) / 
        (SELECT SUM(order_details.quantity * pizzas.price) FROM order_details 
         JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id)) * 100, 2
    ) AS percentage_contribution
FROM 
    order_details
JOIN 
    pizzas ON order_details.pizza_id = pizzas.pizza_id
JOIN 
    pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id
GROUP BY 
    pizza_types.category;

