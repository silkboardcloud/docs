# Installing Metabase in Cent OS 7 

Visit https://metabase.com/start/jar.html

right click on Download, Click on Copy Link Address

```sh
cd /usr/bin
sudo wget http://downloads.metabase.com/v0.32.10/metabase.jar
sudo chmod +x metabase.jar

sudo nano /etc/systemd/system/client_metabase.service
```

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
