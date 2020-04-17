-- https://github.com/DevMountain/sql-2-afternoon

-- https://postgres.devmountain.com/





--Practice joins--
--.1Get all invoices where the unit_price on the invoice_line is greater than $0.99.

-- SELECT * FROM invoice i
-- JOIN invoice_line il ON il.invoice_id = i.invoice_id
-- WHERE il.unit_price > 0.99;

--.2 Get the invoice_date, customer first_name and last_name, and total from all invoices.

-- SELECT i.invoice_date, c.first_name, c.last_name, i.total
-- FROM invoice i
-- JOIN customer c ON i.customer_id = c.customer_id;

--.3Get the customer first_name and last_name and the support rep's first_name and last_name from all customers.
Support reps are on the employee table.

-- SELECT c.first_name, c.last_name, e.first_name, e.last_name
-- FROM customer c
-- JOIN employee e ON c.support_rep_id = e.employee_id;

--.4 Get the album title and the artist name from all albums.

-- SELECT al.title, ar.name
-- FROM album al
-- JOIN artist ar ON al.artist_id = ar.artist_id;


--.5 Get all playlist_track track_ids where the playlist name is Music. 

-- SELECT pt.track_id
-- FROM playlist_track pt
-- JOIN playlist p ON p.playlist_id = pt.playlist_id
-- WHERE p.name = 'Music';

--.6 Get all track names for playlist_id 5.

-- SELECT t.name
-- FROM track t
-- JOIN playlist_track pt ON pt.track_id = t.track_id
-- WHERE pt.playlist_id = 5;

--.7 Get all track names and the playlist name that they're on ( 2 joins ).

-- SELECT t.name, p.name
-- FROM track t
-- JOIN playlist_track pt ON t.track_id = pt.track_id
-- JOIN playlist p ON pt.playlist_id = p.playlist_id;

--.8 Get all track names and album titles that are the genre Alternative & Punk ( 2 joins ).

-- SELECT t.name, a.title
-- FROM track t
-- JOIN album a ON t.album_id = a.album_id
-- JOIN genre g on g.genre_id = t.genre_id
-- WHERE g.name = 'Alternative & Punk';

--BLACK DIAMOND
Get all tracks on the playlist(s) called Music and show their name, genre name, album name, and artist name.
At least 5 joins.

--------------------------------------------------------------------------------

-- Practice nested queries

--.1 Get all invoices where the unit_price on the invoice_line is greater than $0.99.

-- SELECT *
-- FROM invoice
-- WHERE invoice_id IN ( SELECT invoice_id FROM invoice_line WHERE unit_price > 0.99 );

--.2 Get all playlist tracks where the playlist name is Music.

-- SELECT *
-- FROM playlist_track
-- WHERE playlist_id IN ( SELECT playlist_id FROM playlist WHERE name = 'Music');

--.3 Get all track names for playlist_id 5.

-- SELECT name
-- FROM track
-- WHERE track_id IN ( SELECT track_id FROM playlist_track WHERE playlist_id = 5);

--.4 Get all tracks where the genre is Comedy.

-- SELECT * 
-- FROM TRACK 
-- WHERE genre_id IN ( SELECT genre_id FROM genre WHERE name = 'Comedy' );

--.5 Get all tracks where the album is Fireball.

-- SELECT *
-- FROM TRACK
-- WHERE album_id IN ( SELECT album_id FROM album WHERE title = 'Fireball' );

--.6 Get all tracks for the artist Queen ( 2 nested subqueries ).
-- SELECT *
-- FROM track
-- WHERE album_id IN (
--   SELECT album_id FROM album WHERE artist_id IN (
--     SELECT artist_id FROM artist WHERE name = 'Queen'
--     )
--   );
  
--  -- -- --------------------------------------------------------------------------


-- --  ---------------------------------------------------------------------------------------------------------------
  
  --Practice updating Rows
  
  -- .1 Find all customers with fax numbers and set those numbers to null.
  
--   UPDATE customer
--   SET fax = null
--   WHERE fax IS NOT null;
  
  -- .2 Find all customers with no company (null) and set their company to "Self".
  
--   UPDATE customer
--   SET company = 'Self'
--   WHERE company IS null;
  
--   SELECT * FROM customer
	 
-- .3 Find the customer Julia Barnett and change her last name to Thompson.

-- UPDATE customer 
-- SET last_name = 'Thompson' 
-- WHERE first_name = 'Julia' AND last_name = 'Barnett';

-- .4 Find the customer with this email luisrojas@yahoo.cl and change his support rep to 4.

-- UPDATE customer
-- SET support_rep_id = 4
-- WHERE email = 'luisrojas@yahoo.cl';

-- .5   Find all tracks that are the genre Metal and have no composer. Set the composer to "The darkness around us".

--  SELECT * FROM customer


-- UPDATE track
-- SET composer = 'The darkness around us'
-- WHERE genre_id = ( SELECT genre_id FROM genre WHERE name = 'Metal' )
-- AND composer IS null;


-- ____________________________________________________________________________________________


-- -- Group by

--  Syntax Hint
-- SELECT [column1], [column2]
-- FROM [table] [abbr]
-- GROUP BY [column];


-- .1 Find a count of how many tracks there are per genre. Display the genre name with the count.

-- 	SELECT COUNT(*), g.name
--   FROM track t
--   JOIN genre g ON  t.genre_id = g.genre_id
--   GROUP BY g.name;
  

--.2 Find a count of how many tracks are the "Pop" genre and how many tracks are the "Rock" genre.

-- SELECT COUNT(*), g.name
-- FROM track t
-- JOIN genre g ON g.genre_id = t.genre_id
-- WHERE g.name = 'Pop' OR g.name = 'Rock'
-- GROUP BY g.name;


--.3 Find a list of all artists and how many albums they have.

-- SELECT ar.name, COUNT(*)
-- FROM album al
-- JOIN artist ar ON ar.artist_id = al.artist_id
-- GROUP BY ar.name;

 -- -- -----------------------------------------------------------------------------------------
-- Use Distinct

--.1 From the track table find a unique list of all composers.

-- SELECT DISTINCT composer
-- FROM track;

--.2 From the invoice table find a unique list of all billing_postal_codes.

-- SELECT DISTINCT billing_postal_code
-- FROM invoice;

--.3 From the customer table find a unique list of all companys.

-- SELECT DISTINCT company
-- FROM customer;

-----------------------------------------------------------------------------------
-- Delete Rows

--Syntax Hint
--DELETE FROM [table] WHERE [condition]

--.1 Copy, paste, and run the SQL code from the summary.

-- DELETE 
-- FROM practice_delete 
-- WHERE type = 'bronze';

--.2 Delete all 'bronze' entries from the table.

-- DELETE 
-- FROM practice_delete 
-- WHERE type = 'silver';

--.3 Delete all 'silver' entries from the table.

-- DELETE
-- FROM practice_delete
-- WHERE value = 150;


-- SELECT * from track



-- ----------------------------------
-- eCommerce Simulation - No Hints


-- CREATE TABLE users (
--   users_id SERIAL PRIMARY KEY,
--   name VARCHAR(40),
--   email VARCHAR(200)
--   );
  
--   CREATE TABLE products (
--   products_id SERIAL PRIMARY KEY,
--   name VARCHAR(40),
--   price INTEGER
--   );
  
--   CREATE TABLE orders (
--   orders_id SERIAL PRIMARY KEY,
--   product_id INTEGER REFERENCES products(products_id)
--   );

-- INSERT INTO orders (product_id)
--   VALUES(1),(2),(3)
  
  
--   INSERT INTO users (name, email)
--   VALUES ('Joe', 'joe@gmail.com'),
--   ('Jim', 'jim@gmail.com'),
--   ('Steve', 'Steve@gmail.com');
  
--   INSERT INTO products (name, price)
--   VALUES ('Washie', 43.50),
--   ('Slim Jim', 32.99),
--   ('Steve',25.95)
  
  
--   Select * from products
--   JOIN orders o ON o.product_id = products.products_id
--   WHERE o.orders_id =1



-- Get all orders.


-- Select * from products
--   JOIN orders o ON o.product_id = products.products_id
 
 
 
--  Get the total cost of an order ( sum the price of all products on an order ).

-- Select SUM(products.price) FROM products
-- JOIN orders o ON o.product_id = products.products_id 
-- GROUP BY o.orders_id 


--   ALTER TABLE products
--   ALTER price
--   SET DATA TYPE DECIMAL



-- -- Add a foreign key reference from orders to users.

-- ALTER TABLE orders
-- ADD COLUMN user_orders INTEGER REFERENCES users(users_id)


-- -- -- Update the orders table to link a user to each order.

-- UPDATE orders 
-- SET user_orders = 3
-- WHERE orders_id =3;

-- SELECT * FROM orders


-- --- -- Get all orders for a user.
-- SELECT * FROM orders o
-- JOIN users u ON u.users_id = o.orders_id;


-- -- -- Get how many orders each user has.

-- SELECT COUNT(*) FROM orders
-- GROUP BY user_order



