## Solutions

### A. Easy Questions

#### 1. Who is the senior most employee based on job title?

```sql
SELECT   title, last_name, first_name 
FROM     employee
ORDER BY levels DESC
LIMIT    1
```

**Result Set:**


title | first_name | last_name |
--|--|--|
Senior General Manager | Madan | Mohan

---

#### 2. Which countries have the most Invoices?

```sql
SELECT   billing_country, COUNT(*) as invoice_count
FROM     invoice
GROUP BY 1
ORDER BY 2 DESC
LIMIT    10
```

**Result Set:**

billing_country	 |invoice_count |
--|--|
USA |	131 |
Canada |	76 |
Brazil |	61 |
France |	50 |
Germany |	41 |
Czech Republic |	30 |
Portugal |	29 |
United Kingdom |	28 |
India |	21 |
Chile |	13 |


---

#### 3. What are top 3 values of total invoice?

```sql
SELECT   total 
FROM     invoice
ORDER BY 1 DESC
LIMIT    3
```

**Result Set:**

total |
--|
23.759999999999998 |
19.8 |
19.8 |

---

#### 4. Which city has the best customers?

We would like to throw a promotional Music Festival in the city we made the most money. Write a query that returns one city that has the highest sum of invoice totals. Return both the city name & sum of all invoice totals.

```sql
SELECT   billing_city, SUM(total) AS invoice_total
FROM     invoice
GROUP BY 1
ORDER BY 2 DESC
LIMIT    1
```

**Result Set:**

billing_city |	invoice_total |
--|--|
Prague |	273.24000000000007 |

---

#### 5. Who is the best customer?

The customer who has spent the most money will be declared the best customer. Write a query that returns the person who has spent the most money.

```sql
SELECT   c.customer_id, first_name, last_name, SUM(total) AS total_spending
FROM     customer c JOIN invoice i USING(customer_id)
GROUP BY 1
ORDER BY 4 DESC
LIMIT    1
```

**Result Set:**

customer_id |	first_name |	last_name |	total_spending |
--|--|--|--|
5 | R |  Madhav  | 144.54000000000002 |

---

### B. Moderate Questions

#### 1. Write query to return the email, first name, last name, & Genre of all Rock Music listeners.

Return your list ordered alphabetically by email starting with A.


```sql
-- Solution 1:

SELECT DISTINCT email, first_name, last_name
FROM   customer JOIN invoice USING(customer_id)
                JOIN invoiceline USING(invoice_id)
WHERE  track_id IN (
    SELECT track_id 
    FROM   track JOIN genre USING(genre_id)
    WHERE  genre.name LIKE 'Rock'
)
ORDER BY 1


-- Solution 2:

SELECT   DISTINCT email, first_name, last_name, genre.name AS genre
FROM     customer JOIN invoice i USING(customer_id)
                  JOIN invoiceline il USING(invoice_id)
                  JOIN track t USING(track_id)
                  JOIN genre g USING(genre_id)
WHERE    g.name LIKE 'Rock'
ORDER BY 1
```

**Result Set:**

*displaying only first 10 rows out of 59*


| email                          | first_name | last_name  | genre |
|--------------------------------|------------|------------|-------|
| aaronmitchell@yahoo.ca         | Aaron      | Mitchell   | Rock  |
| alero@uol.com.br               | Alexandre  | Rocha      | Rock  |
| astrid.gruber@apple.at         | Astrid     | Gruber     | Rock  |
| bjorn.hansen@yahoo.no          | Bjørn      | Hansen     | Rock  |
| camille.bernard@yahoo.fr       | Camille    | Bernard    | Rock  |
| daan_peeters@apple.be          | Daan       | Peeters    | Rock  |
| diego.gutierrez@yahoo.ar       | Diego      | Gutiérrez  | Rock  |
| dmiller@comcast.com            | Dan        | Miller     | Rock  |
| dominiquelefebvre@gmail.com    | Dominique  | Lefebvre   | Rock  |
| edfrancis@yachoo.ca            | Edward     | Francis    | Rock  |



---

#### 2. Let's invite the artists who have written the most rock music in our dataset.

Write a query that returns the Artist name and total track count of the top 10 rock bands.

```sql
SELECT   a.artist_id, a.name, COUNT(a.artist_id) AS number_of_songs
FROM     track JOIN album al USING(album_id)
               JOIN artist a USING(artist_id)
               JOIN genre g USING(genre_id)
WHERE    g.name LIKE 'Rock'
GROUP BY a.artist_id
ORDER BY 3 DESC
LIMIT    10
```

**Result Set:**

| artist_id | name                          | number_of_songs |
|-----------|-------------------------------|-----------------|
| 22        | Led Zeppelin                  | 114             |
| 150       | U2                            | 112             |
| 58        | Deep Purple                   | 92              |
| 90        | Iron Maiden                   | 81              |
| 118       | Pearl Jam                     | 54              |
| 152       | Van Halen                     | 52              |
| 51        | Queen                         | 45              |
| 142       | The Rolling Stones            | 41              |
| 76        | Creedence Clearwater Revival  | 40              |
| 52        | Kiss                          | 35              |


---

#### 3. Return all the track names that have a song length longer than the average song length.

Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first.

```sql
SELECT name, milliseconds
FROM   track
WHERE  milliseconds > (
         SELECT AVG(milliseconds)
         FROM   track
    )
ORDER BY 2 DESC
-- LIMIT 10
```

**Result Set:**

*displaying only first 10 rows out of 494*


| name                          | milliseconds |
|-------------------------------|--------------|
| Occupation / Precipice        | 5286953      |
| Through a Looking Glass       | 5088838      |
| Greetings from Earth, Pt. 1   | 2960293      |
| The Man With Nine Lives       | 2956998      |
| Battlestar Galactica, Pt. 2   | 2956081      |
| Battlestar Galactica, Pt. 1   | 2952702      |
| Murder On the Rising Star     | 2935894      |
| Battlestar Galactica, Pt. 3   | 2927802      |
| Take the Celestra             | 2927677      |
| Fire In Space                 | 2926593      |
