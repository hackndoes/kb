# Ternary Operator

## SRC: [ how to if correctly in bash](http://stackoverflow.com/questions/669452/is-preferable-over-in-bash-scripts)
```sh
DEBUG_STRING=$([ "$DEBUG" = "yes" ] && echo "-vvv" || echo "")
```

```bash
DEBUG_STRING=$([[ "$DEBUG" == "yes" ]] && echo "-vvv" || echo "")
```