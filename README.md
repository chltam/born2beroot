# born2beroot

## Random
super + right = new tty

`apt-get install net-tools -y`

du -ahx | sort -rh | head -20

`dpkg -l ` to check installed package

`sudo hostnamectl set-hostname new_hostname` set new host name

`dpkg -l | grep sudo` to check sudo

`su` to login as root

`shasum name.vdi` to create signature

APParmor vs SELinux: https://phoenixnap.com/kb/apparmor-vs-selinux

`wc -l` `wc -w`: `-1` counts for lines amd `-w` counts for words

how to ssh into VB machine https://averagelinuxuser.com/ssh-into-virtualbox/

![Screenshot from 2023-01-13 16-32-57](https://user-images.githubusercontent.com/114371464/212358044-1d373edc-d8de-40a4-8a34-7e27fa274c2c.png)

switch from NAT to Bridge Adapter

`ip a` or `ifconfig` or `ip addr show` to check the ip address of the vm

`ssh username@ip -p 4242`

why linux eat all my ram: https://www.linuxatemyram.com/

`systemctl status ssh` or `sudo service ssh status` to check ssh status

`ssh-keygen -R hostIP` to remove a specific host in ~/.ssh/known_hosts

## Sudo
### Install sudo
`apt-get install sudo -y`

`usermod -aG sudo username` add user to the sudo group

### config sudoers
`/var/log` `touch sudo.log` if needed.

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

`root ALL=(xx:xx) xx` The first “ALL” indicates that this rule applies to all hosts.

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

`/etc/login.defs`: pw expire day

`minlen=10`

`ucredit=-1`: must contain atleast one uppercase, no credit for uppercase

`lcredit=-1`: lowercase and ^

`dcredit=-1`: digit and ^

`maxerepeat=3`: more than 3 consecutive identical characters

`reject_username`: must not contain username

`difok=7`: atleast 7 char not from the old password 

`enforce_for_root`: rules apply to root

root old pw wont be compare, so`difok` is not applied to root

`chage` man: https://linux.die.net/man/1/chage

pam-pwquality man: https://man.archlinux.org/man/pam_pwquality.8

## User and group
`sudo adduser new_user`: create and setup a new user

`sudo groupadd new_group`: create a new group

`sudo usermod -aG group_name user_name`: add user to a group, `-a` to add the user to an existing supplementary group, `-g` will change the user primary group, `-G` will change the user primary group AND remove the user previous supplementary group.

`getent`: get entries from a database

`getent group`: show all group

`groups user_name`: check what groups a specific user belongs to

## monitoring.sh
#### Architecture and Kernel
`uname -a`: print all infor of OS architecture and kernel version

or

`hostnamectl | grep Kernel`

`hostnamectl | grep Architecture`

Linux Architecture: https://www.interviewbit.com/blog/linux-architecture/

#### CPU

`grep "physical id" /proc/cpuinfo | sort | uniq | wc -l` count how many cpu on socket

`grep '^core id' /proc/cpuinfo | sort | uniq | wc -l` number of physical core

`nproc` total number processor

`grep "^processor" /proc/cpuinfo | wc -l` should have same number as `nproc`

Core and thread: https://www.guru99.com/cpu-core-multicore-thread.html

cpu, vcpu, core, logical core: https://superuser.com/questions/1257392/physical-vs-logical-vs-virtual-cores

#### RAM and disk usage

`cat /proc/meminfo` for detail meminfo

`free -m` memory usage in MB

`free -m | grep Mem | awk '{printf("%d/%dMB (%.2f%%)", $3, $2, $3/$2*100)}'`

`df -BG` to check disk usage in Gb

`df -BG | grep '^/dev/'` include path start with `/dev/` only since `udev` and `tmpfs` are actually located in RAM

udev, temps: https://askubuntu.com/questions/1150434/what-is-udev-and-tmpfs

/dev/ and subdir: https://unix.stackexchange.com/questions/18239/understanding-dev-and-its-subdirs-and-files
/dev/loop*: https://linuxhint.com/dev-loop-linux/

`df -BG | grep '^/dev/' | grep -v boot` exclude the files mount on boot, they are used to boot the OS.

`df -BG | grep '^/dev/' | grep -v boot | grep -v loop | awk '{fdg += $2} END {printf("%dG", fdg)}'` full size in G

`df -BM | grep '^/dev/' | grep -v boot | grep -v loop | awk '{udm += $3} END {printf("%dMB", udm)}'` used space in MB

`df -BM | grep '^/dev/' | grep -v boot | grep -v loop | awk '{fd += $2} {ud += $3} END {printf("(%.1f%%)", ud/fd*100)}'` usage calculated in MB

#### CPU usage
`vmstat 1 2` check the stat every 1s for 2 times

`vmstat 1 2 | tail -1` only show the last line

`vmstat | tail -1 | awk '{printf("%d%%", 100-$15)}'`: `$15` is the 15th field, which is the % of cpu idle time, 100-cpu idle time will be cpu usage

`top -bn1 | grep '^%Cpu' | awk '{printf("%.1f", 100 - $8)}'`

top and vmstat: https://codeahoy.com/compare/top-vs-vmstat

cpu usage: https://www.baeldung.com/linux/get-cpu-usage

#### Reboot time

`who -b | awk '{print($3, $4)}'` who boot

https://www.cyberciti.biz/tips/linux-last-reboot-time-and-date-find-out.html

`last -i` to check who historial login

#### LVM check

`lsblk | grep "lvm" |` check the line that contain lvm in `lsblk`

`lsblk | grep "lvm" | wc -l` count the number of result

`if [ $(lsblk | grep "lvm" | wc -l) -eq 0 ]; then echo inactive; else echo active; fi` if the count is 0, LVM is not active, else it is active.

`-eq`: comparison operator used in the [ ] construct

`fi`: keyword to close the if-else statement

https://askubuntu.com/questions/202613/how-do-i-check-whether-i-am-using-lvm

#### Number of active connection

`netstat -natp | grep ESTABLISHED | wc -l`

https://www.computerhope.com/issues/ch001079.htm

#### Number is users using the server

`users | wc -w`

#### IP and MAC

`hostname -I` IP

`ip a | grep ether | awk '{print($2)}'` MAC address

#### Sudo command executed
`journalctl _COMM=sudo | grep COMMAND | wc -l`

## Crontab
`sudo crontab -u root -e`: `-u root` specifies managing cron table for root, `-e` means edit

`*/10 * * * */usr/local/bin/monitoring.sh`

`sudo cronstart` `sudo cronstop` `sudo service cron stop`

