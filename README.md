# docker-assignment


# create the folders
```bash
mkdir efk-nginx
cd efk-nginx
mkdir nginx filebeat

```
# create a docker compse file
``` bash
version: '3.8'

services:

  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "8081:80"
    volumes:
      - ./nginx:/var/log/nginx

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.17.9
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
      - ES_JAVA_OPTS=-Xms512m -Xmx512m
    ports:
      - "9200:9200"

  kibana:
    image: docker.elastic.co/kibana/kibana:7.17.9
    container_name: kibana
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch

  filebeat:
    image: docker.elastic.co/beats/filebeat:7.17.9
    container_name: filebeat
    user: root
    volumes:
      - ./filebeat/filebeat.yml:/usr/share/filebeat/filebeat.yml
```
# Create Filebeat Configuration
nano filebeat/filebeat.yml


``` bash
filebeat.inputs:
  - type: log
    enabled: true
    paths:
      - /var/log/nginx/*.log

output.elasticsearch:
  hosts: ["elasticsearch:9200"]
      - ./nginx:/var/log/nginx
    depends_on:
      - elasticsearch
```
# Start the Stack
docker-compose up -d

# Access in Browser
Nginx
http://localhost:8081
Kibana
http://localhost:5601
