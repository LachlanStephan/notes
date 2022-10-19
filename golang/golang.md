# MySQL

github.com/go-sql-driver/mysql v1.6.0

## Standard select query:
1. Add statement to variable e.g `stmt := SELECT * FROM Users WHERE id = ?`
2. Execute the statement add assign to a variable e.g. `rows := db.queryRow(stmt, id)` - query will take the sql as param 1 and any values after will be assigned to the placeholder values in the query. We need to make sure the number of values passed === number of values in query.
3. Create new variable with a reference to the Users struct e.g. `u := &Users{}`
4. We can use the .Scan function to assign all columns retrieved to the new variable `u` e.g. `rows.Scan(&u.Username, &u.Email)`
5. We can then return `u` and be done with the query and function (make sure all error is properly added)
6. NOTE - We can shorten the query by chaining the `.Scan` onto the end of the `queryRow()` e.g. `err := db.queryRow(sql, values).Scan(&u.Username, &u.Email)` - Here we will do error handling and if all good - we can return `u, nil` as the `.Scan()` has worked. This is possible because errors from `queryRow` are deferred until `.Scan()` is called. Shortening the function seems to be the better choice

## Multi select
1. This time we ues `rows, err := db.Query(stmt)`. Two things need to happen here, we need to handle the error and defer/close the connection. We have to handle the error first - then call `defer rows.Close()`.
2. Create an empty slice of the related struct `tests := []*Test{}` - We will return this full of our query results if all goes well.
3. We need to iterate over the rows and assign the data to a slice pointing to the related struct
```
for rows.Next() {
    t := &Test{} // new pointer to Test struct
    err = rows.Scan(make sure all parameters here === selected columns in stmt)
    // handle err
    tests = append(tests, t)
}
```
4. After this it is important to handle the error potentially thrown by rows. If `rows.Err() != nil` then we can `return tests, nil` and all is good.

Notes: In a lot of these queries we will have be using joins. This means the struct of Tests cannot be just the columns in the Tests table. I think the most simple way forward is to add the columns of the joining tables to the Tests struct - The actual Tests struct that relates to the db will be kept elsewhere in the db layer.

# Errors / Error handling

## Error.Is()

This method takes two parameters.

1. The current err variable
2. The target err - what you suspect might be the error
   Used when you want to check for a specific error that may have occurred during the operation. This is good for in certain cases you may want to handle one error a certain way and the rest in a generic way. For example if you want to check that the error === no rows returned

```
    // check for no record returned
	if err != nil {
		if errors.Is(err, sql.ErrNoRows) {
			return nil, models.ErrNoRecord
		} else {
			return nil, err
		}
	}
```

