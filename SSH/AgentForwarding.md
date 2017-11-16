# Use ssh agent forwarding to prevent the need to save private keys on managed machinies

### The following command will create an ssh session on host 'host' for user 'user' from which any ssh session started will query the ssh-agent on the host from which you logged in.
```sh
ssh -A user@host
```
    
### To open an ssh terminal directly to a remote machine
```sh
ssh -At user@host ssh remoteuser@remotehost
```
Where A configures agent forwarding
      t create a psuedo terminal from which you can create the second ssh session to the remotehost