# Restaurant_Analysis_with_Joins
Source: https://www.kaggle.com/datasets/vainero/restaurants-customers-orders-dataset?select=orders.csv

``` sql

-- Restaurants, Members and Orders Analysis
-- Source: https://www.kaggle.com/datasets/vainero/restaurants-customers-orders-dataset?select=orders.csv



------------------------------------------------------------------------------------------------------------------------------------
-- In which City the members do the most orders?

SELECT *
FROM Restaurants_Analysis..monthly_member_totals

SELECT *
FROM Restaurants_Analysis..cities

SELECT City, 
	COUNT(City) AS OrderCount
FROM Restaurants_Analysis..monthly_member_totals
GROUP BY city
ORDER BY OrderCount DESC


------------------------------------------------------------------------------------------------------------------------------------
-- Which period of the day the members do the most orders?

SELECT *
FROM Restaurants_Analysis..orders

SELECT DISTINCT(LEFT(hour,2)) as Hour_of_the_Day,
		COUNT(LEFT(hour,2)) Count_of_the_Hours
FROM Restaurants_Analysis..orders
GROUP BY LEFT(hour,2)
ORDER BY COUNT(LEFT(hour,2)) DESC


------------------------------------------------------------------------------------------------------------------------------------
-- What is the most ordered meal types in restaurants in each city?

SELECT *
FROM Restaurants_Analysis..meals

SELECT *
FROM Restaurants_Analysis..meal_types

SELECT *
FROM Restaurants_Analysis..cities

SELECT *
FROM Restaurants_Analysis..order_details

SELECT *
FROM Restaurants_Analysis..orders

SELECT *
FROM Restaurants_Analysis..restaurants


SELECT meal_id,
		COUNT(meal_id) AS Count_of_Meals
FROM Restaurants_Analysis..order_details
GROUP BY meal_id
ORDER BY Count_of_Meals DESC



SELECT cities.city,
		meal_types.meal_type,
		COUNT(meal_types.meal_type) AS Count_of_the_Types
	
FROM Restaurants_Analysis..order_details
		JOIN Restaurants_Analysis..meals ON
			order_details.meal_id = meals.meal_id
				JOIN Restaurants_Analysis..meal_types ON
					meals.meal_type_id = meal_types.meal_type_id
						JOIN Restaurants_Analysis..orders ON
							meals.restaurant_id = orders.restaurant_id
								JOIN Restaurants_Analysis..restaurants ON
									orders.restaurant_id = restaurants.restaurant_id
										JOIN Restaurants_Analysis..cities ON
											restaurants.city_id = cities.city_id
GROUP BY cities.city, meal_types.meal_type
ORDER BY Count_of_the_Types DESC, cities.city
									


------------------------------------------------------------------------------------------------------------------------------------
--What is the ratio of the orders in cities with the most Italian restaurants?

SELECT *
FROM Restaurants_Analysis..restaurants

SELECT *
FROM Restaurants_Analysis..restaurant_types

SELECT *
FROM Restaurants_Analysis..orders

SELECT *
FROM Restaurants_Analysis..orders
	JOIN Restaurants_Analysis..restaurants ON
		orders.restaurant_id = restaurants.restaurant_id
			JOIN Restaurants_Analysis..restaurant_types ON
					restaurants.restaurant_type_id = restaurant_types.restaurant_type_id


SELECT *
FROM Restaurants_Analysis..orders
	JOIN Restaurants_Analysis..restaurants ON
		orders.restaurant_id = restaurants.restaurant_id
			JOIN Restaurants_Analysis..restaurant_types ON
					restaurants.restaurant_type_id = restaurant_types.restaurant_type_id


SELECT Restaurants_Analysis..restaurant_types.restaurant_type,
		COUNT(restaurant_types.restaurant_type) AS Count_of_each_Restaurant_Types
FROM Restaurants_Analysis..orders
	JOIN Restaurants_Analysis..restaurants ON
		orders.restaurant_id = restaurants.restaurant_id
			JOIN Restaurants_Analysis..restaurant_types ON
					restaurants.restaurant_type_id = restaurant_types.restaurant_type_id
GROUP BY restaurant_types.restaurant_type



SELECT COUNT(restaurant_types.restaurant_type) AS Count_of_All_Restaurant_Types
FROM Restaurants_Analysis..orders
	JOIN Restaurants_Analysis..restaurants ON
		orders.restaurant_id = restaurants.restaurant_id
			JOIN Restaurants_Analysis..restaurant_types ON
					restaurants.restaurant_type_id = restaurant_types.restaurant_type_id


SELECT SUM(CASE WHEN restaurant_types.restaurant_type = 'Italian' THEN 1 
			ELSE 0 
			END) AS Count_of_Italian_Restaurants,
		COUNT(restaurant_types.restaurant_type) AS Count_of_All_restaurants,
		ROUND(
			CAST(SUM(CASE WHEN restaurant_types.restaurant_type = 'Italian' THEN 1 
						ELSE 0 
						END) AS float) / CAST(COUNT(restaurant_types.restaurant_type) AS float), 3) AS Ratio
FROM Restaurants_Analysis..orders
	JOIN Restaurants_Analysis..restaurants ON
		orders.restaurant_id = restaurants.restaurant_id
			JOIN Restaurants_Analysis..restaurant_types ON
					restaurants.restaurant_type_id = restaurant_types.restaurant_type_id







------------------------------------------------------------------------------------------------------------------------------------
--Which cities have the most vegan meals?

SELECT *
FROM Restaurants_Analysis..order_details

SELECT *
FROM Restaurants_Analysis..meals

SELECT *
FROM Restaurants_Analysis..meal_types

SELECT *
FROM Restaurants_Analysis..orders

SELECT *
FROM Restaurants_Analysis..restaurants

SELECT *
FROM Restaurants_Analysis..cities



SELECT *
FROM Restaurants_Analysis..order_details
		JOIN Restaurants_Analysis..meals ON
			order_details.meal_id = meals.meal_id
				JOIN Restaurants_Analysis..meal_types ON
					meals.meal_type_id = meal_types.meal_type_id
						JOIN Restaurants_Analysis..orders ON
							order_details.order_id = orders.id
								JOIN Restaurants_Analysis..restaurants ON
									orders.restaurant_id = restaurants.restaurant_id
										JOIN Restaurants_Analysis..cities ON
											restaurants.city_id = cities.city_id



SELECT meal_types.meal_type,
		cities.city,
		COUNT(meal_types.meal_type_id) AS Count_of_Vegan_Meals
FROM Restaurants_Analysis..order_details
		JOIN Restaurants_Analysis..meals ON
			order_details.meal_id = meals.meal_id
				JOIN Restaurants_Analysis..meal_types ON
					meals.meal_type_id = meal_types.meal_type_id
						JOIN Restaurants_Analysis..orders ON
							order_details.order_id = orders.id
								JOIN Restaurants_Analysis..restaurants ON
									orders.restaurant_id = restaurants.restaurant_id
										JOIN Restaurants_Analysis..cities ON
											restaurants.city_id = cities.city_id
WHERE meal_types.meal_type = 'Vegan'
GROUP BY meal_types.meal_type, cities.city
ORDER BY COUNT(meal_types.meal_type_id) DESC

------------------------------------------------------------------------------------------------------------------------------------
--What is the difference in the range price of the hot or cold meal?

SELECT *
FROM Restaurants_Analysis..order_details

SELECT *
FROM Restaurants_Analysis..meals

SELECT *
FROM Restaurants_Analysis..order_details
		JOIN Restaurants_Analysis..meals ON
			order_details.meal_id = meals.meal_id

SELECT hot_cold,
		COUNT(hot_cold) AS Count_of_Hot_or_Cold_Meals, 
		ROUND(AVG(price), 3) AS Average_Price
FROM Restaurants_Analysis..order_details
		JOIN Restaurants_Analysis..meals ON
			order_details.meal_id = meals.meal_id
GROUP BY hot_cold



```
