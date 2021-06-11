# Querying data from PostgreSQL
SQL is the primary language used to work with databases and PostgresSQL is the leading open-source database platform. The fundamental building blocks to retrieve data are:  
SELECT FROM ;  
SELECT: Specifies columns of interest  
FROM: Where these columns are stored  
;: SQL statements should be terminated  
  
**In this module, we will look at:**  
1. Structure of relational databases  
2. Retrieving data form databases  
3. Limiting results  
4. Combining multiple tables  
  
### Understanding the Relational Model  
Data comes in many forms and SQL is the language we use to access the data. In this module, we will take a look at SQL and its special purpose in the data environment.   
**How do we make the connection between data and our day to day work?**  
Data is stored in a database. At its core, a database is a container that helps us to logically organize the data. Databases are most often relational in nature. SQL is the language we use to interact with that data to analyze the data and to get information that we need.  
SQL is a special purpose language. Its sole purpose is to interact with data. SQL, or Structured Query Language, is a platform that compiles with the national standard ANSI, which allows for SQL to be used across databases. While database providers can add additional features such as PL/SQL and Oracle SQL or the MDX language in Microsoft SQL server, the core SQL functionality is in line with the standard and is the same across platforms.  
## Inroduction to PostgresSQL  
PostgresSQL complies with ANSI standards and also provides additional functionality. Postgres is an opens ource and freely available database platform that is used by many startups and small enterprises. Postgres includes a graphical user interface called pgAdmin.  
## Understanding Relational Databases  
Lets first discuss the structure of a database. There are tables, columns and rows. A table contains all records for a particular set fo data. For example, you might have a table of employees, user data or financial trancations. Within a table you have columns which are the fileds or variables in the dataset. For example, in a people table, you may have column for the first name, a column for last name, etc. Rows are the records within a table. For example, in a people table, John, Bob, Ross and Scott may each have a row to store their data.  
![Table](https://github.com/vidushi-bansal/Postgres/blob/main/Table.png)  
The most important thing to notice in the above descripted tables is the relationship between those tables. This relationship is what makes a database relational. In this case, there is a relationship between the people table and the address table via the PersonID key. Therefore, PersonId is the primary key on the people table. It uniquely identifies each record.    
## Querying the data  
Through a series of examples, we will learn how to query a data to select fields of interest from a database.
**How do we retrieve information?**  
When we want to retrieve information from a database, we query the database for that information. We can write SQL statements called queries to fetch the desired results from the database.  
### Keywords  
Words that tell SQL to do something, or programming commands are keywords. Technically they are called reserved keywords because they are reserved for use by Postgres itself, but in practise they are generally just referred to as keywords.  
#### Selecting your data  
The most fundamental keyword in SQL is the SELECT command.It allows us to retrieve selected data from databases. Select can  perform calculations without a table.  
```
> SELECT 2+2;
4
```  
If we want to see all columns from a given table, we can use asterisk (*) as a wildcard.  
```  
SELECT * FROM tablename;
```  
When databases contain large amount of information, queries using the wildcard run very slowly.    
To tell the database what columns and fields we are interested in, we can simply list them after the SELECT keyword. Each column name should be seperated by a comma.  
```  
SELECT first_name, last_name
FROM person;
```  
We can use alias in our query to have a suitable column name representation to increase the readability.   
```  
SELECT first_name as Name, last_name
FROM person;
```  
The minimum number of fields you can specify is 1, and there is no maximum. You can explicitly notate all fields that exist in a given table. Sometimes, however, it can be useful to look at the distinct values in a database. Distinct values are unique values. For example, think about a classroom. Many people have common names. To get the first names of students, we might write the follwing query:  
```  
SELECT first_name  
FROM students;  
```  
This SQL tells Postgres that we would like it to return all the values for the student's first name that are stored in the students table. But, what if we wanted to know the distinct first names in the classroom? Distinct means that regardless of if the value occurs more than once, we only want to return the given value one time.  
```  
SELECT DISTINCT first_name  
FROM students;  
```  
  
#### Limiting your data  
To specify creiteria to lomit the records returned by our query, we can use the WHERE keyword. The WHERE clause is made up of the WHERE Keyword and any limiting criteria. The simplest criteria is when a field is equal to a particular value.    
For example:  
```  
SELECT first_name, last_name  
FROM person  
WHERE first_name = "Shelby";  
```   
   
In addition to querying for records that have a particular field that equals a specific value, we can use a variety of other comparison operators. These operators are grounded in logic and relational algebra. We have:  
1. Equal =  
2. Not Equal <>  
3. Greater than >  
4. Less than <  
5. Greater than or Equal to >=  
5. Less than or Equal to <=  
We call these comparison operators because they tell PostgreSQL to compare the specified field against a specified value.  
```  
SELECT city, state
FROM country
WHERE city = "Loiusville"
AND state = "KY";
```  
Relational operators require a specific comparison value, however, the LIKE keyword is a logical operator that allows us to find records where a field matches a specific pattern.    
```  
SELECT first_name, last_name
FROM person
WHERE first_name LIKE "Shelby";
```  
Importantly, the pattern can include not only regular characters, but also wildcard characters. Introducing wildcards allows us a great deal of flexibility when choosing our criteria. There are two wildcards that can be used with the LIKE keyword:  
1. The percent sign (%) represents zero or more characters.   
2. The underscore (_) represents exactly one character.  
```  
SELECT city 
FROM country
WHERE city_name LIKE '____, %';
```  
PostgreSQL allows us to select the data that does not match the pattern, by using the NOT LIKE keyword. The type of pattern matching performed by the LIKE keyword is known as FUZZY matching. It allows us to deal with those data that are less than perfect. PostgreSQL also allows the use of regular expressions for more complicated pattern to match various character combinations.   
#### NULL   
PostgreSQL inclued two criteria statements specifically for dealing with null values. Null is a special character in SQL and relational databases. When you work with certain programming languages, we often consider null to be equivalent to zero. However, this is not true in SQL. In database language, null is not a specific value like 0 or a blank space. Rather, null is an indicator of sorts. It indicates when a field has a missing or unknown value. A field is null only when no data of any kind has been entered into that field.  
#### Logical Operators  
The AND keyword is a logical operator that simply means that all of the conditions must be true. If a row from the table matches both conditions, it will be included. The OR keyword says that either this condition OR that condition must be true. Depending on out criteria, there are two special keywords that can make using logical operators even easier. The first of these is BETWEEN. For example:  
```  
SELECT first_name, age
FROM person
WHERE age BETWEEN 18 and 45; 
```  
Instead of using multiple OR statements, we cna use the SQL keyword IN. Let's say that we are looking for specific students based in their first name. Instead of typing:    
```  
SELECT first_name, age FROM person WHERE first_name = "Jimmy" OR first_name = "Martha" OR first_name = "Jacob";
```  
 we can write the query as:  
 ```  
 SELECT first_name, age FROM person WHERE   
 first_name IN ("Jimmy, Martha, Jacob");  
 ```  
 ### Operator Precedence  
 When we use logical operators to combine multiple criteria, we need to be aware of  a SQL concept called operator precedence. In PostgreSQL, by default, AND has a higher operator precedence than OR. It means that an AND statement will be evaluated before an OR statement, regardless of the order we list them in the WHERE clause. We can use parantheses when writing complex expressions.  
  
 ## Joining for futher insights  
 An important use of SQL is the ability to combine records and data from multiple tables. One of the most common ways to do this is by using joins. Joining data is what makes a relational database truly relational. When we join tables in SQL, we combine tables based on the information that is common or shared between the tables. There are three primary types of Joins: inner joins, outer joins and full joins. When we use a relational database, each table stores information about particular segments or types of data. For example, we may have one table that stores customer information, and another table that stores order information. How do we match the information between these two tables? In SQL, information is joined through the common fields, or keys. Keys are fields that describe relationships between tables. There are two types of keys, primary keys and foreign keys. A primary key is a column or a set of columns that when taken together have no duplicate values across rows. The primary key uniquely identifies each record in the table. A foreign key is a set of one or more columns in a table that refers to the primary key of another table. The foreign key in a table is used to point to data in another table. In the customer table, for example, customer_id is a primary key. This ID number uniquely identifies a row of information that is applicable to a specific customer. In the orders table, order_id is a primary key. This id number uniquely identifies a row of information that pertains to a specific order. Notice that the order table also includes the customer_id column. This customer_id allows us to join customer information to the order information.    
 ![Keys](https://github.com/vidushi-bansal/Postgres/blob/main/Keys.png)  
 #### Inner Join  
 An inner join returns all rows from two or more tables that meet our joined conditions. The fields that are being joined must exist and match in both of the tables being joined. For example:    
```  
SELECT customers.*, orders.*  
FROM customers  
INNER JOIN orders  
ON customers.customer_id=orders.customer_id  
WHERE customer.last_name = 'Dodd';  
```  
By default Postgres consider JOIN to be INNER JOIN.    
#### Outer Join  
Outer joins can either be a left outer join or a right outer join. The direction specifies which table we want to have precedence. The lest join selects all records from the left table along with any records that exist in the right table that meet the join condition. In right outer join we do just the opposite. For example, if we want to get the names of all the customers whether or not they have placed any order, but if they have, we want to know their order details as well.  
```  
SELECT c.first_name, c.last_name, o.order_date, o.order_amount 
FROM customers c
LEFT OUTER JOIN orders o
ON c.customer_id = o.customer_id;
```  
#### Full Join  
A full join will return all records from the lest table and all records from the right table, regardless of whether or not the join condition is met. If there is no match from one table or the other, the missing side will have values indicated as null. Remember that in our customers table, we have customers who have never placed an order. In our orders table, we also have the one order that does not have a matching customer ID. If we want to retrieve all rows of data from both tables, including those records without fully matching values, we could write this query.  
```  
SELECT c.first_name, c.last_name, o.order_date, o.order_amount 
FROM customers c
FULL OUTER JOIN orders o
ON c.customer_id = o.customer_id;
```  
  
Joins are frequently used with databases tables know as lookup tables. Lookup tables, or validation tables, are database tables that contain data to determine the value of codes.    
![Joins](https://github.com/vidushi-bansal/Postgres/blob/main/Joins.png)  
  
### Presenting and Aggregating your results  
When we execute a query against a Postgres database, the database returns the results in a seemingly random order. In fact, the data is returned in the order it os stored in the database. We can, however, sort these results using the ORDER BY keyword. For example,  
```  
SELECT name, state  
FROM residency  
ORDER BY name;  
```  
If we do not list the sorting direction, ascending is the default order. We can specify the directions as:    
```  
SELECT name, state  
FROM residency  
ORDER BY name DESC;  
```    
Postgres also allows us to store by more than one column. For example, we cna sort by both state of residency and name by:    
```  
SELECT name, state
FROM residency
ORDER BY state DESC, name  ASC;
```  
This statement will sort first by state in descending order, but if some rows have the same state of residence, it will order them by name in ascending order.  
### Aggregate Functions  
SQL offers a host of functions known as aggregate functions. An aggregate functions performs a calculation accress a set of values to return a single value. Some of these functions include COUNT, which counts rows in a specified table or view; SUM, which calculates the sum of a given set of values; AVG, which calculates the average of a set of values; MIN, which finds the minimum value in a set of values, and MAX, which finds the maximum value in a set of values. For example,  
```  
SELECT AVG(age) AS avg_Age
FROM person;
```  
But what if we want to know the avg age per grade level.     
```  
SELECT grade_lvl, AVG(age) AS avg_age
FROM person
GROUP BY grade_lvl;
```  
### Filtering aggregate results
WHERE is designed to filter songle rows. HAVING, on the other hand, is designed to filter groups or aggregates. Returing to our table of students, grade levels and ages, perhaps we are only interested in only those grade levels where the average age is less than 19.  
```  
SELECT grade_lvl, AVG(age) AS avg_age
FROM person
GROUP BY grade_lvl
HAVING AVG(age) < 19;
```  