#Just Enough SQL
##Database Fundamentals for Non-Coders

### Who this guide is for

If a database server was a business, its main customers would usually be automated computer programs like websites or the server software that supports applications. For that reason, there's a ton of resources about databases out there for software programmers that's interesting, but doesn't focus on the needs of another set of potential database users-- Business Analysts, Risk Analysts, and Managers. That's who this guide is for. People whose job it is to look at the data being collected by a business or other organization and use it to answer questions.

The main goals of this guide are:

* introduce just enough SQL knowledge for smart, patient non-programmers to be able to write SQL queries they might use.
* provide context about SQL so that readers know how to find more detailed information in other sources if they need it
* explain and help the reader to overcome the shortcomings of SQL software -- which is not designed for regular, busy people.

###The Purpose of Databases

It's fair to say that databases are about **lists**. Databases solve a similar problem to a Spreadsheet in Microsoft Excel, or even a Word or text document. If you're really good at Excel, you might already know how to use it filter and cross-reference data, which are some of the same things that SQL lets you do. So why learn a different way of doing the same thing, especially since Excel's a friendlier interface for it in many ways?

In other words, why do databases exist?

The main reasons are that database software can deal with **very very large data**, and that database software coordinates **lots of different users on lots of different computers** reading and changing data at the same time. If a database server was a business, its main customers would usually be automated computer programs like websites or the server software that supports applications. That's probably why the interface they offer isn't as friendly as desktop software like Excel that does the same things on a smaller scale. Large databases are concerned with dealing with 

Some people refer to very large spreadsheets as databases. For the purposes of this book, a database is not a spreadsheet but a central source of data, maybe on a *server*. The server will be running software like *MySQL*, *PostgreSQL*, *Microsoft SQL Server*, or *Oracle*. Unlike Excel, the main purpose of those applications isn't to provide a convenient desktop user interface to look at and filter data. Instead of showing you all the database records in a single grid like Excel, to get data from one of these databases, the normal way of doing it is to use *queries* written in a programming language called *Structured Query Language*, or *SQL*.


### A Simple Database Table
SQL Databases, which are also called *relational databases*, store data in *tables*. Database can have more than one table, and most do. Here's a simple table that you might use for storing a grocery shopping list:

|item|
|-----------|
|whole milk |
|green tea |
|bag salad |
|orange juice|


This is a small list, and it would be more practical to keep it in a text file or a spreadsheet than in a SQL database, but if there were tens of thousands of items (say shopping lists for many people, or all of your shopping lists from several years of trips), a SQL database would be practical.


### The Simplest Query
If we wanted to see all the times on our list, we would write a `SELECT` query like this:

```SQL
SELECT *
FROM items;
```


    A programmer apologizes for: semicolons
    Most SQL databases will expect you to indicate the end of your *query* with a semicolon (;). This is a common feature in programming languages dating back to the 1970s. It doesn't actually mean anything, and it would be nice if it was optional, since it's easy to forget. But most SQL software continues to require it for historical reasons. I'm sorry.

There are 4 important verbs in SQL -- SELECT, INSERT, UPDATE, and DELETE. Select is most important, because it's the one that reads data. If you couldn't read the data out, there'd be no point in having the database. INSERT adds new rows, while UPDATE changes values in existing ones. DELETE permanently destroys rows.

`SELECT * FROM items` is basically the simplest SQL query possible. It has two parts:

|SELECT *| FROM items|
|----|-----|
|Column list| Table name|

You may know the `*` character from other computer programs as a special character that means "anything". In a SQL `SELECT` query, * means "show me all the columns". Here, our table only has one column. Tables can have hundreds of columns! In those cases, it can be useful to specify a specific list of tables that you care about -- it's easier to read and saves network and server resources.

Here's a slightly more complicated query that shows us only the items that contain the letter "i", like this:

```
SELECT *
FROM items
WHERE item LIKE '%i%'
```

It has 3 parts.

|SELECT *| FROM items| WHERE item LIKE '%i%'|
|----|-----|
|Column list| Table name| filter|

This brings up something interesting about SQL. By default, filters are not required, and if you don't include one, your query affects **all rows in the table**. Some people consider this a major safety flaw with SQL as a programming language. Since this query only selects rows that are *LIKE* a pattern, "%i%", which means "zero or many letters, an i, then zero or many characters", it will match `milk` and `orange juice`.

    A programmer apologizes for: * vs %
    SQL uses * to mean "every column", and "%" to mean any character. This is inconvenient because their meanings are pretty similar, and % is actually a very common character in normal text. I'm sorry.


### Insert, Update, and Delete

`INSERT` is the other important SQL verb, because it's how data gets added to tables. If we wanted to add Zucchini to our list, we would do it like this:

`INSERT INTO items VALUES ('Zucchini')`

One interesting thing about this query is that we wrap single quotes around 'Zucchini' so that the SQL database knows it's text data and not a special keyword like INSERT or VALUES.  Although single and double quotes are equivalent in English, they mean different things in SQL, and only single quotes would work here.

`UPDATE` is used to change existing data. For example, if we changed our minds about whole milk, we could change it to nonfat.

`UPDATE items SET item = 'nonfat milk' where item = 'whole milk'`

If we `SELECT * FROM items` after this

What's important (and scary!) here is this very 


### Seleting and filtering multiple columns


Our first example has just one column, *item*. Shopping lists often have amounts, so we could add quantity too:

|item|quantity|
|-----------|----|
|whole milk | 1 Gallon|
|green tea | 2 Boxes |
|bag salad | 1 (12oz) Bag|
|orange juice| 1 Gallon| 
|Tomato Soup| 1 (8 oz) Can|






### "IDs" and other kinds of Primary Keys
One problem with this table right now is that if there are two entries for "eggs" and we want to change just one of them, we can't single it out.

