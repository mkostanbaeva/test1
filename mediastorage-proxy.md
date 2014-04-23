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
                        "path" : "/tmp/log/mediastorage/elliptics.log", 
                        "level" : 2
                },
                "proxy-log" : {   
                        "path" : "/tmp/log/mediastorage/proxy.log", 
                        "level" : 2 
                },
                "mastermind-log" : { 
                        "path" : "/tmp/log/mediastorage/mastermind.log", 
                        "level" : 2 
                },
                "timeouts" : {  
                        "wait" : 30, 
                        "check" : 60 проверка роут листов
                },
                "cfg-flags" : 4, 
                "remotes" : [ 
                        "elisto24f.dev.yandex.net:1025:2"   
                ],
                "elliptics-threads" : {  
                        "io-thread-num" : 16, 
                        "net-thread-num" : 4  
                },
                "mastermind" : { 
                        "nodes" : [    
                                {
                                        "host" : "indigo.dev.yandex.net",   
                                        "port" : 10053  
                                }
                        ],
                        "group-info-update-period" : 10  
                },
                "die-limit" : 1, 
                "eblob-style-path" : true,
                "direction-bit-num" : 16,
                "base-port" : 1024,
                "chunk-size" : { 
                        "write" : 10, 
                        "read" : 10
                }
        }
```
| Parameter | Description |
|---------------|-------------|
| elliptics-log | The Elliptics client logs. Should be set the path to the log-file and the log level (value can be from 0 to 5) |
| proxy-log | The proxy logs. Should be set the path to the log-file and the log level (value can be from 0 to 5) |
| mastermind-log | The libmastermind logs. Should be set the path to the log-file and the log level (value can be from 0 to 5) |
| timeouts | The timeouts sittings |
| cfg-flags | Configuration flags of the Elliptics client |
| remotes | The nodes of Elliptics storage. A string address in the format - “host:port:family" |
| [elliptics-threads](#elliptics-threads) | Configuration of the Elliptics client threads  |
| [mastermind](#mastermind) | Configuration for the libmastermaind |
| die-limit | Sets how many live connections between mediastorage-proxy and Elliptics that to assume that the system is operable. But it is impossible make a record if the the system contains fewer connections because it works in read-only mode |
| eblob-style-path | If the value is 1 that's eblob, else - if the value 0 |
| direction-bit-num |  |
| base-port | The value for Dnet base port. Style specifying to a file path: if the value "1" - eblob, else - filesystem. |
| chunk-size | A size of a single piece of data to be written or to be read. The size is specified in MB. It is a required parameter |

###timeouts
Allow you to override at runtime the previous values for timeouts.
* *wait* - a time to wait for the operation complete,
* *check* - sets the wait for a response from the host. If it stops responding then rebuild the routing table.

###elliptics-threads
The following parameters are using to configure the client:
* *io-thread-num* -  a number of IO threads in processing pool,
* *net-thread-num* - a number of threads in network processing pool.

###mastermind
Allows to communicate the mediastorage-proxy with the mastermind in Cocaine. Mastermind calculates the load on the nodes.  It lets say what the nodes are most loaded and where should be the load on nodes for write operations. To configure the client are using the following parameters:
* *nodes* - paths to all the Cocaine locators that can go to mastermind (values for a path to the cocaine-runtime and for a port where is the locator),
* *group-info-update-period* - a time after which should be updated the information (this parameter in seconds).
