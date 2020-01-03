Inline-style: 
![alt text](https://storage.googleapis.com/my-newest-bucket-coinsparta/docker.jpg "Sample Docker Compose File")


# Sample docker-compose.yml file
Simple Docker compose file to bootstrap your project.  



### Fetch file:
---
```
curl  https://raw.githubusercontent.com/DataDog/docker-compose-example/master/docker-compose.yml --output docker-compose.yml
```

### Sample:
---
```
version: "3"
services:
  web:
    build: web
    command: python app.py
    ports:
     - "5000:5000"
    volumes:
     - ./web:/code # modified here to take into account the new app path
    links:
     - redis
    environment:
     - DATADOG_HOST=datadog # used by the web app to initialize the Datadog library
  redis:
    image: redis
  # agent section
  datadog:
    build: datadog
    links:
     - redis # ensures that redis is a host that the container can find
     - web # ensures that the web app can send metrics
    environment:
     - DD_API_KEY=__your_datadog_api_key_here__
     - DD_DOGSTATSD_NON_LOCAL_TRAFFIC=true
    volumes:
     - /var/run/docker.sock:/var/run/docker.sock
     - /proc/:/host/proc/:ro
     - /sys/fs/cgroup:/host/sys/fs/cgroup:ro
```
