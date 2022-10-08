# MySQL

Standard select query:

1. Add statement to variable e.g `stmt := SELECT * FROM Users WHERE id = ?`
2. Execute the statement add assign to a variable e.g. `rows := db.queryRow(stmt, id)` - query will take the sql as param 1 and any values after will be assigned to the placeholder values in the query. We need to make sure the number of values passed === number of values in query.
3. Create new variable with a reference to the Users struct e.g. `u := &Users{}`
4. We can use the .Scan function to assign all columns retrieved to the new variable `u` e.g. `rows.Scan(&u.Username, &u.Email)`
5. We can then return `u` and be done with the query and function (make sure all error is properly added)
6. NOTE - We can shorten the query by chaining the `.Scan` onto the end of the `queryRow()` e.g. `err := db.queryRow(sql, values).Scan(&u.Username, &u.Email)` - Here we will do error handling and if all good - we can return `u, nil` as the `.Scan()` has worked. This is possible because errors from `queryRow` are deferred until `.Scan()` is called. Shortening the function seems to be the better choice
