### repo

```
[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/oss-6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md

[mongodb-org-4.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc
```

### Install graylog and pkgs
```
rpm -Uvh https://packages.graylog2.org/repo/packages/graylog-3.0-repository_latest.rpm

yum install nginx java elasticsearch-oss httpd-tools.x86_64 graylog-server mongodb-org http://packages.treasuredata.com.s3.amazonaws.com/3/redhat/7/x86_64/td-agent-3.7.0-0.el7.x86_64.rpm 

/opt/td-agent/embedded/bin/gem install gelf
```
