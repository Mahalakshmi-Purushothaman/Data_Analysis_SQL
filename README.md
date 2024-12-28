Order Trends
1.	Retrieve the total number of orders placed.
2.	SELECT COUNT(order_id) AS total_orders
FROM orders;
The total number of orders placed provides an overview of customer activity and business performance. Here the total orders are 21,350, which indicates steady demand and customer engagement.
2.	Determine the distribution of orders by hour of the day.
3.	SELECT HOUR(order_time) AS hour, COUNT(order_id) AS frequency_of_orders
4.	FROM orders
5.	GROUP BY hour
ORDER BY frequency_of_orders DESC;
Analyzing the frequency of orders by hour reveals peak business hours. This can help optimize staffing and operational efficiency. Most orders are placed between 12 PM and 1 PM and then 4 PM to 6 PM, this time period is critical for maximizing service speed and customer satisfaction.
3.	Analyze seasonal variations in pizza orders.
4.	SELECT MONTH(order_date) AS month, COUNT(order_id) AS order_count
5.	FROM orders
6.	GROUP BY month
7.	ORDER BY order_count DESC;
Understanding seasonal trends in orders can guide promotional strategies and menu adjustments. Here we see there is a spike in orders during July, therefore, launching seasonal specials during this period could boost sales further.
4.	Identify the busiest day for orders.
5.	SELECT orders.order_date, SUM(order_details.quantity) AS total_quantity
6.	FROM orders
7.	JOIN order_details ON orders.order_id = order_details.order_id
8.	GROUP BY orders.order_date
9.	ORDER BY total_quantity DESC
10.	LIMIT 1;
Revenue Analysis
1.	Calculate the total revenue generated from pizza sales.
2.	SELECT ROUND(SUM(order_details.quantity * pizzas.price),2) AS total_revenue
3.	FROM order_details
4.	JOIN pizzas
ON order_details.pizza_id = pizzas.pizza_id;
We can calculate the total revenue rounded to 2 decimal places by simply using the above query and adding ROUND to it.
Calculating total revenue provides a measure of the business's financial performance. The total revenue is $817860.05, which indicates the overall sales success.
2.	Determine the top 3 most ordered pizza types based on revenue.
3.	SELECT pizza_types.name, SUM(order_details.quantity * pizzas.price) AS revenue
4.	FROM pizza_types
5.	JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
6.	JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
7.	GROUP BY pizza_types.name
8.	ORDER BY revenue DESC
9.	LIMIT 3;
Identifying the top 3 pizza types by revenue highlights the most profitable items. Focusing on these pizzas in marketing campaigns can maximize profitability.
3.	Calculate the percentage contribution of each pizza category to total revenue.
4.	SELECT pizza_types.category, 
5.	    SUM(order_details.quantity * pizzas.price) / (
6.	        SELECT SUM(order_details.quantity * pizzas.price)
7.	        FROM order_details 
8.	        JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id
9.	    ) * 100 AS revenue_percentage
10.	FROM pizza_types
11.	JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
12.	JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
13.	GROUP BY pizza_types.category
14.	ORDER BY revenue_percentage DESC;
Analyzing the revenue contribution by category helps understand which categories drive the most sales. Classic pizzas contribute 26.91% of total revenue.
4.	Identify the highest-priced pizza.
5.	SELECT pizza_types.name, pizzas.price, pizza_types.ingredients
6.	FROM pizza_types
7.	INNER JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
8.	ORDER BY pizzas.price DESC
9.	LIMIT 1;
Knowing the highest-priced pizza helps in pricing strategy and understanding customer spending behaviour. If the most expensive pizza is a speciality pizza with premium ingredients, highlighting its unique features can justify its price. At this pizza outlet, we have the highest-priced pizza as The Greek Pizza which has Kalamata Olives, Feta Cheese, Tomatoes, Garlic, Beef Chuck Roast, and Red Onions as its ingredients making it pricey.
Pizza Preferences
1.	Identify the most common pizza size ordered.
2.	SELECT pizzas.size, COUNT(order_details.order_details_id) AS order_count
3.	FROM pizzas
4.	INNER JOIN order_details ON pizzas.pizza_id = order_details.pizza_id
5.	GROUP BY pizzas.size
6.	ORDER BY order_count DESC
LIMIT 1;
Knowing the most popular pizza size helps in managing inventory and pricing strategies. The L i.e. LARGE size is the most popular, therefore ensuring a sufficient supply of its pizza ingredients is crucial.
2.	List the top 5 most ordered pizza types along with their quantities.
3.	SELECT pizza_types.name, SUM(order_details.quantity) AS total_quantity_ordered
4.	FROM pizza_types
5.	INNER JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
6.	INNER JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
7.	GROUP BY pizza_types.name
8.	ORDER BY total_quantity_ordered DESC
LIMIT 5;
Identifying the top 5 pizza types in terms of quantity ordered highlights customer favourites. This information is useful for menu optimization and targeted marketing campaigns.
3.	Find the total quantity of each pizza category ordered.
4.	SELECT pizza_types.category, SUM(order_details.quantity)
5.	FROM pizza_types
6.	INNER JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
7.	JOIN order_details ON pizzas.pizza_id = order_details.pizza_id
8.	GROUP BY pizza_types.category;
Analyzing the total quantity ordered by pizza category helps understand customer preferences for different pizza styles. Classic pizzas are highly popular, so expanding this category could attract more customers.
4.	Analyze customer preferences by pizza category. Identify the least and most popular pizza categories.
5.	SELECT pizza_types.category, SUM(order_details.quantity)
6.	FROM pizza_types
7.	INNER JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
8.	INNER JOIN order_details ON pizzas.pizza_id = pizzas.pizza_id
9.	GROUP BY pizza_types.category
10.	ORDER BY SUM(order_details.quantity);
Identifying the least and most popular pizza categories provides insights into customer preferences and areas for menu improvement. Veggie pizzas are the least popular, SO WWE MIGHT HAVE TO reconsider their recipes or pricing might be necessary.
5.	How many pizza types are there in total?
SELECT COUNT(pizza_type_id) AS total_pizza_types FROM pizza_types;
Knowing the total number of pizza types available helps in understanding the variety offered to customers. We have 32 different pizza types, which indicates a diverse menu catering to various tastes and preferences. This variety can attract a wider customer base and meet different customer needs, enhancing overall customer satisfaction.
Customer Insights
1.	Calculate the average number of pizzas ordered per day.
2.	SELECT AVG(quantity)
3.	FROM (SELECT orders.order_date AS date, SUM(order_details.quantity) AS quantity
4.	FROM orders
5.	JOIN order_details ON orders.order_id = order_details.order_id
GROUP BY date) AS daily_quantities;
2.	Identify repeat customers based on order frequency.
3.	SELECT order_id, COUNT(order_id)
4.	FROM order_details
5.	GROUP BY order_id
6.	HAVING COUNT(order_id) > 1
7.	ORDER BY COUNT(order_id) DESC;
8.	
The table is quite long for this one, so I could not upload it entirely.
Identifying repeat customers helps in understanding customer loyalty and designing loyalty programs.
3.	Analyze the cumulative revenue generated over time.
4.	SELECT order_date, 
5.	    SUM(revenue) OVER (ORDER BY order_date) AS cumulative_revenue
6.	FROM (
7.	 SELECT orders.order_date, 
8.	        SUM(order_details.quantity * pizzas.price) AS revenue
9.	 FROM order_details
10.	 JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id
11.	 JOIN orders ON order_details.order_id = orders.order_id
12.	 GROUP BY orders.order_date
13.	) AS daily_revenues;
