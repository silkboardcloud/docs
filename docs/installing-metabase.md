# Installing Metabase in Cent OS 7 

Visit https://metabase.com/start/jar.html

right click on Download, Click on Copy Link Address

```sh
sudo yum install java-1.8.0-openjdk-devel -y
cd /usr/bin
sudo wget http://downloads.metabase.com/v0.32.10/metabase.jar
sudo chmod +x metabase.jar

sudo nano /etc/systemd/system/client_metabase.service
```

### Create MySQL db 

```sh
[Unit]
Description=Client Metabase
After=syslog.target

[Service]
WorkingDirectory=/home/pulse/clients/client/metabase
ExecStart=/usr/bin/java -jar /usr/bin/metabase.jar
ExecReload=/usr/bin/kill -HUP $MAINPID
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=clientmetabase
User=cent
Group=cent
Environment="MB_DB_TYPE=mysql"
Environment="MB_DB_DBNAME=client_metabase"
Environment="MB_DB_PORT=3306"
Environment="MB_DB_USER=mysqluser"
Environment="MB_DB_PASS=mysqlpassword"
Environment="MB_DB_HOST=dbserver1.silkboard.in"


[Install]
WantedBy=multi-user.target
```
CTRL+x to exit 
Press Y and Enter to save

```sh
sudo systemctl enable client_metabase.service
sudo systemctl start client_metabase.service
# Verify - sudo systemctl start client_metabase.service
```

```sh
sudo nano /etc/nginx/conf.d/metabase.clientdomain.com.conf
```

```sh
server {
 listen  80;
 server_name    metabase.clientdomain.com;

 client_max_body_size 20M;

  location / {
    proxy_redirect off;
    proxy_set_header   X-Real-IP         $remote_addr;
    proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
    proxy_set_header   X-Forwarded-Proto $scheme;
    proxy_set_header   Host              $http_host;
    proxy_pass http://127.0.0.1:3000;
  }
}
```

Visit https://metabase.clientdomain.com to continue setup
