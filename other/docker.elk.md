```
Install Compose:
sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
```
docker run --name elas1 --restart always -d  \
    -p 9200:9200  \
    -p 9300:9300  \
    --network=my-bridge-network --ip=172.98.0.31 \
    -e ES_JAVA_OPTS="-Xms512m -Xmx512m"  \
    -v /home/minstrel/elk/elas1/data:/usr/share/elasticsearch/data  \
    -v /home/minstrel/elk/elas1/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml  \
    elasticsearch
```
```
docker run --name elas2 --restart always -d  \
    --network=my-bridge-network --ip=172.98.0.32  \
    -e ES_JAVA_OPTS="-Xms512m -Xmx512m"  \
    -v /home/minstrel/elk/elas2/data:/usr/share/elasticsearch/data  \
    -v /home/minstrel/elk/elas2/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml  \
    elasticsearch
```
```
docker run --name kibana --restart always -d  \
    --network=my-bridge-network --ip=172.98.0.33  \
    -p 5601:5601  \
    -v /home/minstrel/elk/kibana/kibana.yml:/usr/share/kibana/config/kibana.yml  \
    kibana
```
```
docker run --name logstash -it --restart always -d  \
    --network=my-bridge-network --ip=172.98.0.34  \
    -v /home/minstrel/elk/logstash/cfg/logstash.conf:/usr/share/logstash/pipeline/logstash.conf  \
    logstash
```
```
docker run --name filebeat --restart always -d  \
    --network=my-bridge-network --ip=172.98.0.35  \
    -v /home/minstrel/elk/filebeat/cfg/filebeat.yml:/usr/share/filebeat/filebeat.yml  \
    -v /home/minstrel/logs/:/home/logs/  \
    filebeat
```
```
curl http://172.98.0.31:9200/_cat/health?v
http://192.168.2.102:5601
```