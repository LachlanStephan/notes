# References

## Passing values 
I believe PHP passes via value which means that each variable will be copied locally into the function. This is not ideal when dealing with vector types (array, object) 
as we want to have a reference to the original object and not copy it (wasting space). 

<br/>

easy mistake to make:
```
$large_set_data = array() // assume this has heaps of data
functionCall($large_set_data);

public functionCall($large_set_data) {
  // copied data when we use it here
}
```

likely better option:
```
$large_set_data = array() // assume this has heaps of data
functionCall($large_set_data);

public functionCall(&$large_set_data) {
  // reference underlying data
}
```
