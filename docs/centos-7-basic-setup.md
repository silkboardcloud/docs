### Update OS with to Latest

```sh
sudo yum install epel-release -y
sudo yum update -y
```

#### with root login

```sh
useradd cent
passwd cent
# prompts for password, give your secret strong password
usermod cent -aG wheel 
```

#### Setup hostname 

if hostname doesn't end with domain name

mail from system may be rejected

```sh
sudo hostname server1.silkboard.in
### Verify using `hostname` -> it will show server1.silkboard.in
```
### Remove firewalld

```sh
sudo systemctl mask firewalld
sudo systemctl stop firewalld
sudo yum remove firewalld
```

### Setup Timezone

if you are from india execute below lines, otherwise check your timezone [here](https://www.thegeekdiary.com/centos-rhel-7-how-to-change-timezone/)
```sh
sudo rm /etc/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
```

#### Disable Root login

SSH server settings are stored in the `/etc/ssh/sshd_config` file.

To disable root logins, make sure you have the following entry `PermitRootLogin no`*

```sh
sudo sed -i '/#PermitRootLogin yes/c\PermitRootLogin no' /etc/ssh/sshd_config
sudo systemctl restart sshd
## next execute disable selinux 
```

### Disable SELinux
https://github.com/silkboardcloud/docs/blob/master/scripts/disable-selinux-on-cent-os-7.sh


```sh
sudo sed -i 's/enforcing/disabled/g' /etc/selinux/config /etc/selinux/config
sudo reboot
#### To Verify try ssh root@server1.silkboard.in --->> it should say **permission denied, please try again** after entering password
```
wait for sometime to reboot

## Login as **cent** user

```sh
ssh cent@server1.silkboard.com
```

#### Setup Git

```sh
sudo yum install git -y
git config --global user.name "$USER $(hostname)"
git config --global user.email "$USER@$(hostname)"
```

### Setup SSH Keys

```sh
ssh-keygen -t rsa -b 4096 -C "$(git config --global user.email)"
# first enter
# second enter
cat ~/.ssh/id_rsa.pub
```
 
 - add it to github(https://github.com/settings/ssh/new) if you need to pull code without password

#### Ensure ssh-agent is enabled

```sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```


### Install iptables

```sh
sudo yum -y install iptables-services
sudo systemctl start iptables
sudo systemctl enable iptables
```

### Configure iptables

```sh
sudo iptables -P INPUT ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -F
```

```sh
#keep established connections
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

#ssh port for all
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

#### App - If you use nginx and ssl

```sh
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

##### DB - only if mysql server

```sh
sudo iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
```

### END

##### loopback

```sh
sudo iptables -I INPUT 1 -i lo -j ACCEPT
```

**Note**: should always be last

```sh
sudo iptables -A INPUT -j DROP
```

### Finally Save Rules

```sh
sudo service iptables save
sudo systemctl restart iptables
```

 
 
 
 
