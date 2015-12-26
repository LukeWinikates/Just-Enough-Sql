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

If we `SELECT * FROM items` after this, we'll see the following:

|item|
|-----------|
|nonfat milk |
|green tea |
|bag salad |
|orange juice|

The whole milk "row" has been been changed to "nonfat milk". What's scary here is that if we left off the `where` clause entirely, the effect on the table is much different.

`UPDATE items SET item = 'nonfat milk';SELECT * FROM items;`

|item|
|-----------|
|nonfat milk |
|nonfat milk |
|nonfat milk |
|nonfat milk |

Because we didn't specify any filtering rules for our query with `where`, our `UPDATE` affected every row in the table. `SELECT` and `UPDATE` have this property, as does the last of the four SQL verbs, `DELETE`

Delete does what you'd expect -- it completely removes rows from a table. If we need to remove something from our list -- maybe we already have a bag salad, we can do it like this:

`DELETE from items WHERE item = 'bag salad'`

```
Definitions: `query` and `clause`
A snippet of SQL that can be run is called a query. Queries are made up of clauses.
```


### Selecting and filtering multiple columns


Our first example has just one column, *item*. Shopping lists often have amounts, so we could add quantity too:

|item|quantity|
|-----------|----|
|whole milk | 1 Gallon|
|green tea | 2 Boxes |
|bag salad | 1 (12oz) Bag|
|orange juice| 1 Gallon| 
|Tomato Soup| 1 (8 oz) Can|


### Why Databases Usually Have Multiple Tables
We mentioned earlier that Databases can contain multiple tables, and usually do. One reason for this is illustrated well by another example-- cities and states.

Cities

|name|state|
|------|-----|
|Austin|Texas|
|Dallas|Texas|
|Boston|Massachussets|
|Tulsa|Oklahoma|

In this table, the full names of the states are written out, but we often refer to states by standard abbreviations. If we wanted to address an envolope to be mailed, for example, it'd be useful to have TX for Texas, MA for Massachussets, and OK for Oklahoma. We could do that like this:


|name|state|abbrev|
|------|-----|----|
|Austin|Texas|TX|
|Dallas|Texas|TX|
|Boston|Massachussets|MA|
|Tulsa|Oklahoma|OK|

What if we wanted to add the name of the capital for each state?

|name|state|abbrev|capital|
|------|-----|----|-----|
|Austin|Texas|TX|Austin|
|Dallas|Texas|TX|Austin|
|Boston|Massachussets|MA|Boston|
|Tulsa|Oklahoma|OK|Oklahoma City|

This is starting to get more awkward, because of duplication. We're duplicating the abbreviation and capital city for each state. Our table of cities will have one row for every city we're keeping track of -- potentially thousands of cities in the U.S. But there are only 50 states, so we're needlessly repeating the capitals everywhere, and if we were keeping track of more state-level information, like the current Governor, we'd have to update the governor in multiple rows. There's a good solution to this: multiple tables.

**CITY**

|name|state|
|------|-----|
|Austin|TX|
|Dallas|TX|
|Boston|MA|
|Tulsa|OK|

**STATE**

|abbreviation|name|capital|
|------|----|-----|
|TX|Texas|Austin|
|MA|Massachussets|Boston|
|OK|Oklahoma|Oklahoma City|

This makes our city table smaller by referring to the state by its identifier. The state table will only have 50 or so entries (remember DC, Puerto Rico, and territories like Guam and the US Virgin Islands). But what if we wanted to find the state capital that governs Dallas? For that, we need to use a `JOIN` to read data from two tables at once.

```
SELECT city.name, state.capital
FROM city
JOIN state
ON city.capital = state.abbreviation
WHERE city.name = ''
```

The result will look like this:

|name|capital|
|-|-|
|Dallas|Austin|


```
A programmer apologizes for: different "flavors" of SQL

SQL is not a particular database -- it's the name for a language. There are lots of SQL products that try to implement the SQL language, but for various reasons their versions of SQL are slightly different. We sometimes call these "variants", "dialects", or "flavors" of SQL.
There's an official "standard" specification for the language, called ANSI SQL, but most SQL databases don't implement the standard perfectly. Oracle in particular deviates the most from the ANSI SQL standard, partly because the original Oracle database was created *before* SQL itself was standardized! So although many folks at Oracle probably recognize that ANSI SQL is better than Oracle's legacy SQL variation, it would probably create problems for some of their customers if they tried to make extensive changes, so Oracle keeps its version of SQL non-standard for backward compatibility. MySQL is also nonstandard, while PostgreSQL and Microsoft SQL Server are closer to the standard.

What this means is that you'll want to include the name of the SQL product you're using in your google searches. If you need a refresher on the syntax for creating a table and you're using MySQL, you should probably google "mysql create table", and look for documentation about the specific version you're using too.

```


### "IDs" and other kinds of Primary Keys
One problem with this table right now is that if there are two entries for "eggs" and we want to change just one of them, we can't single it out.


## Finding your way around in a new Database


## Changing the format of data, or accessing data that's not in tables
### Time
### Random


