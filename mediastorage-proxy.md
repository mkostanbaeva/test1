#Configuration Mediastorage-proxy

Configuration file consists of configurations the TheVoid and mediastorage-proxy sittings in JSON format. 

##The TheVoid settings
```json
        "endpoints": [
                "unix:/tmp/thevoid/mediastorage-proxy.sock",
                "0.0.0.0:8082"
        ],
        "backlog": 128,
        "safe_mode": true,
        "threads": 2,
        "buffer_size": 65536,
        "daemon": {
                "monitor-port": 20001
        },
```
| Parameter | Description |
|-----------|-------------|
| endpoints | A sockets that will listen proxy. Can set a unix-sockets and a tcp-sockets |
| backlog | A queue to each socket |
| safe_mode | If the value is TRUE this option fixes all uncaught errors and returns 500 |
| threads | The number of streams which handles connection |
| buffer_size | The buffer size of matching packages |
| monitor-port | The port value for monitoring proxy |
##Mediastorage-proxy settings
```json
		"application" : {
                "elliptics-log" : { 
                        "path" : "/tmp/log/mediastorage/elliptics.log", A path to log-file. 
                        "level" : 2 A log level (value can be from 0 to 5).
                },
                "proxy-log" : {   
                        "path" : "/tmp/log/mediastorage/proxy.log", A path to log-file. 
                        "level" : 2 A log level (value can be from 0 to 5).
                },
                "mastermind-log" : { 
                        "path" : "/tmp/log/mediastorage/mastermind.log", A path to log-file. 
                        "level" : 2 A log level (value can be from 0 to 5).
                },
                "timeouts" : {  
                        "wait" : 30, A time to wait for the operation complete.
                        "check" : 60 проверка роут листов
                },
                "cfg-flags" : 4, 
                "remotes" : [ 
                        "elisto24f.dev.yandex.net:1025:2"   
                ],
                "elliptics-threads" : {  
                        "io-thread-num" : 16, The number of IO threads in processing pool.
                        "net-thread-num" : 4  The umber of threads in network processing pool.
                },
                "mastermind" : { 
                        "nodes" : [   Paths to all the Cocaine locators that can go to mastermind. 
                                {
                                        "host" : "indigo.dev.yandex.net",  A path to the cocaine-runtime. 
                                        "port" : 10053 A port where is the locator. 
                                }
                        ],
                        "group-info-update-period" : 10 A time after which should be updated the information (this parameter in seconds). 
                },
                "die-limit" : 1, тоже что и в эллиптикс-фастсжай
                "eblob-style-path" : true,
                "direction-bit-num" : 16,
                "base-port" : 1024,
                "chunk-size" : { обязательный параметр
                        "write" : 10, в мб
                        "read" : 10
                }
        }
```
| Parameter | Description |
|-----------|-------------|
| elliptics-log | The Elliptics client logs |
| proxy-log | The proxy logs |
| mastermind-log | The libmastermind logs |
| timeouts | The timeouts sittings |
| cfg-flags | Configuration flags of the Elliptics client |
| remotes | The nodes of Elliptics storage. A string address Cin the format - “host:port:family" |
| elliptics-threads | Configuration of the Elliptics client threads |
| mastermind | Configuration for the libmastermaind.  Allows to communicate the mediastorage-proxy with the mastermind in Cocaine. Mastermind calculates the load on the nodes.  It lets say what the nodes are most loaded and where should be the load on nodes for write operations |
| die-limit |  |
| direction-bit-num |  |
| base-port |  |
| chunk-size |  |

