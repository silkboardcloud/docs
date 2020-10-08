# INstalling PHP with fpm

```sh
sudo yum install php7.2 php7.2-fpm php7.2-mysql php-common php7.2-cli php7.2-common php7.2-json php7.2-opcache php7.2-readline php7.2-mbstring php7.2-xml php7.2-gd php7.2-curl -y


sudo systemctl enable php7.2-fpm
sudo systemctl start php7.2-fpm
sudo systemctl status php7.2-fpm
```
