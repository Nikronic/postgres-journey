>[!success] It's all the performance[^3]
>Since we like to have concurrent connections to database, when multiple client want to deal with the same data, conflict arises. That's where transactions come in.

Transaction properties[^1]:
1. **A transaction is said to be _atomic_**: From the point of view of other transactions, it either happens completely or not at all, and if some failure occurs that prevents the transaction from completing, then none of the steps affect the database at all.
>[!note] Just like a function
>So, if a function takes a value, do the manipulation but finally fails, the manipulations should be reverted and the input should stay the same. (no inplace operation if failed)

2. **A *completed* transaction must be permanent**: A transactional database guarantees that all the updates made by a transaction are logged in permanent storage (i.e., on disk) before the transaction is reported complete. So reports are only provided if the transaction is complete and no longer running.
3. **Atomic updates**: When multiple transactions are running concurrently, each one should not be able to see the incomplete changes made by others. The updates made so far by an open transaction are *invisible to other transactions* until the transaction completes, whereupon all the updates become visible simultaneously. So if one needs to read data that is written by another, it will have to wait until the other is finished.
>[!note] Like feature branching in Git
>So, when multiple transactions (feature branches) are simultaneously are working, their changes should not result in a *conflict* (per definition in Git); hence, when completing a transaction (merging), only other transactions (a different branch) can be completed. This way, no intermediate result from one branch can affect another branch, since only final merge (assume no rebase and a single merge commit is made) as *a single atomic* commit is being made. 

>[!Question] Why not use transactions all the time?
>Here is my question (I am asking before searching to have pure discussion and logical reasoning): Given the following reasons: 
>1. transactions behave like functions (technically perfect), and
>2. culturally work like feature branching it git (which I love and I think is the best way to manage code),
>
>why we aren't using transaction for every single query? In other words assume all queries are group of queries while some of them have only a single member (single line queries). This way we have the benefits. In simple terms, everything must be transaction by default and can't be turned off even.

>[!success] LOL, I was right
>PostgreSQL actually treats every SQL statement as being executed within a transaction. If you do not issue a `BEGIN` command, then each individual statement has an implicit `BEGIN` and (if successful) `COMMIT` wrapped around it.

So in the last line of my question, I said that "everything must be transaction by default and can't be turned off". But, we do have `SAVEPOINT` and `ROLLBACK TO` commands that enable creating *checkpoints like in video games*.

>[!info] `SAVEPOINT`/`ROLLBACK TO` works like `goto` or `jump` in programming languages
>It gives a lot of control given the possible conditioning for control within a transactional block.

>[!example]
>```sql
>BEGIN;
initial_query
SAVEPOINT my_savepoint;
bad_query
ROLLBACK TO my_savepoint;
fix_query
COMMIT;
>```


>[!Note] The relation between [ACID](http://en.wikipedia.org/wiki/ACID) and transactions[^2]
>ACID is a set of properties that you would like to apply when modifying a database. Including: 1) Atomicity, 2) Consistency, 3) Isolation, 4) Durability
>
>Transactions are tools to achieve the ACID properties.
>
PS: *make sure to read comments, specially about **consistency** in individual and transactional block level.*.


[^1]: https://www.postgresql.org/docs/current/tutorial-transactions.html
[^2]: https://stackoverflow.com/a/3740307/7606121
[^3]: https://www.metisdata.io/blog/transaction-isolation-levels-and-why-we-should-care