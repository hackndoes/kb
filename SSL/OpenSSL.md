# OpenSSL
## Generate Self-Signed certificate
1. Generates a 2048-bit (recommended) RSA private key
```s
openssl genrsa -out key.pem 2048
```
2. Generates a Certificate Signing Request, which you could instead use to generate a CA-signed certificate. This step will ask you questions; be as accurate as you like since you probably aren’t getting this signed by a CA
```s
openssl req -new -sha256 -key key.pem -out csr.csr
```
3. Generates a self-signed x509 certificate suitable for use on web servers. This is the file you were after all along, congrats!
```s
openssl req -x509 -sha256 -days 365 -key key.pem -in csr.csr -out certificate.pem
```
4. The check at the end ensures you will be able to use your certificate beyond 2016. OpenSSL on OS X is currently insufficient, and will silently generate a SHA-1 certificate that will be rejected by browsers in 2017. Update using your package manager, or with Homebrew on a Mac and start the process over.
```s
openssl req -in csr.csr -text -noout | grep -i "Signature.*SHA256" && echo "All is well" || echo "This certificate will stop working in 2017! You must update OpenSSL to generate a widely-compatible certificate"
```
[Original Blog](https://msol.io/blog/tech/create-a-self-signed-ssl-certificate-with-openssl/)