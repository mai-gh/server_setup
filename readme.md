#### ssh key setup


```
KEYNAME='whatever'
mkdir output

ssh-keygen -b 4096 -f ./output/$KEYNAME-id_rsa

ssh-copy-id -i ./output/$KEYNAME-id_rsa.pub username@remote-server.org

ssh -i output/$KEYNAME-id_rsa username@remote-server.org
```


#### /etc/ssh/sshd_config

 - set `Port 22` to anything but 22
 - `PasswordAuthentication no`
 - `PubkeyAuthentication yes`
 - `PermitRootLogin no`
