#Google Cloud.md
```
sudo -i
vi /etc/ssh/sshd_config

# Authentication:
PermitRootLogin yes //默认为no，需要开启root用户访问改为yes
# Change to no to disable tunnelled clear text passwords
PasswordAuthentication yes //默认为no，改为yes开启密码登陆

/etc/init.d/ssh restart
```
