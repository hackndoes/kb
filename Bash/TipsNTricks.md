# SH
Bourn Shell script related Tips and Tricks

## Conditionals
Inside an if conditional the equality operator is a single equal sign
``` sh
if [ "$var" = "str" ]; then
    echo "equal"
fi
```

## Loops

### Until
``` sh
res=-1
until [ res == 0 ]
do
  command
  res=$?
done
```