### Generate password hash
```
echo -n 'Ujcnm123!@#' | sha256sum
```

#### graylog conf
vim /etc/graylog/server/server.conf 

```
password_secret = Ujcnm123!@#Ujcnm123!@#
root_password_sha2 = a7b70cef6a16f9ecc6bf0c343394bd00918a0aec6595ea6e4903d3c0e98d0e7b
http_bind_address = 127.0.0.1:9000
```

#### elasticsearch conf

vim /etc/elasticsearch/elasticsearch.yml

```
cluster.name: graylog
```

#### rsyslogd.conf

vim /etc/rsyslogd.conf 
```
*.* @127.0.0.1:5140
```
