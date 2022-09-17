---
id: 922g6erhxi24imyvhwditce
title: Basic Operators
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: Basic operators
updated_imported: '2022-01-14T09:26:25.000Z'
created_imported: '2022-01-11T09:45:14.000Z'
---

[TOC]






















# Summary on several keyword in SQL
![24bebf202b748b6c9cc65913fc3cb184.png](/assets/24bebf202b748b6c9cc65913fc3cb184-6n2jrn90mzay.png)

```sql
FROM -- Specifies starting set of rows to work with

JOIN -- Merges in data from additinal tables

WHERE -- filter the set of rows

GROUP BY - Groups rows by a unique set of values

HAVING - Filters the set of groups or aggregation function
```






```sql
-- create new table
CREATE TABLE cities (
	name VARCHAR(50), 
  	country VARCHAR(50),
  	population INTEGER,
  	area INTEGER
);

--insert row to table
INSERT INTO cities (name, country, population, area)
VALUES ('Tokyo', 'Japan', 38505000, 8223);

-- insert mutiple rows to table
INSERT INTO cities (name, country, population, area)
VALUES 
	('Delhi', 'India', 28125000, 2240),
    ('Shanghai', 'China', 22125000, 4015),
	('Sao Paulo', 'Brazil', 20935000, 3043);
```



```sql
-- calculated columns
SELECT
	name, (price * units_sold) AS revenue
FROM
	phones

```




# String Operators and Functions
![1fda26d558f2beffc6dbc02a95aa8281.png](/assets/1fda26d558f2beffc6dbc02a95aa8281-jxyjb8ib7izd.png)

```sql
SELECT name || country FROM cities;

SELECT name || ', ' || country FROM cities;

SELECT name || ', ' || country AS location FROM cities;

SELECT CONCAT(name, country) AS location FROM cities;

SELECT CONCAT(name, ', ', country) AS location FROM cities;

SELECT
  CONCAT(UPPER(name), ', ', UPPER(country)) AS location
FROM
  cities;

SELECT
  UPPER(CONCAT(name, ', ', country)) AS location
FROM
  cities;
```


# Filtering Records
Order in the query
![3d10682d2643184ad27a44d2c53ea991.png](/assets/3d10682d2643184ad27a44d2c53ea991-biuuags1n4ic.png)

```sql

--Filter
SELECT
	name, area
FROM
	cities
WHERE
	area > 4000;


SELECT
	name, manufacturer
FROM
	phones 
WHERE
	manufacturer IN ('Apple', 'Samsung');

--Calculations in WHERE
SELECT
    name, price * units_sold AS total_revenue
FROM
    phones
WHERE
    price * units_sold > 1000000;

--Update
UPDATE phones
SET units_sold = 8543
WHERE name = 'N8';

--Delete
DELETE FROM phones
WHERE manufacturer = 'Samsung';


```


# Primary key

```sql

-- auto generated ID in PostgreSql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50)
);

INSERT INTO users (username)
VALUES
	('monahan93'),
    ('pferrer'),
    ('si93onis'),
    ('99stroman');
	
-- Foreign Key
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50)
);

INSERT INTO users (username)
VALUES
	('monahan93'),
    ('pferrer'),
    ('si93onis'),
    ('99stroman');

CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id)
);

INSERT INTO photos (url, user_id)
VALUES
	('http://one.jpg', 4);
	
INSERT INTO photos (url, user_id)
VALUES
	('http://two.jpg', 1),
    ('http://25.jpg', 1),
  	('http://36.jpg', 1),
  	('http://754.jpg', 2),
 	('http://35.jpg', 3),
	('http://256.jpg', 4);

-- if the user_id is not founds, it will throw an error
INSERT INTO photos (url, user_id)
VALUES ('http://jpg', 9999);

-- this works
INSERT INTO photos (url, user_id)
VALUES ('http://jpg', NULL);


```


# Constraints Around Deletion
![f7479db1a4ea47c6df24142971174907.png](/assets/f7479db1a4ea47c6df24142971174907-taxo4p04v7k4.png)

```sql
-- Remove related data when deleing

CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE
);

INSERT INTO photos (url, user_id)
VALUES
	('http://one.jpg', 4),
	('http://two.jpg', 1),
  ('http://25.jpg', 1),
  ('http://36.jpg', 1),
  ('http://754.jpg', 2),
  ('http://35.jpg', 3),
  ('http://256.jpg', 4);

DELETE FROM users
WHERE id = 1;

SELECT * FROM photos;


-- set related foreign key to Null
CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id) ON DELETE SET NULL
);

INSERT INTO photos (url, user_id)
VALUES
  ('http:/one.jpg', 4),
  ('http:/754.jpg', 2),
  ('http:/35.jpg', 3),
  ('http:/256.jpg', 4);

DELETE FROM users
WHERE id = 4;
```



# Relating Records with Joins

## what Joins do ?
![e16fa6d4aee893bc6f20113a844b6807.png](/assets/e16fa6d4aee893bc6f20113a844b6807-9pom1hj1ma7h.png)

- Produces values by merging together rows from different related tables
- Use a join most times when we need to find data hat involves multiple resources

## 4 Types of Join

### Inner join

```sql
SELECT url, username
FROM photos
JOIN users ON users.id = photos.user_id;
```

![daa013903328a7fa3e3b53e891661cce.png](/assets/daa013903328a7fa3e3b53e891661cce-fw73ephg1o7k.png)

### Left Outer Join
```sql
SELECT url, username
FROM photos
LEFT JOIN users ON users.id = photos.user_id;
```

![0d4560bd561b468922d30c40d6730df1.png](/assets/0d4560bd561b468922d30c40d6730df1-ogf7xufynrk4.png)

### Right Outer Join
```sql
SELECT url, username
FROM photos
RIGHT JOIN users ON users.id = photos.user_id;
```
![2379959538085a767fb83175c334f45d.png](/assets/2379959538085a767fb83175c334f45d-i4pxek39h4vv.png)

### Full Join
```sql
SELECT url, username
FROM photos
FULL JOIN users ON users.id = photos.user_id;
```

![30e5c373c4081b221c3d662c4cc9664d.png](/assets/30e5c373c4081b221c3d662c4cc9664d-0d7p4hvkbnt0.png)

# Grouping
![2441f38b5b2f75eba1771db1687672d2.png](/assets/2441f38b5b2f75eba1771db1687672d2-y47aozf2bryt.png)

```sql
SELECT user_id
FROM comments
GROUP BY user_id
```

![368ec71fa8c667d93298e59634437606.png](/assets/368ec71fa8c667d93298e59634437606-i2u15zx8glgd.png)

Results
![3e0396b2dbc13b262af669ba54cd0b70.png](/assets/3e0396b2dbc13b262af669ba54cd0b70-5b73247imfy0.png)

Only can directly select the `Group By` Comlumn,
Cannot select others underlying column
Must combine with `Aggregation Function`

![45182127a6f0e2a802138284351a6c97.png](/assets/45182127a6f0e2a802138284351a6c97-2d2aq137r5hf.png)

 
# Aggregation of Records
## Definition
![19f4ada3cd8ebc0e395f422b99b31c41.png](/assets/19f4ada3cd8ebc0e395f422b99b31c41-a32uuv0uwg5i.png)

## Examples

```sql
SELECT user_id, MAX(id) -- aggregation function
FROM comments
GROUP BY user_id
```

```sql
SELECT user_id, COUNT(id) AS num_comments_created -- aggregation function
FROM comments
GROUP BY user_id
```

![eff942ef219442dc6bd923065b303243.png](/assets/eff942ef219442dc6bd923065b303243-zx4hc6sovaxt.png)

## Some Aggregation functions

![ddf4f6e79b50b80370ad476f7477b548.png](/assets/ddf4f6e79b50b80370ad476f7477b548-rhv57qfy743m.png)

## Use aggregation function alone without GROUP BY

```sql

-- NULL value will not be counted
SELECT COUNT(user_id) FROM photos;

-- total number of photos will be retrieved
SELECT COUNT(*) FROM photosl

```


## filtering groups with `HAVING`

### Example:

Find the nmber of comments for each photo
where the photo_id is < 3
and the photo has more than 2 comments

```sql
SELECT photo_id, COUNT(*)
FROM comments
WHERE photo_id < 3
GROUP BY photo_id
HAVING COUNT(*) > 2;
```

Find the user_ids where the user has commented on the first 50 photos and the user added more than 20 comments on those photos

```sql

SELECT user_ids, COUNT(*)
FROM comments
WHERE photo_id < 51
GROUP BY user_id
HAVING COUNT(*) > 20;
```


# Sort, Offset and Limit

# Unions and Intersections

## Example

Find the 4 products with the highest price and the 4 products with highest price/weight ratio


```sql
(
	SELECT *
	FROM products
	ORDER BY price DESC
	LIMIT 4
)
UNION
-- UNION ALL for keeping the duplicated
(
	SELECT *
	FROM products
	ORDER BY price / weight DESC
	LIMIT 4
);
```

Notes:
- The two union columns should use the same name, and they must have compatible data types as well.
![c8d1eea234c68d7db070dd9cb1bcd26a.png](/assets/c8d1eea234c68d7db070dd9cb1bcd26a-61o2bxsni7jr.png)

## Some keyword in SQL to wok with multiple sets of data

![1a1f46921438a45c6bd8e294dd60658a.png](/assets/1a1f46921438a45c6bd8e294dd60658a-nawn2w994mxm.png)

-  `INTERSECT ALL` doesn’t eliminate duplicate rows like `INTERSECT`

- The SQL `EXCEPT` operator takes the distinct rows of one query and returns the rows that do not appear in a second result set. The `EXCEPT ALL` operator does not remove duplicates.
	- For purposes of row elimination and duplicate removal, the `EXCEPT` operator does not distinguish between `NULLs`.

	- `EXCEPT ALL` which returns all records from the first table which are not present in the second table, leaving the duplicates as is.

# Subquery

## Example

List the name and price of all products that are more expensive than all products in the 'Toys' department 
![7bd968bc0bc0d1ad03ed17a8ede19981.png](/assets/7bd968bc0bc0d1ad03ed17a8ede19981-wtlau2dagwew.png)

```SQL
SELECT name, price
FROM products
WHERE price > (
	SELECT MAX(price) FROM products WHERE  department = 'Toys'
);
```

## Structure of Data
![e594b0f62b51c73b6b987e918bb183aa.png](/assets/e594b0f62b51c73b6b987e918bb183aa-h54c8ddoj5jg.png)

## Subquery in multiple places

```SQL
SELECT
	p1.name,
	(SELECT COUNT(name) FROM products)
FROM (SELECT * FROM products) AS p1
JOIN (SELECT * FROM products) AS p2 ON p1.id = p2.id
WHERE p1.id IN (SELECT id FROM product);
```

![d5481fceac24e612059680777e287851.png](/assets/d5481fceac24e612059680777e287851-lk27yhoaer3c.png)


### Subquery in `SELECT`
![7296662d8881b03dff7bd63c6c06d6f2.png](/assets/7296662d8881b03dff7bd63c6c06d6f2-ekdxo1n9bbn1.png)

## Subquery in `FROM`

![29119c7ba52f0f12865d66fd6f9b2751.png](/assets/29119c7ba52f0f12865d66fd6f9b2751-b8zvpidvopvc.png)
