## :round_pushpin: Objetivo general

El **objetivo** del proyecto es realizar un análisis exploratorio de la base de datos **Sakila** mediante consultas básicas y avanzadas en MySQL.

***Sakila es una base de datos relacional preinstalada en MySQL (Software de código abierto desarrollado por Oracle), que simula una empresa de alquiler de películas.***

## :gear: Herramientas utilizadas

Las consultas se realizarón mediante:

- **MySQL:** Sistema de gestión de base de datos relacional (SGBDR)
- **MySQL Workbench:** Herramienta gráfica diseñada para interactuar con MySQL. 

## :open_file_folder: Estructura de la base de datos

El siguiente diagrama entidad - relación (DER) nos permite identificar como estan estructurados los datos de la empresa y como se relacionan entre sí: 

![DER](https://github.com/Johanna-Rojas/BD_SAKILA/blob/main/SAKILA_ER_Diagram.png)

*MySQL Workbench posibilita la visualización y diseño de los diagramas ER, que se puede descargar de la siguiente manera:*

1. Crear la conexión con la base de datos.
2. En la barra de menú ir a "Database" y seleccionar la opción "Reverse Engineer".
3. Incluir los objetos deseados y configurar las opciones adicionales.
4. Inicia el proceso de generación del diagrama y se abre en una pestaña.
5. Señalar en la barra de menú "File" -> opción "Save Model As" o "Export".
6. Guardar en formato .mwb o escoger el formato a exportar(en este caso PNG).

:warning: **Importante: El proceso de reconocimiento y comprensión de la estructura interna de una base de datos es fundamental para la manipulación de la misma.**

Para una descripción detallada de la base de datos ir a la documentación oficial de MySQL: [Sakila Sample Database](https://dev.mysql.com/doc/sakila/en/)

## :mag: Objetivos de análisis

El análisis de la base de datos se dividio en 2 secciones:

**1. Análisis de alquiler y clientes:**

- Identificar las películas más rentables.
- Determinar los generos de películas más populares.
- Clasificar los actores estrella de las peliculas más rentables.
- Identificar las horas y días pico de alquiler.
- Analizar la tendencia de alquiler e ingresos a lo largo del tiempo.
- Clasificar los mejores clientes.

**2. Análisis de inventario:**

- Reconocer las películas de menor rotación.
- Identificar las películas con menor Stock.

## :bookmark_tabs: Consultas en MySQL

A continuación se presentan algunas de las consultas realizadas, si deseas visualizar el ***Script completo***, ir al siguiente enlace: [Queries DB Sakila / MySQL](https://github.com/Johanna-Rojas/BD_SAKILA/blob/main/QUERIES)

~~~
----------------------------------------------------------------------------------------------------
-- ANÁLISIS DE ALQUILER Y CLIENTES
----------------------------------------------------------------------------------------------------
-- TOP 10 / Películas más rentables

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
    r.return_date IS NOT NULL
GROUP BY
    f.film_id
ORDER BY
    total_revenue DESC
LIMIT 10;

-- Tendencia de alquiler e ingresos a lo largo del tiempo

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

-- TOP 10 / Mejores clientes

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

----------------------------------------------------------------------------------------------------
-- ANÁLISIS DE INVENTARIO
----------------------------------------------------------------------------------------------------

-- Películas de menor rotación

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

-- Películas con menor Stock (Excluyendo las de menor rotación)

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

~~~

## :computer: Contribuciones

Este proyecto genera un espacio para aprender y crecer. Siéntete libre de explorar el código, hacer preguntas y proponer mejoras. 
Tus comentarios son muy valiosos. :dizzy:

***¡Gracias por tu interés en el proyecto!***