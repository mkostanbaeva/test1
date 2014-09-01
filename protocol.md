#Протокол Elliptics

> Транзакция это пакет, который посылается серверу и обладает уникальным номером.

Запрос каждой транзакции это один пакет. Но сервер в ответ может вернуть уже не один пакет, а несколько и только один пакет может быть финальным. Только когда сервер получает финальный пакет можно говорить о том, что получили полный ответ на транзакцию.

Пакет данных в Elliptics представляет собой бинарную структуру данных. 

Вначале которой располагается структура `cmd`. В ней находится:
 * номер транзакции;
 * id куда был совершен ответ;
 * номер команды; 
 * специфические флаги для этой команды; 
 * флаг dnet_flagsmore (если он выставлен, то означает что после этого пакета будет что-то еще, если нет - то этот пакет последний);
 * поле size, которое показывает о наличии данных.
Поле `data` является командноспецифичным. У разных команд может быть разные значения поля `data` и в команды такие как read/write входит значения io_attr, ключ, io_flags, указание писать в кэш или нет, какая-то чек-сумма, и только после всего этого содержанся данные самого файла. 

Протокол в Elliptics осуществлен с поддержкой мультиплексирования. То есть порядок пакетов может произвольно перемешиваться если это пакеты из разных транзакций.  Но при этом, пакеты, которые приходят в одной транзакции, имеют четкую последовательность, в котором виде должен их увидеть клиент. В одном соединение может идти много транзакций. Ответы на эти транзакции могут приходить в произвольном порядке. 
Существует одно физическое соединение, по которому может осуществляться несколько транзакций, а сама транзакция является идентификатором.

#Elliptics protocol

>A transaction is a package that is sent to the server and has a unique number.

Request each transaction is one package. However, the server may return in response not one packet, but several, and only one packet can be final. Only when the server receives the final package can say that we got a complete answer to the transaction.

A data packet  in Elliptics has a binary data structure.

At first of which is the `cmd` structure. It is:
 * transaction number;
 * id where was made answer;
 * number of command;
 * specific flags for this command;
 * flag dnet_flagsmore (if it is set, it means that after this packet will be something else, if it isn't set, this packet will be the last one);
 * field `size`, which indicates the availability of data.
A field `data` is command specific. Different commands can have different values ​​of the field `data` and command such as read/write include io_attr, key, io_flags, specify write to the cache, some kind of a check-sum, and only after all of this contains the data of the file.

Elliptics protocol done with the support of multiplexing. 
