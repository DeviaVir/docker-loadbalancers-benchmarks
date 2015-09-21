# Tests

Tests assume there is a simple static website running on 192.168.99.100:80 (backend).

- haproxy:1.5
- nginx:1.9.4

Tested using ApacheBench, for running the tests, see below.

# Results

These are results on my local machine, your mileage may vary.

*haproxy*

```
Concurrency Level:      10
Time taken for tests:   10.426 seconds
Complete requests:      10000
Failed requests:        0
Total transferred:      7070000 bytes
HTML transferred:       4750000 bytes
Requests per second:    959.18 [#/sec] (mean)
Time per request:       10.426 [ms] (mean)
Time per request:       1.043 [ms] (mean, across all concurrent requests)
Transfer rate:          662.25 [Kbytes/sec] received
```

*nginx*

```
Concurrency Level:      10
Time taken for tests:   10.559 seconds
Complete requests:      10000
Failed requests:        0
Total transferred:      7070000 bytes
HTML transferred:       4750000 bytes
Requests per second:    947.03 [#/sec] (mean)
Time per request:       10.559 [ms] (mean)
Time per request:       1.056 [ms] (mean, across all concurrent requests)
Transfer rate:          653.86 [Kbytes/sec] received
```

It looks like _haproxy_ is slightly faster. This can be even more boosted when the "check" parameter is dropped from the backend nodes (up to 1479 req/s).

# How-to

## Method

Round-robin

## Pull dockers

Fetch from docker hub.

*haproxy*

```
docker pull deviavir/haproxy-benchmark
```

*nginx*

```
docker pull deviavir/nginx-benchmark
```

## Build dockers

Alternatively edit and build the dockers yourself.

*haproxy*

```
docker build -t haproxy-benchmark ./haproxy
```

*nginx*

```
docker build -t nginx-benchmark ./nginx
```

## Run dockers

*haproxy*

```
docker run -p 8080:80 -d --name haproxy-benchmark haproxy-benchmark
```

*nginx*

```
docker run -p 8081:80 -d --name nginx-benchmark nginx-benchmark
```

## Start tests

*haproxy*

```
ab -n 10000 -c 10 http://192.168.99.100:8080/
```

*nginx*

```
ab -n 10000 -c 10 http://192.168.99.100:8081/
```

