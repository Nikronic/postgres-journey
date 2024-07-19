This section is particularly has been written to put *aggregate* functions in direct comparison to *window* function. In other words, why even use *window* function and where it is best suited. So, in examples, I argue what *aggregate* functions fail to do, and propose the solution via *window* functions.

Consider we have the following example throughout this section[^1]:

```shell
select * from work;
 day | start | duration
-----+-------+----------
   1 |     7 |        5
   2 |     9 |        2
   3 |     8 |        2
   4 |     9 |        3
   5 |    10 |        4
   6 |     8 |        5
   7 |     8 |        2
   8 |    12 |        0
(8 rows)
```



Now imagine you want to add sum of all duration as a new column to the table.

Via *aggregation* and [[groupby-concept|groupby]] methods, what you get is the repetition of the `duration` column instead of sum. The reason is that you have to group values and since you have to group both `day` and `start`, you literally get the exact same table.

```sql
select day, duration, sum(duration) from work group by (day, duration);
```
```shell
 day | duration | sum
-----+----------+-----
   7 |        2 |   2
   5 |        4 |   4
   1 |        5 |   5
   2 |        2 |   2
   6 |        5 |   5
   4 |        3 |   3
   3 |        2 |   2
   8 |        0 |   0
(8 rows)
```

>[!note] 
>So *row-wise* grouping that is the way of `groupby` doesn't work. We need to be able to *aggregate the entire column (i.e., column-wise)*.

So, the `over` command allows us to *define a window over a column*:

```sql
select day, duration, sum(duration) over() as total_duration from work;
```

```shell
 day | duration | total_duration
-----+----------+----------------
   1 |        5 |             23
   2 |        2 |             23
   3 |        2 |             23
   4 |        3 |             23
   5 |        4 |             23
   6 |        5 |             23
   7 |        2 |             23
   8 |        0 |             23
(8 rows)
```

![[1_KP_6eui8I3-00T3ufiQQUQ.gif]]
*image courtesy of [^1]*

Now, if you want to limit the rows of this column-wise selection, you can use `partition by` to define the grouping rows, by selecting the column you want to do the grouping on.

For example:

```sql
select day, start, duration, sum(duration) over(partition by start ) as start_total_duration from work;
```

```shell
 day | start | duration | start_total_duration
-----+-------+----------+----------------------
   1 |     7 |        5 |                    5
   7 |     8 |        2 |                    9
   3 |     8 |        2 |                    9
   6 |     8 |        5 |                    9
   2 |     9 |        2 |                    5
   4 |     9 |        3 |                    5
   5 |    10 |        4 |                    4
   8 |    12 |        0 |                    0
(8 rows)
```

![[1_RELgQECGj-kjhUu6lOQMZA.gif]]
image courtesy of [^1]



>[!note] `over(order by)` is a *running window*
>Instead of partition which just takes a slice of window, one can *gradually expand the window to the same size as the partition* (instead of an instant slice). Just like *running sum* which takes one row at a time.

For example, expanding the `partition by` example:

```sql
select day, start, duration, sum(duration) over(partition by start order by day) as sum_duration_sort_by_day from work;
```

```shell
 day | start | duration | sum_duration_sort_by_day
-----+-------+----------+--------------------------
   1 |     7 |        5 |                        5
   3 |     8 |        2 |                        2
   6 |     8 |        5 |                        7
   7 |     8 |        2 |                        9
   2 |     9 |        2 |                        2
   4 |     9 |        3 |                        5
   5 |    10 |        4 |                        4
   8 |    12 |        0 |                        0
(8 rows)
```

**![[1_fA2TuRBufUwxaPQxPvOJAw.gif]]**
image courtesy of [^1]


>[!note] `rows` (instead of `partition by`) allows *sliding window*
>So, if we want a fixed size window to move over rows instead of defining groups based on rows, we can do that via `rows` and `range`. Of course, in this case, the context is fixed but it carries business value or domain specific knowledge.

For example imagine we want to know how much we work over a 5-day period (work week). In this case we want a sliding window with fixed height of 5 moving over all rows:

```sql
select day, duration, sum(duration) over(rows between 1 preceding and current row) as moving_sum from work;
```

```shell
 day | duration | moving_sum
-----+----------+------------
   1 |        5 |          5
   2 |        2 |          7
   3 |        2 |          4
   4 |        3 |          5
   5 |        4 |          7
   6 |        5 |          9
   7 |        2 |          7
   8 |        0 |          2
(8 rows)
```

![[1_R6ZZUMqi1mytYztLxOLcng.gif]]
image courtesy of [^1]


>[!note] Combine `order by` and `rows` to have a *fixed running window*
>So,
>1. `order by` gives you a *running* window that starts by size 1 and increases to the size of the group you have partitioned with
>2. `rows` gives you a *fixed* window that takes a fixed amount of items
>3. Now, you can have a *fixed running* window, which makes the running window to be limited by the fixed size defined. In other words, *the running window never expands beyond the `rows` size*

![[1_CKUa6tmvyRS9ROZQcmOZEw.gif]]
image courtesy of [^1]


>[!success] Window functions are *column-wise* operations
>So:
>1. select the entire column you want to aggregate via `over`
>2. select a subset of rows (if you want to limit the selection window of the column from the previous step), by choosing to `partition by`. Here, you can limit the rows by *partitioning by* any column you like.
>>[!note] 
>>If you `partition by day` you get the same result as initial `group by` example which repeats the `duration` column. The reason is that `day` is unique and hence, you creating a window with with of 1 given the uniqueness of the column `day`
>3. If you need *running (increasing) window* (on top of partitioned rows), you can use `order by`.
>4. If you like to have a *fixed absolute window* (instead of a contextually built one), you can use `rows` and `range` commands. *`rows` and `range` allow definition of a sliding (moving) window*.
>5. If you like to have a *running fixed window*, combine step (3) and (4)



[^1]: all examples are taken from https://medium.com/learning-sql/sql-window-function-visualized-fff1927f00f2. By the way, this is the first article of his! legendary.