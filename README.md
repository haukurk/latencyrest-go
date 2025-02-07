[![Build Status](https://travis-ci.org/haukurk/latency-microservice-go.svg?branch=master)](https://travis-ci.org/haukurk/latency-microservice-go)
[![Docker Stars](https://img.shields.io/docker/stars/haukurk/latency-server.svg)]()
[![Docker Pulls](https://img.shields.io/docker/pulls/haukurk/latency-server.svg)]()

Latency Analyser Microservice
=================

An extremly simple service that allows you to check latency between the server and some IP.
The project is based on fastping-go and the GIN framework to simplify ICMP and service communication.

Frameworks and libraries used:
 * Gin
 * cli.go

# Installation

Building solution:
```
make deps && make 
```

Installing to GOPATH bin folder:
```
make install
```

Run tests
```
make test
```

# Running the service

To start-up the service use:
```
cmd/server/server --config config.json
```

# Client

The solution includes a CLI client that offers you to create a request from your command-line, like such:

```
cmd/latency-cli/latency-cli --host $HOSTNAME -r $HOSTTOCHECKLATENCY client
```


# JSON specifications (Examples)

/GET /latency/<string:hostname>

200 OK
```
{
  "ip": "74.125.136.138",
  "rtt": 4.243,
  "status": "ok",
  "unit": "ms"
}

```

404 NOT FOUND
```
{
  "error":"cannot resolve remote address",
  "status":"fail"
}
```

/GET /stats

200 OK

```
{
  "pid":1953,
  "uptime":"1h42m44.212770594s",
  "uptime_sec":6164.212770594,
  "time":"2015-05-10 10:33:17.482820233 -0400 EDT",
  "unixtime":1431268397,
  "status_code_count":{},
  "total_status_code_count":{"200":4},
  "count":0,
  "total_count":4,
  "total_response_time":"4.199562871s",
  "total_response_time_sec":4.199562871,
  "average_response_time":"1.049890717s",
  "average_response_time_sec":1.049890717
}
```

# Dockerization 

Pull from Docker registry
```
docker pull haukurk/latency-server:latest 
```

Or

Create a Docker Image from solutions definition:
```
docker build -t latency-microservice-go .
```

Create a container instance named latency-server:
```
docker run -d --publish 7801:7801 --name latency-server [latency-microservice-go or image id from docker registry]
```

# Considerations

The ICMP library uses raw sockets, therefore needs root privileges to function properly.

*I discourage users using this as a public service.*

Make sure to run tests before using.
