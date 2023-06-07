# In vs Equals for enums

**Disclaimer: When talking about enums in this post I'm not refering to Postgresql enumerated types but enums on the application level which are stored as integers in the database.**

When querying a large table with an enum in the where clause it can be faster to use the `IN` operator than the `=` operator. While the planning time of the `=` operator is much faster than the `IN` operator, the actual execution time is much slower.

Pseudo explain analyse for the `=` operator:

```
Group Aggregate
Sort (sort key is same as group key), method quicksort
-> Bitmap Heap Scan (same number of rows)
    Recheck Cond: ((very long condition string) AND condition a AND condition b)
    Rows Removed by Index Recheck: ~408.3k
    Filter: (very long filter with = operator on enum column)
    Rows Removed by Filter: 213
    Heap Blocks: exact=~40.7k lossy=~22.8k
    -> BitmapOr
        -> Bitmap Index Scan on triple column index
        -> BitmapAnd
            -> Bitmap Index Scan on single column index
            -> Bitmap Index Scan on triple column index

Planning time: 2.471 ms
Execution time: 18113.917 ms
```

Pseudo explain analyse for the `IN` operator:

```
Group Aggregate
Sort (sort key is same as group key), method quicksort
-> Bitmap Heap Scan (same number of rows)
    Recheck Cond: (very long condition string)
    Rows Removed by Index Recheck: ~818k
    Filter: (different very long filter with enum <> ALL('{array of excluded enum values}'::integer[]))
    Rows Removed by Filter: ~335.7k
    Heap Blocks: exact=~63.2k lossy=~67.9k
    -> BitmapOr
        -> Bitmap Index Scan on triple column index
        -> Bitmap Index Scan on single column index

Planning time: 4.668 ms
Execution time: 1107.252 ms
```

As can be seen from the examples above the `IN` example did some very different planning, namely:

1. Dropped two conditions from the recheck condition (the very long conditiong string is exactly the same for both of the examples), resulting in more rows being removed by the index recheck
2. Different filter with some enum magic in there which removed significantly more rows (yes the rows removed by the filter in the first example were actually that small)
3. Presumably, due to the previous two changes (number of rows that were dropped), the index scan on the triple column index was ommitted, along with the `BitmapAnd` operation, resulting in one less nesting level

Version used: 10.18
