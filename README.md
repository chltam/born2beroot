# born2beroot

## Random
super + right = new tty

du -ahx | sort -rh | head -20
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

