# Sed

## RegEx replace strings in files
``` sh
# Store the 'instance_filters = tag:environment=' string in group 1 and rebuild/replace the string with instance_filters = tag:environment=${DEST_INVENTORY}
sed -i.bak -E "s/(instance_filters = tag:environment=).*/\1${DEST_INVENTORY}/g" "inventories/${DEST_INVENTORY}/ec2.ini"
```

# Tr
## Remove new line characters from files
```bash
tr -d '\n' < filename.ext
```