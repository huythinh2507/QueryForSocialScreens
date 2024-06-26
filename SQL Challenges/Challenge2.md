## A. Pizza Metrics

### 1. How many pizzas were ordered?

````sql
SELECT COUNT(*) AS total_pizzas_ordered
FROM customer_orders;
````

**Answer:**

| total_pizzas_ordered|
|---|
|14|

- Total of 14 pizzas were ordered.

### 2. How many unique customer orders were made?

````sql
SELECT COUNT(DISTINCT order_id) AS unique_customer_orders
FROM customer_orders;

````

**Answer:**

| total_pizzas_ordered|
|---|
|10|

- There are 10 unique customer orders.

### 3. How many successful orders were delivered by each runner?

````sql
SELECT runner_id, COUNT(*) AS successful_orders
FROM runner_orders
WHERE cancellation IS NULL OR cancellation = ''
GROUP BY runner_id;
````

**Answer:**
| runner_id | successful_orders |
| ----------- | ----------- |
| 1           | 3          |
| 2           | 1         |
| 3           | 1         |

- Runner 1 has 3 successful delivered orders.
- Runner 2 has 1 successful delivered orders.
- Runner 3 has 1 successful delivered order.

### 4. How many of each type of pizza was delivered?

````sql
SELECT 
  users.user_id, 
  users.email, 
  users.name, 
  users.role, 
  users.profile_pic, 
  users.phone_number
FROM 
  users
JOIN 
  follows ON users.user_id = follows.followee_id
WHERE 
  follows.follower_id = 1;

-- Query to retrieve a list of people that are following user 1
SELECT 
   users.user_id, 
  users.email, 
  users.name, 
  users.role, 
  users.profile_pic, 
  users.phone_number
FROM 
  users
JOIN 
  follows ON users.user_id = follows.follower_id
WHERE 
  follows.followee_id = 1;
````

**Answer:**
| pizza_name | number_delivered |
| ----------- | ----------- |
| Meatlovers           | 6          |
| Vegetarian           | 2         |

- There are 6 delivered Meatlovers pizzas and 2 Vegetarian pizzas.

### 5. How many Vegetarian and Meatlovers were ordered by each customer?**

````sql
SELECT customer_id, pizza_names.pizza_name, COUNT(customer_orders.pizza_id) AS number_ordered
FROM customer_orders
JOIN pizza_names ON customer_orders.pizza_id = pizza_names.pizza_id
GROUP BY customer_id, pizza_names.pizza_name
ORDER BY customer_id ASC;
````

**Answer:**
| customer_id | pizza_name  |number_ordered |
| ----------- | ----------- |-----------    |
| 101  |      Meatlovers           |2              |
| 101  |      Vegetarian      |1              |
| 102  |      Meatlovers         |2              |
| 102  |  Vegetarian          |1              |
| 103  |   Meatlovers         |3              |
| 103  | Vegetarian           |1              |
| 104  |  Meatlovers          |3              |
| 105  |  Vegetarian          |1              |

- Customer 101 ordered 2 Meatlovers pizzas and 1 Vegetarian pizza.
- Customer 102 ordered 2 Meatlovers pizzas and 1 Vegetarian pizzas.
- Customer 103 ordered 3 Meatlovers pizzas and 1 Vegetarian pizza.
- Customer 104 ordered 3 Meatlovers pizza.
- Customer 105 ordered 1 Vegetarian pizza.

### 6. What was the maximum number of pizzas delivered in a single order?

````sql
SELECT order_id, COUNT(pizza_id) AS pizzas_in_order
FROM customer_orders
GROUP BY order_id
ORDER BY pizzas_in_order DESC
LIMIT 1;
````

**Answer:**

| order_id | pizzas_in_order |
| ----------- | ----------- |
| 4           | 3          |

- Maximum number of pizza delivered in a single order is 3 pizzas.

### 7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?

````sql
SELECT customer_id,
       SUM(CASE WHEN exclusions != '' AND exclusions != 'null' OR extras != '' AND extras != 'null' THEN 1 ELSE 0 END) AS pizzas_with_changes,
       SUM(CASE WHEN exclusions = ''  AND extras = '' THEN 1 ELSE 0 END) AS pizzas_no_changes
FROM customer_orders
JOIN runner_orders ON customer_orders.order_id = runner_orders.order_id
WHERE runner_orders.cancellation IS NULL OR runner_orders.cancellation = '' OR runner_orders.cancellation = 'null'
GROUP BY customer_id
ORDER BY customer_id ASC;
````

**Answer:**
| customer_id | pizzas_with_changes  |pizzas_no_changes |
| ----------- | ----------- |-----------    |
| 101  |      0           |2              |
| 102  |      0         |2              |
| 103  |   3         |0            |
| 104  |  2          |0             |
| 105  |  1          |0            |


### 8. How many pizzas were delivered that had both exclusions and extras?

````sql
SELECT COUNT(*) AS pizzas_with_exclusions_and_extras
FROM customer_orders
JOIN runner_orders ON customer_orders.order_id = runner_orders.order_id
WHERE (exclusions != '' AND exclusions IS NOT NULL)
  AND (extras != '' AND extras IS NOT NULL)
  AND (runner_orders.cancellation IS NULL OR runner_orders.cancellation = '');

````

**Answer:**
| pizzas_with_exclusions_and_extras |
| ----------- | 
| 1           | 

- Only 1 pizza delivered that had both extra and exclusion topping. 

### 9. What was the total volume of pizzas ordered for each hour of the day?

````sql
SELECT EXTRACT(HOUR FROM order_time) AS hour_of_day, COUNT(*) AS total_orders
FROM customer_orders
GROUP BY hour_of_day
ORDER BY hour_of_day;
````

**Answer:**
| hour_of_day     | total_orders |
| ----------- | ----------- |
|11           | 1          |
|  13         | 3      |
|   18        |3       |
|    19       |1        |
|    21       | 3          |
|    23       |3         |

### 10. What was the volume of orders for each day of the week?

````sql
SELECT 
  EXTRACT(ISODOW FROM order_time) AS day_of_week,
  COUNT(order_id) AS volume_of_orders
FROM 
  customer_orders
GROUP BY 
  day_of_week;
````

**Answer:**
| day_of_week     | volume_of_orders |
| ----------- | ----------- |
|3          |5        |
|  4       | 3      |
|   6        |5      |
|   5      |1        |


***

## B. Runner and Customer Experience

### 1. How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)

````sql
SELECT 
  DATE_TRUNC('week', registration_date) AS week,
  COUNT(runner_id) AS num_runners
FROM 
  runners
GROUP BY 
  week
ORDER BY 
  week;
````

**Answer:**
| week                             | num_runners |
| ---------------------------------| ----------- |
|2020-12-28T00:00:00.000Z          |2            |
|2021-01-04T00:00:00.000Z          |1            |
|2021-01-11T00:00:00.000Z          |1            |



- On Week 1 of Jan 2021, 2 new runners signed up.
- On Week 2 and 3 of Jan 2021, 1 new runner signed up.

### 2. What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?

````sql
SELECT 
  runner_id, 
  AVG(EXTRACT(EPOCH FROM (pickup_time::timestamp - order_time::timestamp))/60) AS avg_time_minutes
FROM 
  runner_orders 
JOIN 
  customer_orders USING(order_id)
WHERE 
  pickup_time <> '' AND pickup_time IS NOT NULL
GROUP BY 
  runner_id;
````

**Answer:**
| week                             | avg_time_minutes |
| ---------------------------------| ----------- |
|3          |10.466666666666667            |
|2          |23.720000000000002            |
|1          |15.677777777777777            |

### 3. Is there any relationship between the number of pizzas and how long the order takes to prepare?

````sql
SELECT 
  order_id,
  COUNT(pizza_id) AS num_pizzas, 
  AVG(EXTRACT(EPOCH FROM (pickup_time::timestamp - order_time::timestamp))/60) AS avg_prep_time_minutes
FROM 
  customer_orders 
JOIN 
  runner_orders USING(order_id)
WHERE 
  pickup_time <> '' AND pickup_time IS NOT NULL
GROUP BY 
  order_id;
````
**Answer:**

| order_id | num_pizzas | avg_prep_time_minutes |
| -------- | ---------- | --------------------- |
| 1        | 1          | 10.533333333333333    |
| 2        | 1          | 10.033333333333333    |
| 3        | 2          | 21.233333333333334    |
| 4        | 3          | 29.283333333333335    |
| 5        | 1          | 10.466666666666667    |
| 7        | 1          | 10.266666666666667    |
| 8        | 1          | 20.483333333333334    |
| 10       | 2          | 15.516666666666667    |

### 4. What was the average distance travelled for each customer?

````sql
SELECT 
  customer_id, 
  AVG(CAST(distance AS FLOAT)) AS avg_distance
FROM 
  customer_orders 
JOIN 
  runner_orders USING(order_id)
WHERE 
  distance <> ''
GROUP BY 
  customer_id
ORDER BY customer_id asc;
````

**Answer:**
| customer_id | avg_distance       |
| ----------- | ------------------ |
| 101         | 20                 |
| 102         | 16.733333333333334 |
| 103         | 23.399999999999995 |
| 104         | 10                 |
| 105         | 25                 |

### 5. What was the difference between the longest and shortest delivery times for all orders?

````sql
WITH durations AS (
  SELECT order_id, CAST(duration AS INTEGER) AS duration
  FROM runner_orders
  WHERE duration <> '' AND cancellation = ''
)
SELECT MAX(duration) - MIN(duration) AS difference
FROM durations;
````
**Answer:**
| difference | 
| ----------- |
| 30         |             


### 6. What was the average speed for each runner for each delivery and do you notice any trend for these values?

````sql
SELECT 
  runner_id, 
  order_id, 
  (CAST(distance AS FLOAT) / (EXTRACT(EPOCH FROM (pickup_time::timestamp - order_time::timestamp))/3600)) AS avg_speed_kmph
FROM 
  customer_orders 
JOIN 
  runner_orders USING(order_id)
WHERE 
  pickup_time IS NOT NULL AND distance <> '';
````
**Answer:**
| runner_id | order_id | avg_speed_kmph     |
| --------- | -------- | ------------------ |
| 1         | 1        | 113.9240506329114  |
| 1         | 2        | 119.60132890365449 |
| 1         | 3        | 37.86499215070644  |
| 1         | 3        | 37.86499215070644  |
| 2         | 4        | 47.94536141149686  |
| 2         | 4        | 47.94536141149686  |
| 2         | 4        | 47.94536141149686  |
| 3         | 5        | 57.324840764331206 |
| 2         | 7        | 146.10389610389612 |
| 2         | 8        | 68.54353132628152  |
| 1         | 10       | 38.66809881847475  |
| 1         | 10       | 38.66809881847475  |


### 7. What is the successful delivery percentage for each runner?

````sql
SELECT 
  runner_id, 
  COUNT(order_id) FILTER (WHERE cancellation IS NULL OR cancellation = '') * 100.0 / COUNT(order_id) AS success_percentage
FROM 
  runner_orders
GROUP BY 
  runner_id;
````
**Answer:**

| runner_id | success_percentage   |
| --------- | -------------------- |
| 3         | 50.0000000000000000  |
| 2         | 75.0000000000000000  |
| 1         | 100.0000000000000000 |

## C. Ingredient Optimisation

### 1. What are the standard ingredients for each pizza?

````sql
SELECT pn.pizza_name, pt.topping_name
FROM pizza_recipes pr
JOIN pizza_names pn ON pr.pizza_id = pn.pizza_id
JOIN pizza_toppings pt ON pr.toppings LIKE '%' || pt.topping_id || '%'
ORDER BY pn.pizza_name;
````

**Answer:**

| pizza_name | topping_name |
| ---------- | ------------ |
| Meatlovers | Bacon        |
| Meatlovers | BBQ Sauce    |
| Meatlovers | Beef         |
| Meatlovers | Cheese       |
| Meatlovers | Chicken      |
| Meatlovers | Mushrooms    |
| Meatlovers | Pepperoni    |
| Meatlovers | Salami       |
| Vegetarian | Bacon        |
| Vegetarian | BBQ Sauce    |
| Vegetarian | Cheese       |
| Vegetarian | Mushrooms    |
| Vegetarian | Onions       |
| Vegetarian | Peppers      |
| Vegetarian | Tomatoes     |
| Vegetarian | Tomato Sauce |

### 2. What was the most commonly added extra?

````sql
SELECT pn.pizza_name, pt.topping_name
FROM pizza_recipes pr
JOIN pizza_names pn ON pr.pizza_id = pn.pizza_id
JOIN pizza_toppings pt ON pr.toppings LIKE '%' || pt.topping_id || '%'
ORDER BY pn.pizza_name;
````

**Answer:**

| count      | topping_name |
| ---------- | ------------ |
| 4          | Bacon        |

### 3. What was the most common exclusion?

````sql
SELECT pn.pizza_name, pt.topping_name
FROM pizza_recipes pr
JOIN pizza_names pn ON pr.pizza_id = pn.pizza_id
JOIN pizza_toppings pt ON pr.toppings LIKE '%' || pt.topping_id || '%'
ORDER BY pn.pizza_name;
````

**Answer:**

| count      | topping_name     |
| ---------- | ------------     |
| 3          | Mushrooms        |

### 4. Generate an order item for each record in the customers_orders table in the format of one of the following:
### - Meat Lovers
### - Meat Lovers - Exclude Beef
### - Meat Lovers - Extra Bacon
### - Meat Lovers - Exclude Cheese, Bacon - Extra Mushroom, Peppers
````sql
SELECT co.order_id, pn.pizza_name || 
CASE WHEN co.exclusions <> '' THEN ' - Exclude ' || STRING_AGG(et.topping_name, ', ') ELSE '' END || 
CASE WHEN co.extras <> '' THEN ' - Extra ' || STRING_AGG(at.topping_name, ', ') ELSE '' END as order_item
FROM customer_orders co
JOIN pizza_names pn ON co.pizza_id = pn.pizza_id
LEFT JOIN pizza_toppings et ON co.exclusions LIKE '%' || et.topping_id || '%'
LEFT JOIN pizza_toppings at ON co.extras LIKE '%' || at.topping_id || '%'
GROUP BY co.order_id, pn.pizza_name, co.exclusions, co.extras;

````

**Answer:**

| order_id | order_item                                                                                           |
| -------- | ---------------------------------------------------------------------------------------------------- |
| 1        | Meatlovers                                                                                           |
| 2        | Meatlovers                                                                                           |
| 3        | Meatlovers                                                                                           |
| 3        | Vegetarian                                                                                           |
| 4        | Meatlovers                                                                                           |
| 4        | Vegetarian                                                                                           |
| 5        | Meatlovers - Extra Bacon                                                                             |
| 6        | Vegetarian                                                                                           |
| 7        | Vegetarian - Extra Bacon                                                                             |
| 8        | Meatlovers                                                                                           |
| 9        | Meatlovers - Extra Bacon, Chicken                                                                    |
| 10       | Meatlovers                                                                                           |
| 10       | Meatlovers - Exclude BBQ Sauce, BBQ Sauce, Mushrooms, Mushrooms - Extra Bacon, Cheese, Bacon, Cheese |

### 5. Generate an alphabetically ordered comma separated ingredient list for each pizza order from the customer_orders table and add a 2x in front of any relevant ingredients
### For example: "Meat Lovers: 2xBacon, Beef, ... , Salami"

````sql
SELECT co.order_id, pn.pizza_name || ': ' || STRING_AGG(pt.topping_name || CASE WHEN co.extras LIKE '%' || pt.topping_id || '%' THEN ' 2x' ELSE '' END, ', ' ORDER BY pt.topping_name) as ingredients
FROM customer_orders co
JOIN pizza_names pn ON co.pizza_id = pn.pizza_id
JOIN pizza_recipes pr ON co.pizza_id = pr.pizza_id
JOIN pizza_toppings pt ON pr.toppings LIKE '%' || pt.topping_id || '%'
GROUP BY co.order_id, pn.pizza_name;
````
**Answer:**

| order_id | ingredients                                                                                                                                                    |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | Meatlovers: BBQ Sauce, Bacon, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami                                                                              |
| 2        | Meatlovers: BBQ Sauce, Bacon, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami                                                                              |
| 3        | Meatlovers: BBQ Sauce, Bacon, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami                                                                              |
| 3        | Vegetarian: BBQ Sauce, Bacon, Cheese, Mushrooms, Onions, Peppers, Tomato Sauce, Tomatoes                                                                       |
| 4        | Meatlovers: BBQ Sauce, BBQ Sauce, Bacon, Bacon, Beef, Beef, Cheese, Cheese, Chicken, Chicken, Mushrooms, Mushrooms, Pepperoni, Pepperoni, Salami, Salami       |
| 4        | Vegetarian: BBQ Sauce, Bacon, Cheese, Mushrooms, Onions, Peppers, Tomato Sauce, Tomatoes                                                                       |
| 5        | Meatlovers: BBQ Sauce, Bacon 2x, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami                                                                           |
| 6        | Vegetarian: BBQ Sauce, Bacon, Cheese, Mushrooms, Onions, Peppers, Tomato Sauce, Tomatoes                                                                       |
| 7        | Vegetarian: BBQ Sauce, Bacon 2x, Cheese, Mushrooms, Onions, Peppers, Tomato Sauce, Tomatoes                                                                    |
| 8        | Meatlovers: BBQ Sauce, Bacon, Beef, Cheese, Chicken, Mushrooms, Pepperoni, Salami                                                                              |
| 9        | Meatlovers: BBQ Sauce, Bacon 2x, Beef, Cheese, Chicken 2x, Mushrooms, Pepperoni, Salami                                                                        |
| 10       | Meatlovers: BBQ Sauce, BBQ Sauce, Bacon, Bacon 2x, Beef, Beef, Cheese 2x, Cheese, Chicken, Chicken, Mushrooms, Mushrooms, Pepperoni, Pepperoni, Salami, Salami |

### 6. What is the total quantity of each ingredient used in all delivered pizzas sorted by most frequent first?

````sql
SELECT pt.topping_name, COUNT(*) as count
FROM runner_orders ro
JOIN customer_orders co ON ro.order_id = co.order_id AND ro.cancellation = ''
JOIN pizza_recipes pr ON co.pizza_id = pr.pizza_id
JOIN pizza_toppings pt ON pr.toppings LIKE '%' || pt.topping_id || '%'
GROUP BY pt.topping_name
ORDER BY count DESC;
````
**Answer:**

| topping_name | count |
| ------------ | ----- |
| Bacon        | 12    |
| Mushrooms    | 12    |
| BBQ Sauce    | 12    |
| Cheese       | 12    |
| Pepperoni    | 9     |
| Chicken      | 9     |
| Salami       | 9     |
| Beef         | 9     |
| Tomato Sauce | 3     |
| Onions       | 3     |
| Tomatoes     | 3     |
| Peppers      | 3     |

## D. Pricing and Ratings

### 1. If a Meat Lovers pizza costs $12 and Vegetarian costs $10 and there were no charges for changes - how much money has Pizza Runner made so far if there are no delivery fees?

````sql
SELECT COUNT(*) AS total_pizzas_ordered
FROM customer_orders;
````

**Answer:**

| total_money|
|---|
|138|

### 2. What if there was an additional $1 charge for any pizza extras?
### - Add cheese is $1 extra

````sql
SELECT COUNT(*) AS total_pizzas_ordered
FROM customer_orders;
````

**Answer:**

| total_money|
|---|
|150|

### 3. The Pizza Runner team now wants to add an additional ratings system that allows customers to rate their runner, how would you design an additional table for this new dataset - generate a schema for this new table and insert your own data for ratings for each successful customer order between 1 to 5.

````sql
  CREATE TABLE customer_ratings (
  "rating_id" SERIAL PRIMARY KEY,
  "order_id" INTEGER,
  "customer_id" INTEGER,
  "runner_id" INTEGER,
  "rating" INTEGER CHECK(rating >= 1 AND rating <= 5),
  "rating_time" TIMESTAMP
);

INSERT INTO customer_ratings (order_id, customer_id, runner_id, rating, rating_time)
VALUES 
  (1, 101, 1, 5, '2024-06-13 08:31:59'),
  (2, 102, 2, 4, '2024-06-14 08:31:59'),
  (3, 103, 3, 3, '2024-06-15 08:31:59');
````

**Answer:**

| rating_id | order_id | customer_id | runner_id | rating | rating_time              |
| --------- | -------- | ----------- | --------- | ------ | ------------------------ |
| 1         | 1        | 101         | 1         | 5      | 2024-06-13T08:31:59.000Z |
| 2         | 2        | 102         | 2         | 4      | 2024-06-14T08:31:59.000Z |
| 3         | 3        | 103         | 3         | 3      | 2024-06-15T08:31:59.000Z |
| 4         | 4        | 104         | 4         | 5      | 2024-08-12T08:31:59.000Z |
| 5         | 5        | 105         | 5         | 3      | 2024-09-11T08:31:59.000Z |

### 4. Using your newly generated table - can you join all of the information together to form a table which has the following information for successful deliveries?

````sql
SELECT 
  co.order_id,
  co.customer_id,
  co.pizza_id,
  pn.pizza_name,
  pr.toppings,
  co.exclusions,
  co.extras,
  co.order_time,
  ro.runner_id,
  r.registration_date,
  ro.pickup_time,
  ro.distance,
  ro.duration,
  cr.rating,
  cr.rating_time
FROM 
  customer_orders co
JOIN 
  runner_orders ro ON co.order_id = ro.order_id
JOIN 
  runners r ON ro.runner_id = r.runner_id
JOIN 
  pizza_names pn ON co.pizza_id = pn.pizza_id
JOIN 
  pizza_recipes pr ON co.pizza_id = pr.pizza_id
LEFT JOIN 
  customer_ratings cr ON co.order_id = cr.order_id AND co.customer_id = cr.customer_id AND ro.runner_id = cr.runner_id
WHERE 
  ro.cancellation = '';

````
**Answer:**

| order_id | customer_id | pizza_id | pizza_name | toppings                | exclusions | extras | order_time               | runner_id | registration_date        | pickup_time         | distance | duration | rating | rating_time              |
| -------- | ----------- | -------- | ---------- | ----------------------- | ---------- | ------ | ------------------------ | --------- | ------------------------ | ------------------- | -------- | -------- | ------ | ------------------------ |
| 5        | 104         | 1        | Meatlovers | 1, 2, 3, 4, 5, 6, 8, 10 |            | 1      | 2020-01-08T21:00:29.000Z | 3         | 2021-01-08T00:00:00.000Z | 2020-01-08 21:10:57 | 10       | 15       |        |                          |
| 10       | 104         | 1        | Meatlovers | 1, 2, 3, 4, 5, 6, 8, 10 | 2, 6       | 1, 4   | 2020-01-11T18:34:49.000Z | 1         | 2021-01-01T00:00:00.000Z | 2020-01-11 18:50:20 | 10       | 10       |        |                          |
| 3        | 102         | 1        | Meatlovers | 1, 2, 3, 4, 5, 6, 8, 10 |            |        | 2020-01-02T23:51:23.000Z | 1         | 2021-01-01T00:00:00.000Z | 2020-01-03 00:12:37 | 13.4     | 20       |        |                          |
| 4        | 103         | 1        | Meatlovers | 1, 2, 3, 4, 5, 6, 8, 10 |            |        | 2020-01-04T13:23:46.000Z | 2         | 2021-01-03T00:00:00.000Z | 2020-01-04 13:53:03 | 23.4     | 40       |        |                          |
| 8        | 102         | 1        | Meatlovers | 1, 2, 3, 4, 5, 6, 8, 10 |            |        | 2020-01-09T23:54:33.000Z | 2         | 2021-01-03T00:00:00.000Z | 2020-01-10 00:15:02 | 23.4     | 15       |        |                          |
| 10       | 104         | 1        | Meatlovers | 1, 2, 3, 4, 5, 6, 8, 10 |            |        | 2020-01-11T18:34:49.000Z | 1         | 2021-01-01T00:00:00.000Z | 2020-01-11 18:50:20 | 10       | 10       |        |                          |
| 2        | 101         | 1        | Meatlovers | 1, 2, 3, 4, 5, 6, 8, 10 |            |        | 2020-01-01T19:00:52.000Z | 1         | 2021-01-01T00:00:00.000Z | 2020-01-01 19:10:54 | 20       | 27       |        |                          |
| 1        | 101         | 1        | Meatlovers | 1, 2, 3, 4, 5, 6, 8, 10 |            |        | 2020-01-01T18:05:02.000Z | 1         | 2021-01-01T00:00:00.000Z | 2020-01-01 18:15:34 | 20       | 32       | 5      | 2024-06-13T08:31:59.000Z |
| 4        | 103         | 1        | Meatlovers | 1, 2, 3, 4, 5, 6, 8, 10 |            |        | 2020-01-04T13:23:46.000Z | 2         | 2021-01-03T00:00:00.000Z | 2020-01-04 13:53:03 | 23.4     | 40       |        |                          |
| 3        | 102         | 2        | Vegetarian | 4, 6, 7, 9, 11, 12      |            |        | 2020-01-02T23:51:23.000Z | 1         | 2021-01-01T00:00:00.000Z | 2020-01-03 00:12:37 | 13.4     | 20       |        |                          |
| 4        | 103         | 2        | Vegetarian | 4, 6, 7, 9, 11, 12      |            |        | 2020-01-04T13:23:46.000Z | 2         | 2021-01-03T00:00:00.000Z | 2020-01-04 13:53:03 | 23.4     | 40       |        |                          |
| 7        | 105         | 2        | Vegetarian | 4, 6, 7, 9, 11, 12      |            | 1      | 2020-01-08T21:20:29.000Z | 2         | 2021-01-03T00:00:00.000Z | 2020-01-08 21:30:45 | 25       | 25       |        |    

### 5. If a Meat Lovers pizza was $12 and Vegetarian $10 fixed prices with no cost for extras and each runner is paid $0.30 per kilometre traveled - how much money does Pizza Runner have left over after these deliveries?

````sql
SELECT SUM(
  CASE 
    WHEN pn.pizza_name = 'Meatlovers' THEN 12
    WHEN pn.pizza_name = 'Vegetarian' THEN 10
    ELSE 0
  END
) - SUM(0.30 * CAST(ro.distance AS FLOAT)) as money_left_over
FROM customer_orders co
JOIN pizza_names pn ON co.pizza_id = pn.pizza_id
JOIN runner_orders ro ON co.order_id = ro.order_id AND ro.cancellation = '';
````

**Answer:**
| money_left_over|
|---|
|73.38000000000001|
