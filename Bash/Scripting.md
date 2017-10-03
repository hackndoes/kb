# Ternary Operator

## SRC: [ how to if correctly in bash](http://stackoverflow.com/questions/669452/is-preferable-over-in-bash-scripts)
```sh
DEBUG_STRING=$([ "$DEBUG" = "yes" ] && echo "-vvv" || echo "")
```

```bash
DEBUG_STRING=$([[ "$DEBUG" == "yes" ]] && echo "-vvv" || echo "")
```

# STDOUT/STDERR
## Capture command output to variable and operate accordingly
``` sh
  ansible-vault view "inventories/ssh/${DEST_INVENTORY}/id_rsa_ansible" >/tmp/Error 2>&1
  OUT=$(</tmp/Error)
  rm /tmp/Error
  if [[ $OUT == *"sub string"* ]]; then
    exit 0
  else
    exit 1
  fi
```

# Script Arguments
## Interactive arguments requested from user
``` sh
SRC_INVENTORY=${1}
DEST_INVENTORY=${2}
if [ -z "$SRC_INVENTORY" ]; then
  echo "Name of inventory to copy from: (only the name of the directory under inventories. eg. awsdev-cpu)"
  read -r SRC_INVENTORY
fi
if [ -z "$DEST_INVENTORY" ]; then
  echo "Name to use for created inventory: (the name of the output directory under inventories. eg. cpu-dan)"
  read -r DEST_INVENTORY
fi
```