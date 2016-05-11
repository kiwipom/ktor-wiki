Simple test done via [siege](/JoeDog/siege)

Please don't use it as a reference as it is just a measurement rather than a complete benchmark.

Test plan: 500 users, 3 minutes, no delay, all default configurations, gzip/deflate enabled

### nginx

```
siege -b -t 3M -c 500 http://localhost/my/main.css
** SIEGE 3.1.0
** Preparing 500 concurrent users for battle.
The server is now under siege...
Lifting the server siege..      done.


Transactions:                5328211 hits
Availability:                 100.00 %
Elapsed time:                 179.95 secs
Data transferred:           13729.88 MB
Response time:                  0.01 secs
Transaction rate:           29609.40 trans/sec
Throughput:                    76.30 MB/sec
Concurrency:                  440.84
Successful transactions:     5328211
Failed transactions:             108
Longest transaction:           82.36
Shortest transaction:           0.00
```

---------------------------------------------------------------

### Netty/ktor

```
siege -b -t 3M -c 500 http://localhost:8080/styles/main.css
** SIEGE 3.1.0
** Preparing 500 concurrent users for battle.
The server is now under siege...
Lifting the server siege..      done.

Transactions:                3744953 hits
Availability:                 100.00 %
Elapsed time:                 179.73 secs
Data transferred:            9650.10 MB
Response time:                  0.02 secs
Transaction rate:           20836.55 trans/sec
Throughput:                    53.69 MB/sec
Concurrency:                  498.49
Successful transactions:     3744953
Failed transactions:               0
Longest transaction:            7.07
Shortest transaction:           0.00
```

---------------------------------------------------------------

### Jetty/ktor

```
siege -b -t 3M -c 500 http://localhost:8080/styles/main.css
** SIEGE 3.1.0
** Preparing 500 concurrent users for battle.
The server is now under siege...
Lifting the server siege..      done.

Transactions:                4743523 hits
Availability:                 100.00 %
Elapsed time:                 179.65 secs
Data transferred:           12223.24 MB
Response time:                  0.02 secs
Transaction rate:           26404.25 trans/sec
Throughput:                    68.04 MB/sec
Concurrency:                  497.75
Successful transactions:     4743523
Failed transactions:               0
Longest transaction:            7.22
Shortest transaction:           0.00
```

----------------------------------------------------------------

### Lighttpd

```
siege -b -t 3M -c 500 'http://localhost/~cy/main.css'
** SIEGE 3.1.0
** Preparing 500 concurrent users for battle.
The server is now under siege...
Lifting the server siege..      done.

Transactions:                4846819 hits
Availability:                 100.00 %
Elapsed time:                 179.20 secs
Data transferred:           12489.42 MB
Response time:                  0.02 secs
Transaction rate:           27046.98 trans/sec
Throughput:                    69.70 MB/sec
Concurrency:                  470.47
Successful transactions:     4846819
Failed transactions:              57
Longest transaction:           64.71
Shortest transaction:           0.00
```
