# Generate a key pair

```bash
gpg --generate-key
```

# Send public key for someone to encrypt data with
```bash
gpg --export --armor identity@email.com > public.asc
```
send public.asc to someone to encrypt something for you specifically

# Add remote user identity public key to be able to encrypt files for him/her
```bash
gpg --import /path/to/received/public/key/public.asc
```

# Encrypt a file using a public key for identity
```bash
gpg -e -r identity@email.com /path/to/file/for/encryption
```

# Decrypt a file encrypted for your identity with your provided public key
```bash
gpg -o /path/to/decrypted/output/file -d /path/to/encrypted/file
```