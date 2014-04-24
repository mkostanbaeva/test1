# API

##hosts
###Production
###Testing

##upload 
###Description
The handlers are used to upload a data. Used as follows - *hostname:port/upload-$namespace/$filename* and a data will be transmist by post. Where are: 
* *$namespace* - the name of your namespase,
* *$filename* -  name of the key for your data.
When you specify the namespace will be issued Authorization header, which should not forget to indicate -
[more here.][]
[more here.]: http://en.wikipedia.org/wiki/Basic_access_authentication 
The proxy has no restrictions on the size of upload files (it uploads by pieces, as they come to the socket).

Additional arguments (transmitted by GET):
* `offset` - an offset with which data should be written, you can use to overwrite the piece of file;
* `embed` or `embed_timestamp` and `timestamp` - the `embed` flag is used to store meta-information together with a data; from meta-information supported now only `timestamp`.

###Http response codes
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
На что обязательно надо обратить внимание в ответе:
атрибут key у тега post
key="221/file1" -- это тот ключ, по которому потом можно манипулировать с записанными данными (считывать и удалять). При записи в неймспейс со статическими группами ключ будет равен $filename.
Вообще ключ составляется так: /$group/$filename , что такое $filename см. выше, $group -- любая из групп, в которые были залиты данные.
Остальные атрибуты тега
post: 
obj-- сгенерированный ключ для ваших данных ($namespace.$filename)
id -- sha512(obj) - под таким ключом elliptics сохранил данные где-то в себе
groups-- во столько групп пытались записать
size -- размер данных
Атрибуты тега complete
addr-- адрес машинки, куда были записанны данные
path-- они где-то в этом блобе
group-- номер группы этой ноды (его тоже можно использовать как часть ключа для чтения/удаления)
status-- а удалось ли вообще записать в эту ноду (0 -- все хорошо)
Тег writtenпоказывает во сколько групп записать удалось (по сути: количество тегов complete, у которых status="0").

##get
###Description 
###Http response codes
###Example

##delete
###Description 
###Http response codes
###Example

##downloadinfo
###Description 
###Http response codes
###Example

##ping & stat
###Description 
###Http response codes
###Example

##stat_log & stat-log
###Description 
###Http response codes
###Example

##cache
###Description 
###Http response codes
###Example

##cache-update
###Description 
###Http response codes
###Example
