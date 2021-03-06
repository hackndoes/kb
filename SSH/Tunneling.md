# Tunneling

## Local Forwarding

### Use localhost to forward connection to a remote

* -f = run in background
* -i = identity file
* -N = don't execute remote command (only port forward)
* 34.252.171.12 = the remote host to login to
* 2022:172.16.200.228:22 = [local port]:[ip of host to forward to]:[port on forwarded host]

**This command will setup forwarding of ssh connection from localhost through 34.252.171.12
to 172.16.200.228:22**
so for instance if you have a VPC with one machine with a public IP from which access to other machines
is possible in a private subnet you can set it up like this and from your own machine easily connect to the remote machine

ssh -L <local-port-to-listen>:<remote-host>:<remote-port> <gateway>

```sh
ssh -f -L 2022:172.16.200.228:22 -i [identity file] ubuntu@34.252.171.12 -N
```

**now to connect to the machine you will run**
```sh
ssh -i [identity file] ubuntu@localhost -p 2022
```

### if you want to forward a local port to the remote host's port
```sh
ssh -L 5900:localhost:5900 home
```
5900 on localhost will forward packats to 5900 on the remote machine (home) using the localhost name
it's like ssh to home and than using localhost:5900

## Remote Forwarding

### Will configure a reverse tunnel to allow a connection from a remote using the machine that created the tunnel. An example is an office computer creating a tunnel to a home computer, later you can use office resources tunneled through the work computer since it has initiated the connection.

```sh
ssh -R 9000:internal.site.com:80 home
```
from home you can goto http://localhost:9000 and it will take you to the internal site @internal.site.com:80


[A Very Good And Simple Guide](https://chamibuddhika.wordpress.com/2012/03/21/ssh-tunnelling-explained/)