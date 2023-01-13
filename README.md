# born2beroot

## Random
super + right = new tty

du -ahx | sort -rh | head -20

`su` to login as root

how to ssh into VB machine https://averagelinuxuser.com/ssh-into-virtualbox/

![Screenshot from 2023-01-13 16-32-57](https://user-images.githubusercontent.com/114371464/212358044-1d373edc-d8de-40a4-8a34-7e27fa274c2c.png)

switch from NAT to Bridge Adapter

`ip a` or `ifconfig` or `ip addr show` to check the ip address of the vm

`ssh username@ip -p 4242`


`systemctl status ssh` or `sudo service ssh status` to check ssh status

`ssh-keygen -R hostIP` to remove a specific host in ~/.ssh/known_hosts

## Sudo
### Install sudo
`apt-get install sudo -y`

`usermod -aG sudo username` add user to the sudo group

### config sudoers
`/etc/var/log` `touch sudo.log` if needed.

`sudo visudo` or `vim /etc/sudoers` to edit the config file.

`sudo visudo` is more preferred since it validates the syntax upon saving.

`sudo update-alternatives --config editor` to change the default editor.

`Default  env_reset`: resets the terminal environment to remove any user variables.

`Default  mail_badpass`: mail notices to `mailto`user when bad sudo password attempts encountered.

`Default  secure_path=...`: restrict the PATH that will be used for sudo operations.

```
Defaults	badpass_message="incorrect password!"
Defaults	passwd_tries=3
Defaults	logfile="/var/log/sudo.log"
Defaults	log_input, log_output
Defaults	requiretty
```
### User privilege specification
`username ALL=(ALL:ALL) ALL`

`root <mark>ALL</mark>=(xx:xx) xx` The first field indicates the username that the rule will apply to (root).

`root xx=(ALL:xx) xx` This “ALL” indicates that the root user can run commands as all users.

`root xx=(xx:ALL) xx` This “ALL” indicates that the root user can run commands as all groups.

`root xx=(xx:xx) ALL` The last “ALL” indicates these rules apply to all commands.

`%sudo  ALL=(ALL:ALL) ALL` group name must start with `%` in the group privilege section

#### Reference: https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file

## UFW
`apt-get install ufw -y` to install ufw

`sudo ufw enable`

`sudo ufw status` 

`sudo ufw allow ssh` `ufw allow 4242`

`sudo ufw delete NUMBER` to delete specific port according to the number from`sudo ufw status numbered`

`sudo ufw delete allow 22/tcp` remove directly


## Password policy:

`sudo apt-get install libpam-pwquality` to install the moudule

config: /etc/pam.d/common-password

`minlen=10`

`ucredit=-1`: must contain atleast one uppercase, no credit for uppercase

`lcredit=-1`: lowercase and ^

`dcredit=-1`: digit and ^

`maxerepeat=3`: more than 3 consecutive identical characters

`reject_username`: must not contain username

`difok=7`: atleast 7 char not from the old password 

`enforce_for_root`: rules apply to root

root old pw wont be compare, so`difok` is not applied to root

pam-pwquality man: https://man.archlinux.org/man/pam_pwquality.8

## User and group
`sudo adduser new_user`: create and setup a new user

`sudo groupadd new_group`: create a new group

`getent`: get entries from a database

`getent group`: show all group

## Crontab

`apt-get install net-tools -y`

### monitoring.sh
`uname -a`: print all infor of OS architecture and kernel version

or

`hostnamectl | grep Kernel`

`hostnamectl | grep Architecture`

cpu, vcpu, core, logical core: https://superuser.com/questions/1257392/physical-vs-logical-vs-virtual-cores
`cat /proc/meminfo` for detail meminfo
`free -m` memory usage in MB
`free -m | grep Mem | awk '{printf("%d/%dMB (%.2f%%)"), $7, $2, $7/$2*100'}`
