# Connections

## Pools

- Usually when working with mysql - we have a pool of connections that will be used to connect and exectue queries.
- It is important to manage connections correctly - ensuring they are opened and closed correctly. Not closing connections can lead to maxing out the connection/limit.

# Transactions

- We use transactions to ensure multiple queries (usually that relate on rely on eachother) use the same connection from the pool and that both succeed or fail.

```
  db.Begin()

  // execute query 1

  // execute query 2

  if (err) {
    db.Rollback()
  } else // all good
```

Either both queries were succesful or the db is unchanged and nothing happned.
- Transactions can be a good way to avoid deadlocks - if we need to lock a table and run multiple queries, this will ensure we are using the same connection from the pool and protect us.

# Null values

## Db best practices

- It is a good idea to completely avoid null values in the db. Setting `NOT NULL` and always allocating a sensible defualt value is the way to go.
