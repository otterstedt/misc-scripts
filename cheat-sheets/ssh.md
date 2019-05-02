# SSH FS

## Configure Mount at Boot
### /etc/fstab
```
<ssh alias>:/path/remote/folder /path/local/folder fuse.sshfs reconnect,delay_connect,defaults,idmap=user,port=22,uid=<user id>,gid=<group id>,allow_other,cache=no,noauto_cache 0 0
```
### ~/.ssh/config
```
Host <ssh alis>
HostName <remote server>
User <user name>
ServerAliveInterval 15
ServerAliveCountMax 3
IdentityFile <path to ssh key>
```
### system d controlled tunnels
```
[Unit]
Description=Some Tunnels
After=network.target ssh.service

[Service]
ExecStart=/usr/bin/ssh -NT <ssh config alias>
User=<user>
RestartSec=5
Restart=always

[Install]
WantedBy=multi-user.target 
```

