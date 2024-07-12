>[!success] Use view as a consistent interface
>"Views allow you to encapsulate the details of the structure of your tables, which might change as your application evolves, behind consistent interfaces."

Views can be used in almost any place a real table can be used. Building views upon other views is not uncommon.[^1]

Unless indexed, _**a view does not exist as a stored set of data values**_ in a database. The rows and columns of data come from tables referenced in the query defining the view and are produced dynamically when the view is referenced. [^2]

>[!info] Just like numpy/torch views
>So, in computation via tensors, we do not use stored data all the time. We create views of the original tensors, and only do the computation when the data itself needs to be actualized. This way, we don't need to call the data from memory, compute over rows or columns that we don't need all time, and just do the computational graph once we decided what view represents.
> 
>PS: Dask uses this logic which is also used for distributed computing. 

Here are some of the use cases of view in regard with users[^2]:
1. Views are generally used to focus, simplify, and customize the *perception each user* has of the database.
2. Views can be used as *security mechanisms* by letting users access data through the view, without granting the users permissions to directly access the underlying base tables of the view.
3. Views can be used to provide a *backward compatible interface* to emulate a table that used to exist but whose schema has changed.

>[!example]
>A cool example on transition from an older table scheme to a newer one: https://stackoverflow.com/a/2680361/7606121

>[!note] Read More
>Someone mentioned that I need to read this and currently I have no idea what it means: http://www.codinghorror.com/blog/2005/05/stored-procedures-vs-ad-hoc-sql.html.
>

>Given the host is dead, I have saved the latest version via archive: ![[Stored-Procedures-vs-Ad-Hoc-SQL-(12_07_2024 5_29_51-pm).pdf]]


>[!warning] Don't make evil views
>Views can be evil if used badly. People who use them badly the most frequently seem to be those who are using them to abstract things and then do as you say, call views that call views that call views to the point where _**you may need to materialize 10 million records before returning a result set of 3**_. Views directly calling tables can be very useful.
– [HLGEM](https://stackoverflow.com/users/9034/hlgem "95,941 reputation") [Commented Sep 10, 2011 at 16:14](https://stackoverflow.com/questions/2680207/what-is-a-good-reason-to-use-sql-views#comment8899459_7318889)


[^1]: https://www.postgresql.org/docs/current/tutorial-views.html
[^2]: https://learn.microsoft.com/en-us/sql/relational-databases/views/views?view=sql-server-ver16