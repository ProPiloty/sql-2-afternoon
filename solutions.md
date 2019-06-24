#Solutions

##Joins
1.
SELECT *
FROM invoice_line
WHERE unit_price > 0.99;

2.
SELECT i.invoice_date, i.total, c.first_name, c.last_name
FROM invoice i
JOIN customer c ON c.customer_id = i.customer_id;

3.
SELECT c.first_name, c.last_name, e.first_name, e.last_name
FROM customer c
JOIN employee e on c.support_rep_id = e.employee_id;

4.
SELECT al.title, ar.name
FROM album al
JOIN artist ar ON al.artist_id = ar.artist_id;

5.
SELECT plt.track_id
FROM playlist_track plt
JOIN playlist pl ON plt.playlist_id = pl.playlist_id
WHERE pl.name = 'Music';

6.
SELECT t.name
FROM track t
JOIN playlist_track plt ON t.track_id = plt.track_id
WHERE playlist_id = 5;

7.
SELECT t.name, pl.name
FROM track t
JOIN playlist_track plt ON t.track_id = plt.track_id
JOIN playlist pl ON plt.playlist_id = pl.playlist_id;

8.
SELECT t.name, a.title
FROM track t
JOIN album a ON t.album_id = a.album_id
JOIN genre g ON t.genre_id = g.genre_id
WHERE g.name IN ('Alternative & Punk');

Black Diamond.


##Nested Queries
1.
select *
from invoice
where invoice_id in (
	select invoice_id from invoice_line where unit_price > 0.99
)

2.
SELECT * FROM playlist_track
WHERE playlist_id IN (
	SELECT playlist_id FROM playlist
  WHERE name = 'Music'
);

3.
SELECT name
FROM track
WHERE track_id IN (
	SELECT track_id from playlist_track WHERE playlist_id = 5
);

4.
SELECT *
FROM track
WHERE genre_id IN (
	SELECT genre_id FROM genre where name = 'Comedy'
);

5.
SELECT *
FROM track
WHERE album_id IN (
	SELECT album_id FROM album where name = 'Fireball'
);

6.
SELECT *
FROM track
WHERE album_id IN (
	SELECT album_id FROM album where artist_id IN (
  	SELECT artist_id FROM artist WHERE name = 'Queen'
  )
);

##Updating Rows
1.
UPDATE customer
SET fax = null
WHERE fax IS NOT null;

2.
UPDATE customer
SET company = 'self'
WHERE company is null;

3.
UPDATE customer
SET last_name = 'Thompson'
WHERE first_name = 'Julia' AND last_name = 'Barnett';

4.
UPDATE customer
SET support_rep_id = 4
WHERE email = 'luisrojas@yahoo.cl';

5.
UPDATE track
SET composer = 'The darkness around us'
WHERE composer is null AND genre_id IN (
	SELECT genre_id FROM genre WHERE name = 'Metal'
);

##Group By
1.
SELECT COUNT(*)
FROM track
GROUP BY genre_id;

2.
SELECT COUNT(*), g.name
FROM track t
JOIN genre g ON g.genre_id = t.genre_id
WHERE g.name = 'Pop' OR g.name = 'Rock'
GROUP BY g.name;

3.
SELECT COUNT(*), ar.name
FROM artist ar
JOIN album al ON ar.artist_id = al.artist_id
GROUP BY ar.name;

##Use Distinct
1.
SELECT DISTINCT composer
FROM track;

2.
SELECT DISTINCT billing_postal_code
FROM invoice;

3.
SELECT DISTINCT company
FROM customer;

##Delete Rows
1. (ACTUALLY #2)
DELETE FROM practice_delete
WHERE type = 'bronze';

2. (ACTUALLY #3)
DELETE FROM practice_delete
WHERE type = 'silver';

3. (ACTUALLY #4)
DELETE FROM practice_delete
WHERE value = 150;

##E-Commerce Simulation

###Create 3 tables
1.
CREATE TABLE users (
    id serial primary key,
    name varchar(120),
    email varchar(120)
)
2.
CREATE TABLE products (
    id serial primary key,
    name varchar(120),
    price integer
);
3.
CREATE TABLE orders (
	id serial primary key,
  product integer REFERENCES products(id)
);

###Add data to each table
1.
INSERT INTO users (name, email)
VALUES ('John', 'John@ymail.com');
VALUES ('Jane', 'Jane@zmail.com');
VALUES ('Rocky', 'Rocky@adrian.com');

2.
INSERT INTO products (name, price)
VALUES ('Bike', 20);
VALUES ('Truck', 60);
VALUES ('House', 200);

3.
INSERT INTO orders (product)
VALUES (1);
VALUES (2);
VALUES (3);

###Queries against data

1.
SELECT *
FROM orders
WHERE id = 1;

2.
SELECT *
FROM orders;

3.
SELECT SUM(pro.price)
FROM orders ord
JOIN products pro
ON ord.product = pro.id;


###Add foreign Key
1. ALTER TABLE orders ADD COLUMN user_id integer REFERENCES users(id);

###Update orders to add users
1. UPDATE TABLE orders SET user_id = ${id} where user_id = null;

###Queries against data
1. SELECT * FROM orders or JOIN users us ON or.user_id = us.id WHERE us.id = ${id};

2. SELECT COUNT(*) FROM orders ord JOIN users usr ON ord.user_id = usr.id;