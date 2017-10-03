# SH
Bourn Shell script related Tips and Tricks

## Conditionals

### Inside an if conditional the equality operator is a single equal sign
``` sh
if [ "$var" = "str" ]; then
    echo "equal"
fi
```
### String contains 
#### you want to know if a string contains a substring
``` sh
string='my ery long string'
if [[ $string == *"ery long"* ]]; then
  echo "It's threre!"
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

### With timeout for loop break
``` sh
  timeout=$((SECONDS+60))
  log "Wait until ops is available for ansible configuration"
  res=-1
  until [ $res = 0 ]
  do
    # break after timeout of 1 minute maximum
    if [ $SECONDS -gt $timeout ]; then break; fi
    log "No ping yet..."
    ansible -i inventories/${DEST_INVENTORY} -m ping ops
    res=$?
  done
```