
# Joining Data in SQL (Dtata Camp)

## Inner Join

Throughout this course, you'll be working with the countries database containing information about the most populous world cities as well as country-level economic data, population data, and geographic data. This countries database also contains information on languages spoken in each country.

You can see the different tables in this database by clicking on the tabs on the bottom right below query.sql. Click through them to get a sense for the types of data that each table contains before you continue with the course! Take note of the fields that appear to be shared across the tables.

Recall from the video the basic syntax for an INNER JOIN, here including all columns in both tables:

SELECT *
FROM left_table
INNER JOIN right_table
ON left_table.id = right_table.id;
You'll start off with a SELECT statement and then build up to an inner join with the cities and countries tables. Let's get to it!

```
SELECT * 
FROM cities
INNER JOIN countries
on cities.country_code = countries.code
```

```
SELECT cities.name AS city, countries.name AS country, region
FROM cities 
INNER JOIN countries 
ON cities.country_code = countries.code
```

```
SELECT c.code AS country_code, name, year, inflation_rate
FROM countries AS c
INNER JOIN economies AS e
ON c.code = e.code;

```



The ability to combine multiple joins in a single query is a powerful feature of SQL, e.g:

SELECT *
FROM left_table
INNER JOIN right_table
ON left_table.id = right_table.id
INNER JOIN another_table
ON left_table.id = another_table.id;
As you can see here it becomes tedious to continually write long table names in joins. This is when it becomes useful to alias each table using the first letter of its name (e.g. countries AS c)! It is standard practice to alias in this way and, if you choose to alias tables or are asked to specifically for an exercise in this course, you should follow this protocol.


```
SELECT c.code, c.name, c.region, year, fertility_rate
FROM countries AS c
INNER JOIN populations AS p
on c.code = p.country_code;
```

```
SELECT c.code, name, region, e.year, fertility_rate, unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
INNER JOIN economies AS e
ON p.country_code = e.code;
```

```
SELECT c.code, name, region, e.year, fertility_rate, unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
INNER JOIN economies AS e
ON c.code = e.code and p.year = e.year;
```

```
SELECT c.name AS country, continent, l.name AS language, official
FROM countries AS c
INNER JOIN languages AS l
using(code);
```

```
SELECT p1.country_code, 
       p1.size AS size2010,
       p2.size AS size2015
FROM populations AS p1
INNER JOIN populations AS p2
ON  p1.country_code = p2.country_code;
```
```
SELECT p1.country_code, 
       p1.size AS size2010,
       p2.size AS size2015
FROM populations AS p1
INNER JOIN populations AS p2
ON  p1.country_code = p2.country_code
    AND p1.year = p2.year - 5;
```

```
### Wrong Code
SELECT p1.country_code, p1.size AS size2010, p2.size AS size2015,((size2015-size2010)/size2010) AS growth_perc
FROM populations AS p1
INNER JOIN populations AS p2
ON p1.country_code = p2.country_code and p1.year = p2.year -5;
```

```
SELECT p1.country_code, p1.size AS size2010, p2.size AS size2015,((p2.size-p1.size)/p1.size) AS growth_perc
FROM populations AS p1
INNER JOIN populations AS p2
ON p1.country_code = p2.country_code and p1.year = p2.year -5;
```

```
SELECT name, continent, code, surface_area,
        -- first case
    CASE WHEN surface_area > 2000000 THEN 'large'
        -- second case
        WHEN surface_area > 350000 THEN 'medium'
        -- else clause + end
        ELSE 'small' END
        AS geosize_group
FROM countries;
```

```
SELECT country_code, size,
    CASE WHEN size > 50000000 THEN 'large'
        WHEN size > 1000000 THEN 'medium'
        ELSE 'small' END
        AS popsize_group
FROM populations
WHERE year = 2015;
```

```
SELECT country_code, size,
    CASE WHEN size > 50000000 THEN 'large'
        WHEN size > 1000000 THEN 'medium'
        ELSE 'small' END
        AS popsize_group
INTO pop_plus
FROM populations
WHERE year = 2015;

SELECT *
FROM pop_plus
```


```
SELECT country_code, size,
    CASE WHEN size > 50000000 THEN 'large'
        WHEN size > 1000000 THEN 'medium'
        ELSE 'small' END
        AS popsize_group
INTO pop_plus
FROM populations
WHERE year = 2015;

SELECT name, continent, geosize_group, popsize_group
FROM countries_plus AS c
INNER JOIN pop_plus AS p
ON p.country_code = c.code
ORDER BY geosize_group;
```









