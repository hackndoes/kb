# Return Values and Exits

## return with value other than 0 will fail the script if running with set -e

``` sh
#!/bin/bash -e
function vault_is_encrypted(){
    ansible-vault view "inventories/ssh/awsdev-1cpu/id_rsa_ansible" >/tmp/Error 2>&1
    OUT=$(</tmp/Error)
    rm /tmp/Error
    if [[ $OUT == *"not vault encrypted"* ]]; then
      return 1
    else
      return 0
    fi
}
 
 vault_is_encrypted
 ENCRYPTED=$?
 echo "ENCRPYTED=$ENCRYPTED"
 ```

 ## control behavior when returning with error occuring in the function level scope
 ``` sh
#!/bin/bash -e
function vault_is_encrypted(){
    set +e #disable error propegating out to script
    ansible-vault view "inventories/ssh/awsdev-1cpu/id_rsa_ansible" >/tmp/Error 2>&1
    OUT=$(</tmp/Error)
    rm /tmp/Error
    if [[ $OUT == *"not vault encrypted"* ]]; then
      return 1
    else
      return 0
    fi
    set -e #reenable exit on error but function will simply return the result 1 or 0 without killing the script
}
 
 vault_is_encrypted
 ENCRYPTED=$?
 echo "ENCRPYTED=$ENCRYPTED"
 ```