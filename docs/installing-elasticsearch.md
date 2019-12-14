# Installing Elastic Search

```
sudo yum install java-1.8.0-openjdk-devel -y

rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch

sudo nano /etc/yum.repos.d/elasticsearch.repo

[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md


# https://www.elastic.co/guide/en/elasticsearch/reference/current/rpm.html
sudo yum install --enablerepo=elasticsearch elasticsearch

sudo /bin/systemctl enable elasticsearch.service

sudo systemctl start elasticsearch.service

sudo systemctl status elasticsearch.service

curl localhost:9200
```
