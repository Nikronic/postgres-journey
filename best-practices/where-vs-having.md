>[!info] Where --> group by --> having
>So, we use `where` to filter results _**before** being grouped and aggregated_, while we use `having` _**after**_ group rows and aggregation are calculated.

Although both `where` and `having` do the *conditioning*, but the important point is that `having` is required (logically noy syntactically) to be used with a *aggregation* method.

>[!success] Use all non-grouped conditioning via `where`
>So, given that grouping and aggregating is the most computational hungry task, any condition that is not targeting grouped data, done with `having` can be done *more efficiently with `where`*. In other words, the input rows to the grouping and aggregation methods are being filtered to be smaller.

Note that it is absolutely logical to assume that the aggregated/grouped value is always a subset of the original table, hence, filtering the original table through `where` is more efficient that doing the same conditioning via `having` [^1].

[^1]: https://www.postgresql.org/docs/current/tutorial-agg.html