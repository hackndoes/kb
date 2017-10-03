# A way of declaring variables and inialize them
``` go

type person struct {
    fname string
    lname string
    Age int //will be visible outside the package (exported) with capital latter
}

xi := []int{1,2,3,4,5} //can have a comma after last thing in list but not required

m = map[string]int{
    "dan": 1, 
    "daniella": 2, //trailing comma required because its listen on multiple lines.
}
```

## Ordered struct initialization 
``` go
p1 := person{
    "firstname",
    "lastname",
    50,
}
```

## Named parameter initialization
```go
p1 := person{
    fname: "firstname",
    lname: "lastname",
    Age: 50,
}
```