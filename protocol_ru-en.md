#Протокол Elliptics

Elliptics представляет собой сеть из серверных нод, на который хранится информация, и нод, которые запускаются в рамках клиентских приложений, их используют для удаленного доступа к этим данным и называют клиентскими нодами.

Пара запрос-ответ называется транзакцией. Запрос осуществляется одним пакетом. Ответ может состоять из нескольких пакетов. Только когда клиентская нода получает финальный пакет можно говорить о том, что получен полный ответ, и он является завершающем пакетом для транзакции.  Транзакцию легко отследить по специальному идентификатору, находящемуся в каждом пакете. 

Протокол поддерживает мультиплексирование, то есть по одному физическому соединению может производится несколько транзакций одновременно. Порядок пакетов из одной транзакции имеет четкую последовательность, но пакеты из разных транзакций могут произвольно перемешиваться.

Все пакеты имеют единую структуру. Поля структуры:

| Поле | Размер поля | Описание |
|-----------|-------------|-------------|
| dnet_id | 64 | Идентификатор ключа. |
| status | 4 | Код результата (errno). |
| cmd | 4 | Тип команды. |
| backend_id | 4 | Бэкенд, обрабатывающий операцию. |
| trace_id | 8 | Уникальный идентификатор для поиска по логам |
| flags | 8 | Флаги [DNET_CMD_FLAGS.] (https://github.com/reverbrain/elliptics/blob/master/include/elliptics/packet.h#L129)|
| trans | 8 | Номер транзакции, уникальный для клиента. |
| size | 8 | Размер данных. |
| data | size | Данные. |



#Elliptics protocol

Elliptics represents a network of the server nodes in which information is stored, and the nodes that run within the client applications, they are used for remote access to the data and called the client nodes.

The request-response pair is called a transaction. The request is made in one package. The answer may consist of several packets. Only when the client node receives the final package can talk about the fact that it received a complete answer, and it is the final package for the transaction. Easily keep track of the transaction by a special identifier contained in each package.

The protocol supports multiplexing, that is, on one physical connection can be produced multiple transactions simultaneously. The order of the packets has a clear sequence of an one transaction, but packets from different transactions can be optionally to stir.

All packages have a single structure. Fields in the structure:

| Field | Field size | Description |
|-----------|-------------|-------------|
| dnet_id | 64 | The key identifier. |
| status | 4 | The result code (errno). |
| cmd | 4 | Type of the command. |
| backend_id | 4 | The backend which is processing  an operation. |
| trace_id | 8 | Unique identifier for search by logs. |
| flags | 8 | The flags of [DNET_CMD_FLAGS.] (https://github.com/reverbrain/elliptics/blob/master/include/elliptics/packet.h#L129)|
| trans | 8 | A transaction number which unique to the client.|
| size | 8 | Size of the data. |
| data | size | The data. |



