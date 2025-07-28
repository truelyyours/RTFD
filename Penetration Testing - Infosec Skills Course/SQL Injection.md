In the context of SQL injection, ORDER BY n -- // is typically used to determine the number of columns in a SQL query's SELECT statement when the exact structure of the query is unknown to the attacker.(where n is an integer)
## Union Based SQL Injection

Union-based SQL injection is a type of SQL injection attack that leverages the UNION SQL operator to combine the results of two or more SQL queries into a single result set. This technique is used when an application is vulnerable to SQL injection and allows an attacker to retrieve information from the database that they are not authorized to access.

Enter the following command to retrieve information about the database:

`' union select null, null, database(), user(), @@version -- //`

