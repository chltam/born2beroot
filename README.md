# born2beroot

## Random
super + right = new tty

du -ahx | sort -rh | head -20

`su` to login as root

![Screenshot from 2023-01-13 16-29-51](https://user-images.githubusercontent.com/114371464/212357397-a46a040f-e8ee-45f0-8657-bed1a6e9f088.png)

`ip a` or `ifconfig` or `ip addr show` to check the ip address of the vm


`systemctl status ssh` or `sudo service ssh status` to check ssh status

change the 
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

