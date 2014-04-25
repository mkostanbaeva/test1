# API

##hosts
###Production
The balancer open out for reading operation (the get handler only) - `storage.mds.yandex.net:80`.<br/>
The internal balancer for reading operation (the get handler only) - `storage-int.mds.yandex.net:80`.<br/>
The internal balancer for writing operation (the upload, delete, downloadinfo handlers) - `storage-int.mds.yandex.net:12000`.

###Testing
The host for reading operation (the get handler only) - `storage.mdst.yandex.net:80`.<br/>
The host for writing operation (the upload, delete, downloadinfo handlers) - `storage-int.mdst.yandex.net:12000`.

##upload 
###Description
The handler is used to upload a data. Used as follows - *hostname:port/upload-$namespace/$filename* and a data will be transmist by post. Where are: 
* *$namespace* - the name of your namespase,
* *$filename* -  name of the key for your data.
When you specify the namespace will be issued Authorization header, which should not forget to indicate -
[more here.][]
[more here.]: http://en.wikipedia.org/wiki/Basic_access_authentication 
The proxy has no restrictions on the size of upload files (it uploads by pieces, as they come to the socket).

Additional arguments (transmitted by GET):
* `offset` - an offset with which data should be written, you can use to overwrite the piece of file;
* `embed` or `embed_timestamp` and `timestamp` - the `embed` flag is used to store meta-information together with a data; from meta-information supported now only `timestamp`.

###HTTP response codes
200 - Ok (response will be similar to that in Example), <br/>
400 - the request cannot be fulfilled due to bad syntax, <br/>
401 - forgot to specify a title for the authorization <br/>
507 - in your namespaces is not enough space for writing this size <br/>
5xx - the server failed to fulfill an apparently valid request, we already know about it and fix it.

###Example
Request: 
```
curl -H "Authorization: Basic ZGVmYXVsdDoxMjM=" "http://storage-int.mds.yandex.net:12000/upload-default/file1" -d "data"
```
Answer:
```xml
<?xml version="1.0" encoding="utf-8"?>
<post obj="default.file1" id="81d8ba78474fa835aab93150230020bf94d23fc7e4c5390f6f65951210d1f254dad1a27643bd84f087ed39125b1f54a988e5b7e2f5f2d18b0218c00666dd35d1" groups="3" size="4" key="221/file1">
<complete addr="141.8.145.55:1032" path="/srv/storage/8/data-0.0" group="223" status="0"/>
<complete addr="141.8.145.116:1032" path="/srv/storage/8/data-0.0" group="221" status="0"/>
<complete addr="141.8.145.119:1029" path="/srv/storage/5/data-0.0" group="225" status="0"/>
<written>3</written>
</post>
```
Should pay attention to:
* at the `post` tag attribute `key="221/file1"` - the key with which you can work with written data (read and delete).  The key will be equal $filename when written in namespace with a static group. Generally, the key is made so: /$group/$ filename, where $group - any of the groups in which the data were written;
* at the `post` tag attribute `obj` - generated key for the data $namespace.$filename);
* at the `post` tag attribute `id` - sha512(obj) - elliptics saved a data under this key;
* at the `post` tag attribute `groups` - a number of groups in which have tried to write;
* at the `post` tag attribute `size`-  a data size;
* at the `complete` tag attribute `addr` - an address of the node where the data were written;
* at the `complete` tag attribute `path` - a path to the data;
* at the `complete` tag attribute `group` - a group number (can be used as part of the key to read/delete operations);
* at the `complete` tag attribute `status` - a status of the operation (0 - ok);
* the `written` tag - a number of groups that succeeded to write.

##get
###Description 
The handler is used for reading a data. Used as follows - *hostname:port/get-$namespace/$group/$filename*, where is *$group/$filename* is value from `key` attribute `post` tag in answer from write operation. For namespace with a static group - *hostname:port/get-$namespace/$filename*.

Additional arguments (transmitted by GET):
* `offset` - an offset with which data should be reading;
* `size` - a size of the data chunk to be read;
* `embed` and `embed_timestamp` - flag is used to indicate that the data was written with the meta-information (now is a legacy -  proxy may determine this fact for all new written data).

###HTTP response codes
200 - Ok, <br/>
404 - no data was found for this key, <br/>
5xx - the server failed to fulfill an apparently valid request, we already know about it and fix it.

###Example
Request: 
```
curl "http://storage.mds.yandex.net:80/get-default/221/file1"
```
Answer:
```
data
```
In a response are coming your data. 

##delete
###Description 
The handler is used for deleting a data. Used as follows - *hostname:port/delete-$namespace/$group/$filename*, where is  *$group/$filename* is value from `key` attribute `post` tag in answer from write operation. When you specify the namespace will be issued Authorization header, which should not forget to indicate -
[more here.][]
[more here.]: http://en.wikipedia.org/wiki/Basic_access_authentication 
For namespace with a static group - *hostname:port/delete-$namespace/$filename*.

###HTTP response codes
200 - Ok, <br/>
404 - no data was found for this key, <br/>
5xx - the server failed to fulfill an apparently valid request, we already know about it and fix it.

###Example
Request: 
```
curl -H "Authorization: Basic ZGVmYXVsdDoxMjM=" "http://storage-int.mds.yandex.net:12000/delete-default/221/file1"
```

##downloadinfo
###Description 
The handler is used to find out where the data are physically located. Used as follows - *hostname:port/downloadinfo-$namespace/$group/$filename*, where is  *$group/$filename* is value from `key` attribute `post` tag in answer from write operation. For namespace with a static group - *hostname:port/downloadinfo-$namespace/$filename*.

###HTTP response codes
200 - Ok, <br/>
404 - no data was found for this key, <br/>
5xx - the server failed to fulfill an apparently valid request, we already know about it and fix it.

###Example
Request: 
```
curl "http://storage-int.mds.yandex.net:12000/downloadinfo-default/221/file1"
```
Answer:
```xml
<?xml version="1.0" encoding="utf-8"?><download-info><host>elisto35f.dev.yandex.net</host><path>/8/data-0.0:3940155250:4</path><region>-1</region></download-info>
```

##ping & stat
###Description 
Lets you know about the operability proxy. Used as follows - *hostname:port/ping* or *hostname:port/stat*. 

###HTTP response codes
200 - Ok, <br/>
5xx - the server failed to fulfill an apparently valid request, we already know about it and fix it.

##stat_log & stat-log
###Description
Displays an information for all connected nodes. Used as follows - *:port/stat_log* or *hostname:port/stat-log*.

###HTTP response codes
200 - Ok, <br/>
5xx - the server failed to fulfill an apparently valid request, we already know about it and fix it.

###Example
```
curl "http://localhost:81/stat-log/221/file1"
```
Answer:
```xml
<?xml version="1.0" encoding="utf-8"?>
<data>
...
<stat addr="141.8.145.116:1025" id="c042ec64498b6800d061b7b30adf01f4cbefc6edce0c6db03e11f9cf08c18a2bec2f58e87bc543df5e35cf6adf5b38bab1dc06e436ea262d3369a7c4b4fae03c"><la>0.78 0.61 0.49</la><memtotal>1796394</memtotal><memfree>219f98</memfree><memcached>6686cc</memcached><storage_size>c4dc</storage_size><available_size>905c</available_size><files>320000</files><fsid>ffffffffd833998f</fsid></stat>
...</data>
```

##cache
###Description 
Shows the information received from the proxy mastermind and cashing in themselves. Used as follows - *hostname:port/cache?$types*, where is *$types* - a types of caches that want to see (listed by &).
The types of caches are:
* *group-weight* - a list of good drops, indicating a free space (with a lag of up to a minute), a namespaces and a weights;
* *symmetric-groups* - accordance in which one group can get a full drop;
* *bad-groups* - a list of bad drops;
* *cache-groups* - a groups used for cache mastermind storage;
* *namespaces-settings* - a configuration namespaces, which is working proxy now.

###HTTP response codes
200 - Ok, <br/>
5xx - the server failed to fulfill an apparently valid request, we already know about it and fix it.

###Example
Request: 
```
curl "http://localhost:81/cache?group-weights&symmetric-groups&bad-groups&cache-groups&namespaces-settings"
```
Answer:
```
{
"group-weights" : {
	"default" : {
		"2" : {
			"[201, 220]" : {
				"weight" : 0,
				"memory" : 37086994432
			},
			"[223, 225]" : {
				"weight" : 200000000,
				"memory" : 66702532608
			}
		},
		"3" : {
			"[143, 173, 228]" : {
				"weight" : 0,
				"memory" : 66702528512
			},
			"[102, 226, 227]" : {
				"weight" : 200000000,
				"memory" : 103789527040
			}
		}
	},
	"nostaticns" : {
		"2" : {
			"[222, 224]" : {
				"weight" : 200000000,
				"memory" : 66702532608
			}
		}
	}
},
"symmetric-groups" : {
	"2" : [2, 63, 103],
	"63" : [2, 63, 103],
	"102" : [102, 226, 227],
	"103" : [2, 63, 103],
	"143" : [143, 173, 228],
	"173" : [143, 173, 228],
	"201" : [201, 220],
	"220" : [201, 220],
	"222" : [222, 224],
	"223" : [223, 225],
	"224" : [222, 224],
	"225" : [223, 225],
	"226" : [102, 226, 227],
	"227" : [102, 226, 227],
	"228" : [143, 173, 228]
},
"bad-groups" : {
	[2, 63, 103]
},
"cache-groups" : {
},
"namespaces-settings" : {
	"nostaticns" : {
		"groups-count" : 2,
		"success-copies-num" : "any",
		"auth-key" : "",
		"static-couple" : []
	},
	"testns2" : {
		"groups-count" : 3,
		"success-copies-num" : "all",
		"auth-key" : "123",
		"static-couple" : [227, 102, 226]
	},
	"testns" : {
		"groups-count" : 3,
		"success-copies-num" : "all",
		"auth-key" : "",
		"static-couple" : [143, 173, 228]
	}
}
}
```

##cache-update
###Description 
Makes proxy to immediately update information from mastermind, without waiting for the timeout. If you pass an argument with-namespaces, then the proxy also will be update information about the namespaces (just as it does at the start).

###HTTP response codes
200 - Ok, <br/>
5xx - the server failed to fulfill an apparently valid request, we already know about it and fix it.

###Example
Request: 
```
curl "http://localhost:81/cache-update?with-namespaces"
```
