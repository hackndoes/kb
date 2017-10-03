# Upload a signed certificate to amazon to use as resource

## list certificate info
```s
aws iam get-server-certificate --server-certificate-name certificate_object_name
```

## create a bundle certificate
```s
cat intermediate1.crt intermediate2.crt root.crt > ssl-bundle.crt
```

## upload the key
```s
aws iam upload-server-certificate --server-certificate-name certificate_object_name --certificate-body file://certificate --private-key file://privatekey.pem --certificate-chain file://certificate_chain_file
```