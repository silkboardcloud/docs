### Update OS with to Latest

```sh
sudo yum install epel-release 
sudo yum update
```

#### with root login

```sh
useradd cent
passwd cent
usermod cent -aG wheel 
```

#### Disable Root login

SSH server settings are stored in the `/etc/ssh/sshd_config` file.

To disable root logins, make sure you have the following entry `PermitRootLogin no`*

```sh
sudo sed -i '/#PermitRootLogin yes/c\PermitRootLogin no' /etc/ssh/sshd_config
sudo systemctl restart sshd
```

- Now exit from root prompt and login as **cent** user

### Disable SELinux
https://github.com/silkboardcloud/docs/blob/master/scripts/disable-selinux-on-cent-os-7.sh

### Setup Timezone

```sh
sudo rm /etc/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
```

#### setup hostname 

if hostname doesn't end with domain name

mail from system may be rejected

`sudo hostname $HOSTNAME`

#### Setup Git

```sh
sudo yum install git
git config --global user.name "$USER $(hostname)"
git config --global user.email "$USER@$(hostname)"
```

### Setup SSH Keys

```sh
ssh-keygen -t rsa -b 4096 -C "$(git config --global user.email)"
```
 
 - add it to github if you need to pull code without password

#### Ensure ssh-agent is enabled

```sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

### Remove firewalld

```sh
sudo systemctl mask firewalld
sudo systemctl stop firewalld
sudo yum remove firewalld
```

### Install iptables

```sh
sudo yum -y install iptables-services
sudo systemctl start iptables
sudo systemctl enable iptables
```

**iptables**

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

###server

**app**

```sh
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

**db**

```sh
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
```

### END

**loopback**

`sudo iptables -I INPUT 1 -i lo -j ACCEPT`

**should always be last**

`sudo iptables -A INPUT -j DROP`

### Finally Save Rules

`sudo service iptables save`

`sudo systemctl restart iptables`

 
 
 
 
