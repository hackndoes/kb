# Default var values if not supplied in running environment (like Jenkins vars)

## Take The current environment var 'MY_TEST_VAR' value and if not defined use default of 25
``` sh
MY_TEST_VAR=${MY_TEST_VAR:-25}
```

# Arithmetics
## Increment a var value

``` sh
var=$((var+1))
((var=var+1))
((var+=1))
((var++))
```