# Encrypt value to use in jenkins configurations

## I will use Github Authentication plugin

For example when you want to run a copy of a running jenkins from backup you will get redirected by 
jenkins Github Authentication plugin back to your old instance (it's configured in the github account).

what you could do is create a new application in your github account (that has a new clientid and secret key) and have it redirect to your new jenkins.
the problem is the value for secret key in config.xml is encrypted and you want to be able to replace it with the encrypted value if the new secret key for it to work and revert you back to your new instance after login.

so:
1. open jenkins script console
2. paste the following code into it 
```groovy 
import hudson.util.Secret
Secret a = Secret.fromString('my secret value')
String ciphertext = a.getEncryptedValue()
println ciphertext
```
3. replace both clientid and secret key in your github security realm to the new values
4. restart jenkins