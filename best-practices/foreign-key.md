>[!success] The problem is that people make mistakes. Computers do not.
>Foreign key is important for maintaining **referential integrity**.

So, even assuming you are a legendary engineer, manually preserving the data integrity, the system is still prune to mistakes. [^1]

Other benefits of a foreign key (FK)[^2]:
- Foreign keys also serve as documentation, in that you can see what relates to what. This information is typically also used by tools, such as for generating reports, creating data sets from table definitions, object-relational mappers, etc.
- Foreign keys also allow you to define cascade rules, which e.g. can be used to to delete associated records in related tables when a row in one table is deleted.
- Setting up foreign key enables having a "good" data (important for me as ML engineer)

>[!question] Why not place all the data into a single table?[^3]
>We could as well place all the data into one table. That is true; however, then we wouldn’t be able to segregate the data into different categories within one table. That’s the reason why foreign keys are such a crucial part of database design. They allow us to _**place related data into multiple tables and then link them together to keep their integrity**_.

>[!example] 
>![[foreign-key-example-01.webp]]
>
>```sql
>CREATE TABLE Airplane (
AirplaneId VARCHAR(10) NOT NULL,
AirplaneBrand VARCHAR(30) NOT NULL,
AirplaneModel VARCHAR(30) NOT NULL,
CONSTRAINT PK_AirplaneId PRIMARY KEY (AirplaneId)
);
>```
>```sql
>CREATE TABLE Flight (
FlightId VARCHAR(10) NOT NULL,
AirplaneId VARCHAR(10) NOT NULL,
PilotId INTEGER NOT NULL,
CONSTRAINT PK_FlightId PRIMARY KEY (FlightId),
CONSTRAINT FK_AirplaneId FOREIGN KEY (AirplaneId)
REFERENCES Airplane(AirplaneId)
);
>```


[^1]: https://stackoverflow.com/a/5493818/7606121
[^2]: https://stackoverflow.com/a/5493822/7606121
[^3]: https://learnsql.com/blog/why-use-foreign-key-in-sql/
