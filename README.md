# ELK - Александр Шевцов
![image](https://github.com/aztecprod/ELK/assets/25949605/f4cde8d7-0fd7-41c6-b01c-824257f6198c)
![image](https://github.com/aztecprod/ELK/assets/25949605/dbe42f0e-13cb-4a9a-93ef-3f2c8f4e8b6f)
![image](https://github.com/aztecprod/ELK/assets/25949605/186a7600-fba8-4f9b-8ea9-cb487e9f9ecd)
![image](https://github.com/aztecprod/ELK/assets/25949605/7f815889-f0f5-4dc7-a061-41e9db924104)
![image](https://github.com/aztecprod/ELK/assets/25949605/0138be62-e662-46ce-a90d-68d040af2d1a)

/etc/logstash/logstash.yml

```
input {
file {
path => "/var/log/nginx/access.log"
start_position => "beginning"
}
}
filter {
grok {
match => { "message" => "%{IPORHOST:remote_ip} - %{DATA:user_name}
\[%{HTTPDATE:access_time}\] \"%{WORD:http_method} %{DATA:url}
HTTP/%{NUMBER:http_version}\" %{NUMBER:response_code} %{NUMBER:body_sent_bytes}
\"%{DATA:referrer}\" \"%{DATA:agent}\"" }
}
mutate {
remove_field => [ "host" ]
}
}
output {
elasticsearch {
hosts => "https://localhost:9200"
index    => "websrv-%{+YYYY.MM}"
data_stream => "true"
}
```
/etc/kibana/kibana.yml
```
server.host: "192.168.0.108"
server.publicBaseUrl: "http://192.168.0.108:5601/"
```
/etc/elasticsearch/elasticsearch.yml
```
path.logs: /var/log/elasticsearch
path.data: /var/lib/elasticsearch
network.host: 0.0.0.0
http.host: 127.0.0.1
discovery.seed_hosts: ["127.0.0.1", "[::1]"]

```
![image](https://github.com/aztecprod/ELK/assets/25949605/5fc89876-25ec-44b9-937a-c5798f0ec4f3)
