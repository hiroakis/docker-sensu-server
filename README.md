# docker-sensu-server

CentOS and sensu.
It runs redis, rabbitmq-server, sensu-dashboard, sensu-api, sensu-server and ssh processes.

## Installation

Install from docker index or build you own

```
docker pull hiroakis/docker-sensu-server
```

or

```
git clone https://github.com/hiroakis/docker-sensu-server.git
cd docker-sensu-server
docker build -t yourname/docker-sensu-server .
```

## Run

```
docker run -d -p 10022:22 -p 8080:8080 -p 4567:4567 -p 5671:5671 -p 15672:15672 hiroakis/docker-sensu-server
```

## how to access via bowser and sensu-client

### rabbitmq console

* http://your-server:15672/
* id/pwd : sensu/password

### sensu dashboard

* http://your-server:8080/
* id/pwd : admin/secret

### sensu-client configuration example

* /etc/sensu/config.json

```
{
  "rabbitmq": {
    "host": "sensu-server-ipaddr",
    "port": 5671,
    "vhost": "/sensu",
    "user": "sensu",
    "password": "password",
    "ssl": {
      "cert_chain_file": "/etc/sensu/ssl/cert.pem",
      "private_key_file": "/etc/sensu/ssl/key.pem"
    }
  }
}
```

* /etc/sensu/conf.d/client.json

```
{
  "client": {
    "name": "sensu-client-node-hostname",
    "address": "sensu-client-node-ipaddr",
    "subscriptions": [
      "common",
      "web"
    ]
  },
  "keepalive": {
    "thresholds": {
      "critical": 60
    },
    "refresh": 300
  }
}
```

## ssh login

```
ssh hiroakis@localhost -p 10022
password: hiroakis
```

## Lisence

MIT
