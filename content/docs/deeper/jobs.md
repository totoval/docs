# Jobs

## Queue Configuration
### nsq

example **docker-compose.yaml**:
```yaml
version: '3'
services:
  nsqlookupd:
    image: nsqio/nsq:v1.1.0
    container_name: nsqlookupd
    command: /nsqlookupd
    expose:
      - "4160"
      - "4161"
    ports:
      - "4161:4161"
  nsqd:
    image: nsqio/nsq:v1.1.0
    container_name: nsqd
    command: /nsqd --lookupd-tcp-address=nsqlookupd:4160 --data-path=/data
    volumes:
      - "/YOUR-DATA-PATH:/data"
    depends_on:
      - nsqlookupd
    expose:
      - "4150"
      - "4151"
    ports:
      - "4150:4150"
  nsqadmin:
    image: nsqio/nsq:v1.1.0
    container_name: nsqadmin
    command: /nsqadmin --lookupd-http-address=nsqlookupd:4161
    depends_on:
      - nsqlookupd  
    expose:
      - "4171"
    ports:
      - "4171:4171"
```
> **Attention**: Need to change the nsqd volume `YOUR-DATA-PATH` field to your own.
