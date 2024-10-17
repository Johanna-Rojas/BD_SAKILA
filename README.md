## :point_right: Introducción

El **objetivo** del proyecto es realizar un análisis exploratorio de la base de datos Sakila mediante consultas básicas y avanzadas en MySQL.

***Sakila es una base de datos relacional preinstalada en MySQL, que simula una empresa de alquiler de películas.***

## :wrench: Tecnologías utilizadas

Las consultas se realizarón mediante las siguientes herramientas:

- **MySQL:** Sistema de gestión de base de datos relacional (SGBDR)
- **MySQL Workbench:** Herramienta gráfica diseñada para interactuar con MySQL. 

## :open_file_folder: Estructura de la base de datos

El siguiente diagrama entidad - relación (DER) nos permite saber como estan estructurados los datos de la empresa Sakila y como se relacionan entre sí: 

![DER]()

MySQL Workbench posibilita la visualización y diseño de los diagramas ER. 
Para la base de datos Sakila y en general, el diagrama se descarga de la siguiente manera:

1. Crear la conexión con la base de datos.
2. En la barra de menú ir a "Database" y seleccionar la opción "Reverse Engineer".
3. Incluir los objetos deseados y configurar las opciones adicionales.
4. Inicia el proceso de generación del diagrama y se abre en una pestaña.
5. Señalar en la barra de menú "File" -> opción "Save Model As" o "Export".
6. Guardar en formato .mwb o escoger el formato a exportar(en este caso PNG).

***Importante: El proceso de reconocimiento y comprensión de la esctructura interna de una base de datos es fundamental para la manipulación de la misma.***

## :mag: Objetivos de análisis

El análisis de la base de datos de la empresa Sakila se dividio en 3 secciones:

1. Análisis de Películas y clientes:

- Identificar las películas más rentables.
- Determinar los generos de películas más populares.
- Relacionar el genero y calificación.

2. Análisis de alquiler y venta:

- Identificar las horas y días pico de alquiler.
- Analizar la tendencia de alquiler, ventas e ingresos a lo largo del tiempo.
- Clasificar los mejores clientes.

3. Análisis de inventario:

- Reconocer las películas de menor rotación.
- Identificar las películas con poca existencia.

## :bookmark_tabs: Consultas SQLite

A continuación se presentan algunas de las consultas realizadas, si deseas visualizar el ***Script completo***, ir al siguiente enlace: [Consultas DB Sakila con MySQL]()

~~~
----------------------------------------------------------------------------------------------------
-- 
----------------------------------------------------------------------------------------------------



----------------------------------------------------------------------------------------------------
-- 
----------------------------------------------------------------------------------------------------


~~~