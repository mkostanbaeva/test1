#Протокол Elliptics

Транзакция в Elliptics - это пара запрос-ответ. Запрос в каждой транзакции это один пакет. В ответ сервер может возвращать несколько пакетов (их количество неограниченно). Только когда клиент получает финальный пакет можно говорить о том, что получили полный ответ на транзакцию. Финальный пакет является завершающем пакетом для транзакции. 

Протокол в Elliptics поддерживает мультиплексирование. То есть порядок пакетов может произвольно перемешиваться если это пакеты из разных транзакций.  Но при этом, пакеты, которые приходят в одной транзакции, имеют четкую последовательность, в котором виде должен их увидеть клиент. В одном соединение может идти много транзакций. Ответы на эти запросы могут приходить в произвольном порядке.

Пакет запроса или ответа в Elliptics представляет собой бинарную структуру данных. Поля структуры:

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

Request in each transaction is one package. However, the server may return in response not one packet, but several, and only one packet can be final. Only when the server receives the final package can say that we got a complete answer to the transaction.

A data packet  in Elliptics has a binary data structure.

At first of which is the `cmd` structure. It is:
 * transaction number;
 * id where was made answer;
 * number of command;
 * specific flags for this command;
 * flag dnet_flagsmore (if it is set, it means that after this packet will be something else, if it isn't set, this packet will be the last one);
 * field `size`, which indicates the availability of data.
A field `data` is command specific. Different commands can have different values ​​of the field `data` and command such as read/write include io_attr, key, io_flags, specify write to the cache, some kind of a check-sum, and only after all of this contains the data of the file.

Elliptics protocol done with the support of multiplexing. That is the order of packets can arbitrarily stir if packets from different transactions. But in this case, packets that come within the same transaction, have a clearly sequence in which the client should see. In one connection may go many transactions.The answers to these transactions may come in any order.

There is one physical connection wherein may be several transactions, and the transaction itself is an identifier.
