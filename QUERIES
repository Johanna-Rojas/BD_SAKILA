USE sakila;
-------------------------------------------------------------------------------------------------------------------
--RENTAL AND CUSTOMER ANALYSIS
-------------------------------------------------------------------------------------------------------------------

-- TOP 10 / MOST RENTABLE FILMS
CREATE VIEW top10_films AS
SELECT
    f.film_id,
    f.title,
    SUM(IF(p.amount > 0, p.amount, 0)) AS total_revenue,
    COUNT(DISTINCT i.inventory_id) AS total_copies_rented
FROM
    payment p
INNER JOIN rental r ON p.rental_id = r.rental_id
INNER JOIN inventory i ON r.inventory_id = i.inventory_id
INNER JOIN film f ON i.film_id = f.film_id
WHERE
    -- Excluir devoluciones
    r.return_date IS NOT NULL
GROUP BY
    f.film_id
ORDER BY
    total_revenue DESC
LIMIT 10;

-- TOP 10 / MORE RENTALS BY CATEGORY
SELECT
    c.name AS category_name,
    COUNT(*) AS total_rentals
FROM
    category c
INNER JOIN film_category fc ON c.category_id = fc.category_id
INNER JOIN inventory i ON fc.film_id = i.film_id
INNER JOIN rental r ON i.inventory_id = r.inventory_id
GROUP BY
    c.name
ORDER BY
    total_rentals DESC
LIMIT 10;

-- POPULAR ACTORS
CREATE OR REPLACE VIEW popular_actors AS
SELECT
	fa.actor_id,
	CONCAT(va.first_name,' ', va.last_name) AS actor_name,
    COUNT(*) AS films_quantity
FROM 
	film_actor fa
INNER JOIN actor_info va ON fa.actor_id = va.actor_id
GROUP BY
	fa.actor_id
HAVING COUNT(*) > 29
ORDER BY
	films_quantity DESC

-- MOST POPULAR ACTORS IN THE MOST RENTED FILMS
SELECT
	vf.title AS title_film,
    va.actor_name AS star_actor
FROM
	film_actor fa
INNER JOIN top10_films vf ON fa.film_id = vf.film_id
INNER JOIN popular_actors va ON fa.actor_id = va.actor_id;

-- PEAK RENTAL DAY AND TIME
SELECT
    /*MONTH(rental_date) AS month_rent,*/
    DAYNAME(rental_date) AS day_week,
    HOUR(rental_date) AS time_rent,
    COUNT(*) AS total_rent
FROM
    rental
GROUP BY
	/*month_rent,*/day_week, time_rent
ORDER BY
    total_rent DESC

-- RENT AND INCOME TREND
SELECT
    YEAR(payment_date) AS year_rent,
    MONTH(payment_date) AS month_rent,
    COUNT(*) AS total_rent,
    SUM(amount) AS total_revenue
FROM
    payment
GROUP BY
	year_rent, month_rent
ORDER BY
    total_revenue DESC

-- TOP 10 / STAR CUSTOMERS
SELECT
	CONCAT(c.first_name,' ', c.last_name) AS Customers_Name,
    COUNT(*) AS Total_Rentals,
    SUM(p.amount) AS Total_Revenue
FROM
    customer c
INNER JOIN rental r ON c.customer_id = r.customer_id
INNER JOIN payment p ON r.rental_id = p.rental_id
GROUP BY
    c.customer_id
ORDER BY
    total_rentals DESC
LIMIT 10;

-------------------------------------------------------------------------------------------------------------------
-- INVENTORY ANALYSIS
-------------------------------------------------------------------------------------------------------------------

-- LOWER ROTATION FILMS
SELECT
    f.film_id,
    f.title,
    COUNT(*) AS total_copies_rented
FROM
    rental r
INNER JOIN inventory i ON r.inventory_id = i.inventory_id
INNER JOIN film f ON i.film_id = f.film_id
GROUP BY
    f.film_id
HAVING
	total_copies_rented < (0.2 * (SELECT MAX(total_copies) 
								  FROM (SELECT COUNT(*) AS total_copies
										FROM rental r
                                        INNER JOIN inventory i ON r.inventory_id = i.inventory_id
                                        GROUP BY i.film_id) AS subquery))
ORDER BY
	total_copies_rented

-- MOVIES WITH LOWER STOCK (Does not include low rotation films)
SELECT 
    f.film_id,
    f.title AS title_film,
    COUNT(i.inventory_id) AS copies_stock
FROM
    film f
INNER JOIN inventory i ON f.film_id = i.film_id
WHERE
	f.title NOT IN (SELECT title FROM lower_rotation_films)
GROUP BY 
	f.film_id
HAVING 
	copies_stock < 3
ORDER BY 
	copies_stock ASC