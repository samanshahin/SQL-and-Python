# SQL-and-Python

With all complexity of Sql, we're just trying to discuss data science-related issues of it here.
There are more just one easy way to handle databases in python. From using python features like collections (arrays) to using python libraries like sqlite3 and finally no-sql solutions with MingoDB, all will be discussed here.
Note: We are not considering on your database architecture and how tables are connected together. That's a different issue and we're not getting into that discussion. Briefly, there are more tools that you can use to visualize your db structure and analyze it. 

## Section 1. Python features known as python collections (arrays) including:

In all these features, you can access to a member like this:
```
x = mylist[0]
```
#### - List

List is a collection which is ordered and changeable. Allows duplicate members.
```
mylist = ["apple", 12, true]
```
Or
```
mylist = list(("apple", 12, true))
```

**List Methods:**
- append(): Adds an element at the end of the list
- clear(): Removes all the elements from the list
- copy(): Returns a copy of the list
- count(): Returns the number of elements with the specified value
- extend(): Add the elements of a list (or any iterable), to the end of the current list
- index(): Returns the index of the first element with the specified value
- insert(): Adds an element at the specified position
- pop(): Removes the element at the specified position
- remove(): Removes the item with the specified value
- reverse(): Reverses the order of the list
- sort(): Sorts the list

#### - Set
Set is a collection which is unordered, unchangeable*, and unindexed. No duplicate members.
```
myset = {"apple", "banana", "cherry"}
```
Or
```
myself = set(("apple", "banana", "cherry"))
```
Note: As it does not allow duplicates, the values True and 1 are considered the same value in sets, and are treated as duplicates.

#### - Dictionary
A dictionary is a collection which is unordered, changeable and indexed (it means do not allow duplicates):
```
x = dict(key1="value1", key2=3, key3="val3")
```
Or
```
thisdict = {
  "brand": "Ford",
  "model": "Mustang",
  "year": 1964,
  "colors": ["red", "white", "blue"]
}
```
#### - Tuple
It is a collection which is ordered and unchangeable. Allows duplicate members.
```
mytuple = ("apple", "banana", "cherry")
```
Or
```
thistuple = tuple(("apple", "banana", "cherry"))
```
One item tuple (remember the comma):
```
thistuple = ("apple",)
```
#### - String manipulation
There are some functions that could be applied on strings, if your source data is in a textual content. The common functions are:
- count("string"): Returns the number of times a specified value occurs in a string
- split(separator, maxsplit): split the string by separator with maxsplit (max number of members) and returns a list.
- rsplit(separator, maxrsplit): the same as split() but from the end of the text
- splitlines(keeplinesbreaks): splits the string at line breaks and returns a list. 

## Section 2. Working with files:
Files that we could extract data from and use in python includes: json, xml, csv, xls, xlsx, txt, html
### A. txt file format:
First you should open the file:
```
f = open("demofile.txt", rt): for the second argument, you should pick one from this list: r (read), w (write), a (append), x (create) and another one from this list: t (rext), b (binary)
```
Then read the content:
```
x = f.read() or f.readlines()
```
Now you can work on the content by methods introduces in 'string' section above 
And finally close it by f.close()
### B. html file format: 
It is all about scraping web content and I will publish a repository on this ...
### C. lovely json file format:
I will publish a repository on this...
## Section 3. Python libraries
### A little about python and sql...
First you should introduce sql to your working project in python, no matter you are trying to build app for Desktop or web development. There are 2 python libraries to work with sql in python: sqlite3 library and mysql.connector library. The difference is ONLY in getting connected to SQL.
#### A. For sqlite3, just import it:
````
import sqlite3
```
Then connect to dbname, if it does not exist, it creates it:
````
db = sqlite3.connect("dbname.db")
cur = db.cursor()
```
Now is the time to do query:
```
cur.execute("query")
```
And at the end, ALWAYS commit the query and then close the db:
```
db.commit()
db.close()
```
#### B. For mysql.connector, things are a little different:
So you should install MySQL Driver on Python folder. In command prompt:
```
python -m pip install mysql-connector-python
```
Now import it and create connection:
```
import mysql.connector
mydb = mysql.connector.connect(
  host="localhost",
  user="yourusername",
  password="yourpassword"
)
```
** Now you are free to do your job in sql: **
The pattern is like this:
```
mycursor = mydb.cursor()
mycursor.execute("the_queries")
```
The mysql.connector module uses the placeholder %s to prevent sql injection, increasing the security I case of cyber attacks. So we used it in some coding. 

## Now div into the sql
Before talking about no-sql solution, we should talk a little about SQL features::
The queries include these:
### 1. Creating db:
```
CREATE DATABASE mydbname
```
After creating DB, you can use it in connection part above to distinguish from other databases and again, increase the security, so add this line there, after 'password' section:
database="mydbname" 
### 2. Creating table:
```
CREATE TABLE customers (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(255), address VARCHAR(255))
```
If the table exists, use 'alter' instead to add a column to the table:
```
ALTER TABLE customers ADD COLUMN id INT AUTO_INCREMENT PRIMARY KEY
```
Note: Everywhere you want to insert or delete or update data, one line would  be added to our code: mydb.commit() 
### 3. Inserting data:
Note: The mysql.connector module uses the placeholder %s to escape values in a statement.
```
sql = "INSERT INTO customers (name, address) VALUES (%s, %s)"
val = [
  ('Peter', 'Lowstreet 4'),
  ('Amy', 'Apple st 652'),
  ('Hannah', 'Mountain 21'),
  ('Michael', 'Valley 345'),
  ('Sandy', 'Ocean blvd 2'),
  ('Betty', 'Green Grass 1'),
  ('Richard', 'Sky st 331'),
  ('Susan', 'One way 98'),
  ('Vicky', 'Yellow Garden 2'),
  ('Ben', 'Park Lane 38'),
  ('William', 'Central st 954'),
  ('Chuck', 'Main Road 989'),
  ('Viola', 'Sideway 1633')
]
mycursor.execute(sql, val)
mydb.commit() 
```
### 4. Selecting/fetching data
```
mycursor.execute("SELECT name, address FROM customers")
```
Note that the comma after SELECT but it will NOT be repeated after each modifier as you can see in the following sections.
You can format the date presened in a column, it changes the col1name by newcol1name:
```
SELECT DATE_FORMAT(col1name, '%Y-%m-%d') AS newcol1name, ...
```
You should use fetchall() to get the result:
```
myresult = mycursor.fetchall()
for x in myresult:
  print(x)
```
You can fetch data into another table, even in another db:
```
SELECT colname(s) INTO new_tblname IN externaldb FROM old_tblname 
```
### 5. Conditional fetching data:
#### A. Using 'LIMIT' and 'OFFSET' keywords
to show first n rows starting from position m (remember that the counting starts from 0 not 1, it means 'OFFSET 2' refers to 3rd position of the row):
```
LIMIT n OFFSET m
```
#### B. Using 'WHERE' keyword:
```
WHERE address ='some value' AND|OR other_condition
WHERE address LIKE '%something%'
WHERE address BETWEEN value1 AND value2
WHERE colname IN (value1,value2,..)
```
#### C. With multiple conditions using 'CASE' keyword:
```
SELECT client, date_due, amount_due, DATEDIFF ('2020-04-30', date_due) AS days_due, CASE WHEN DATEDIFF ('2020-04-30', date_due) <= 30 THEN '0-30 days' WHEN DATEDIFF ('2020-04-30', date_due) > 30 AND DATEDIFF ('2020-04-30', date_due) <=90 THEN '31-90 days' WHEN DATEDIFF ('2020-04-30', date_due) > 90 AND DATEDIFF ('2020-04-30', date_due) <=180 THEN '91-180 days' WHEN DATEDIFF ('2020-04-30', date_due) > 180 AND DATEDIFF ('2020-04-30', date_due) <=365 THEN '181-365 days' ELSE '> 365 days' END AS time_bucket FROM debt
```
#### D. Using 'SELECT' keyword:
```
SELECT TOP number|percent col(s) FROM tblname
```
#### E. IF EXISTS
```
IF EXISTS (SELECT * FROM table_name WHERE id = ?)
BEGIN
--do what needs to be done if exists
END
ELSE
BEGIN
--do what needs to be done if not
END
```
#### D. HAVING 
```
HAVING col1name operator_value 
```
### 6. Sorting and grouping data:
#### A. First in first, sortig:
```
ORDER BY colname
```
You can add 'DESC' or 'ASC' after the colname
#### B. What about Grouping:
GROUPING SETS provides an alternative to using UNION ALL to combine results from multiple individual queries, each with its own GROUP BY clause.
```
SELECT Category, Cust, SUM(Qty) AS TotalQty FROM Sales.CategorySales GROUP BY GROUPING SETS((Category),(Cust),()) ORDER BY Category, Cust 
```
You could use empty parentheses at the end if you want aggregating all rows
#### C. Another way of grouping:
Like GROUPING SETS, the CUBE and ROLLUP subclauses also enable multiple groupings for aggregating data. However, CUBE and ROLLUP do not need you to specify each set of attributes to group. It's easy:
```
SELECT Category, Cust, SUM(Qty) AS TotalQty FROM Sales.CategorySales GROUP BY CUBE(Category,Cust)
```
Or:
```
SELECT Category, Subcategory, Product, SUM(Qty) AS TotalQty FROM Sales.ProductSales GROUP BY ROLLUP(Category,Subcategory, Product)
```
#### D. Let's try Pivoting (converting rows to columns, if your dataset is rows to columns):
It's creating a new table (D) with 4 cols cat, 2018, ... and categorizes data, then sum up and show the aggregate for each year, but not ordered.
```
SELECT  Category, [2019],[2020],[2021] FROM  ( SELECT  Category, Qty, Orderyear FROM Sales.CategoryQtyYear) AS D PIVOT(SUM(qty) FOR orderyear IN ([2019],[2020],[2021])) AS pvt
```
And vice versa for unpivoting if your dataset is columns to rows. It sums up items by category and year.
```
SELECT category, qty, orderyear FROM Sales.PivotedCategorySales UNPIVOT(qty FOR orderyear IN([2019],[2020],[2021])) AS unpvt
```
#### E. A little about ranking:
```
SELECT  product, product_price, items_sold, product_price * items_sold AS revenue, RANK() OVER (ORDER BY product_price * items_sold DESC) AS revenue_rank FROM sales 
```
### 7. Deleting data:
Deleting row(s):
```
DELETE FROM customers WHERE address = 'some value'
```
Or dropping a whole table (if exists):
```
DROP TABLE IF EXISTS customers 
```
### 8. Updating data:
```
UPDATE customers SET address = 'some value' WHERE address = 'some other value' 
```
### 9. Joining data:
#### A. Using 'JOIN's family:
Note: INNER JOIN and JOIN work the same and no matter how many tables you want to join, with or without condition(s). The condition(s) goes after 'ON':
```
SELECT tbl1.col1 AS colX, tbl2.col2 AS colY FROM tbl1 INNER JOIN tbl2 ON tbl1.col1 = tbl2.col2
```
But, If you want to join tables with conditions to be applied on other tables, not a specific one, you should use 'LEFT JOIN' or 'RIGHT JOIN'. It would be applied where you have two tables.
FULL JOIN is the combination of both, frankly I don't know what's the deal with this command:
```
SELECT colname(s) FROM tbl1 FULL JOIN tbl2 ON tbl1.col1=tbl2.col2
```
#### B. UNION:
```
SELECT col(s) FROM tbl1 UNION SELECT col(s) FROM tbl2
```
For UNION ALL, we mentioned somewhere above. 

## More complex examples with sql:
### A. You can count the number of any repeats by:
```
COUNT(*) FROM cust_order GROUP BY DATE_FORMAT(order_date, '%Y-%m-%d')
```
Or counting and showing just unique results by using DISTINCT keyword:
```
SELECT DATE_FORMAT(co.order_date, '%Y-%m-%d') AS order_day, COUNT(DISTINCT co.order_id) AS num_orders FROM cust_order co INNER JOIN order_line ol ON co.order_id = ol.order_id GROUP BY DATE_FORMAT(co.order_date, '%Y-%m-%d') ORDER BY co.order_date ASC
```
Note that you can use more than one COUNT modofier.
You can go further and add sum to counted results:
```
SELECT DATE_FORMAT(co.order_date, '%Y-%m-%d') AS order_day, COUNT(DISTINCT co.order_id) AS num_orders, COUNT(ol.book_id) AS num_books, SUM(ol.price) AS total_price FROM cust_order co INNER JOIN order_line ol ON co.order_id = ol.order_id GROUP BY DATE_FORMAT(co.order_date, '%Y-%m-%d') ORDER BY co.order_date ASC 
```
### B. Now, what about a running total:
A running total means a sum of orders for the current row and all rows before it. This means each row the running total should increase and it should reset at the start of the next month. Like this (it sums up ever day's order to the previous day and shows it for the day and resets every month):
```
SELECT DATE_FORMAT(co.order_date, '%Y-%m') AS order_month, DATE_FORMAT(co.order_date, '%Y-%m-%d') AS order_day, COUNT(DISTINCT co.order_id) AS num_orders, COUNT(ol.book_id) AS num_books, SUM(ol.price) AS total_price, SUM(COUNT(ol.book_id)) OVER ( PARTITION BY DATE_FORMAT(co.order_date, '%Y-%m') ORDER BY DATE_FORMAT(co.order_date, '%Y-%m-%d') ) AS running_total_num_books FROM cust_order co INNER JOIN order_line ol ON co.order_id = ol.order_id GROUP BY DATE_FORMAT(co.order_date, '%Y-%m'), DATE_FORMAT(co.order_date, '%Y-%m-%d') ORDER BY co.order_date ASC 
```
### C. The next example is also complicated.
It’s to show “the number of books from the same day last week (e.g. how this Sunday compares to last Sunday)”. 
```
SELECT order_month, order_day, COUNT(DISTINCT order_id) AS num_orders, COUNT(book_id) AS num_books, SUM(price) AS total_price, SUM(COUNT(book_id)) OVER ( PARTITION BY order_month ORDER BY order_day ) AS running_total_num_books, LAG(COUNT(book_id), 1) OVER (ORDER BY order_day) AS prev_books FROM ( SELECT DATE_FORMAT(co.order_date, '%Y-%m') AS order_month, DATE_FORMAT(co.order_date, '%Y-%m-%d') AS order_day, co.order_id, ol.book_id, ol.price FROM cust_order co INNER JOIN order_line ol ON co.order_id = ol.order_id ) sub GROUP BY order_month, order_day ORDER BY order_day ASC
```
This query is for last day, just change 1 in 'LAG(COUNT(book_id), 1)' to 7 for data from last week.
More explanation can be found here: https://www.databasestar.com/sql-lead/ and here: https://www.databasestar.com/sql-window-functions/


## Section 4. What about no-sql solution:
MongoDB is one famous no-sql solution, but a little weird. It stores data in JSON-like documents, which makes the database very flexible and scalable, as I think is more memory and CPU consuming and not very secure.
By the way, first you should install the library:
```
python -m pip install pymongo
```
Then get in to the work:
Note: In MongoDB, you are facing lots of weird stuffs, like a database is not created until it gets content! It means MongoDB waits until you have created a collection (table), with at least one document (record) before it actually creates the database (and collection). What a weird naming!!
How to code:
```
import pymongo
myclient = pymongo.MongoClient("serverinfo", port_num)
# You can use this instead: mongodb://localhost:27017/")
mydb = myclient["mydbname"]
mycol = mydb["customers"]
mylist = [
  { "name": "Amy", "address": "Apple st 652"},
  { "name": "Hannah", "address": "Mountain 21"},
  { "name": "Michael", "address": "Valley 345"},
  { "name": "Sandy", "address": "Ocean blvd 2"},
  { "name": "Betty", "address": "Green Grass 1"},
  { "name": "Richard", "address": "Sky st 331"},
  { "name": "Susan", "address": "One way 98"},
  { "name": "Vicky", "address": "Yellow Garden 2"},
  { "name": "Ben", "address": "Park Lane 38"},
  { "name": "William", "address": "Central st 954"},
  { "name": "Chuck", "address": "Main Road 989"},
  { "name": "Viola", "address": "Sideway 1633"}
]
x = mycol.insert_many(mylist)
```
Note: You can use insert_one(mydict) to add one document (record).
Note: The tricky note is there is no way to add auto increasing ID to documents in MongoDB.
For finding data, like SELECT in sql, we use find_one() for one document and fine() for many.
The code below returns only the names and addresses, not the _ids. 
The other weird thing is if you specify a field with the value 0, all other fields get the value 1, and vice versa: (it means you can just call mycol.find({},{ "address": 0 }) to exclude 'address' field but you can not use mycol.find({},{ "name": 1, "address": 0 }) for the same purpose, because 0 while 1 exists, is only for _id field. WEIRD!)
```
for x in mycol.find({},{ "_id": 0, "name": 1, "address": 1 }):
  print(x)
# Or be more specific:
mycol.find({"address": "Park Lane 38"})
# you can use modifiers as values in the query object, e.g. to find the documents where the "address" field starts with the letter "S" or higher (alphabetically), use the greater than modifier: {"$gt": "S"}:
mycol.find({ "address": { "$gt": "S" } })
# Or filter with regular expression:
mycol.find({ "address": { "$regex": "^S" } })
# For sorting data:
mycol.find().sort("name", 1) #for asc and -1 for desc
# For deleting data:
myquery = { "address": "Mountain 21" }
mycol.delete_one(myquery)
```
But if it finds more than one result, it deletes the first row. Instead use this:
```
myquery = { "address": {"$regex": "^S"} }
x = mycol.delete_many(myquery)
If(x.deleted_count>1): 
   print("documents deleted.")
# For updating one or many douments:
myquery = { "address": "Valley 345" }
newvalues = { "$set": { "address": "Canyon 123" } }
mycol.update_one(myquery, newvalues)
# And for many:
myquery = { "address": { "$regex": "^S" } }
newvalues = { "$set": { "name": "Minnie" } }
x = mycol.update_many(myquery, newvalues)
if(x.modified_count>1):
    print("documents updated.")
```
Read their documents at https://pymongo.readthedocs.io/en/stable/ and for other advacnced weird methods refer to their website at https://www.mongodb.com/docs/ .

Have fun with it!
