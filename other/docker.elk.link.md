```
docker run --name elas3 --restart always -d  \
    -p 9210:9200 -p 9310:9300 -h elas3  \
    -e cluster.name=lookout-es -e ES_JAVA_OPTS="-Xms512m -Xmx512m" -e xpack.security.enabled=false  \
    elasticsearch
```

```
docker run --name elas4 --restart always -d  \
    -p 9211:9200 -p 9311:9300 --link elas3  \
    -e cluster.name=lookout-es -e ES_JAVA_OPTS="-Xms512m -Xmx512m" -e xpack.security.enabled=false  \
    -e discovery.zen.ping.unicast.hosts=elas3  \
    elasticsearch
```

```
docker run --name kibana2 --restart always -d  \
    -p 5602:5601 --link elas3  \
    -e ELASTICSEARCH_URL=http://elas3:9200  \
    kibana
```