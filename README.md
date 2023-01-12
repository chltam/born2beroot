# born2beroot

## password policy:

`sudo apt-get install libpam-pwquality` to install the moudule

config: /etc/pam.d/common-password
`minlen=10`

`ucredit=-1`: must contain atleast one uppercase, no credit for uppercase

`lcredit=-1`: lowercase and ^

`dcredit=-1`: digit and ^

`maxerepeat=3`: more than 3 consecutive identical characters

`reject_username`: must not contain username

`difok=7`: atleast 7 char not from the old password 

pam-pwquality man: https://man.archlinux.org/man/pam_pwquality.8
