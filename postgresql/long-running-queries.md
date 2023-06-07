# Long running queries

To find all running queries the following query can be used:

```sql
SELECT
  pid,
  now() - pg_stat_activity.query_start AS duration,
  query,
  state
FROM pg_stat_activity
WHERE duration > interval '5 minutes';
```

To kill the process gracefully use:

```sql
SELECT pg_cancel_backend(pid);
```

Equivalent to a [SIGINT](https://www.baeldung.com/linux/sigint-and-other-termination-signals#sigint).

To kill the process forcefully use:

```sql
SELECT pg_terminate_backend(pid);
```

Equivalent to a [SIGTERM](https://www.baeldung.com/linux/sigint-and-other-termination-signals#sigterm-and-sigquit).

[Reference documentation](https://www.postgresql.org/docs/15/functions-admin.html#FUNCTIONS-ADMIN-SIGNAL)
