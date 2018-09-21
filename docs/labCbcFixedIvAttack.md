Прекрасным летним днём, Евгений Фёдорович решил узнать побольше о режимах шифрования и приобрёл книгу В.М. Фомичёва 
"Методы дискретной математики в криптологии. Обрадовавшись новой покупке, он незамедлительно открыл страницу 301 и прочитал там 

`"Однако важно избегать повторения синхропосылки в разных сообщениях, шифруемых одинаковым ключом. Это затрудняет атаку на шифртекст,
основанную на наличии стандартов в начале сообщения. В качестве синхропосылки используется некоторая строка случайных байтов,
или метка времени."`.

Проникнувшись простой реализации режима CBC он незамедлительно зашифровал свой пин-код от банковской карты этим режимом,
исмпользуя в качестве ветора инициализации текущее время. 

Почитав немного википедию, Евгений Фёдорович узнал, что режим CBC настолько прекрасен, что даже если противник имеет доступ 
к оракулу зашифрования (шифрует произольные сообщения противника), он всё равно не сможет расшифровать шифртекст.

Осознав своё могущество, Евгений Фёдорович написал оракул, позволяющих шифровать произольные сообщения **на том же ключе**, на котором
он зашифровал пин-код, и захостил его на своём сайте вместе с зашифрованным пин-кодом 
(что может пойти не так, ведь CBC семантически стойкий, а в книге В.М. Фомичёва что попало не напишут).

## Задание

Дана REST служба с API указанным ниже.

Задача - детерминированно дешифровать пин-код из шифртекста.

Пин-код длины не более 5, состоит из цифр. Строковое представление пин-кода кодируется с помощью ASCII, дополняется до 
длины 16 байт нулями и шифруется.

**ВАЖНО!** Все передаваемые сообщения должны быть предварительно закодированы с помощью BASE64. 

Полученные сообщения так же должны быть декодированы из BASE64 в указанный тип.

**ВАЖНО!** Все строки кодируется с использование кодировки ASCII.

т.е. передача строки для лабы выглядит следующим образом:
строка -> (asсii) -> массив байт -> (base64) -> строка -> json 

## Ход работы.

### Тестирование 

`<userId>` = имя аккаунта на GitHub  (или фамилия студента)

`<challengeId>` = 1


1. Проверить работоспособность контроллера с помощью метода `GET <host>/api/IvIsTime`
2. Получить зашифрованный пин-код с помощью метода `GET <host>/api/EncryptionModeOracle/<userId>/<challengeId>/encryptedpin`
3. Получить пин-код, используя метод `POST <host>/api/EncryptionModeOracle/<userId>/<challengeId>/noentropy`
4. Проверить пин-код, используя метод `GET <host>/api/IvIsTime/<userId>/<challengeId>/validate`

### Сдача лабы
шаги 1 - 3 этапа тестирования аналогично для 20 различных `<challengeId>`.

## Описание API

Rest запросы, в заголовке выстален Content-Type: application/json; charset=utf-8.

### Описание методов

## `GET <host>/api/IvIsTime`

Проверка работоспособности контроллера. Возращает `operating`. Ответ не кодируется в BASE64.

| Парметр| Описание| 
| --- | --- 
| `<host>` | имя хоста веб службы


## `POST <host>/api/IvIsTime/<userId>/<challengeId>/noentropy`

Зашифровывает данные в режиме CBC на фиксированном для задания для задания ключе.

| Парметр| Описание| 
| --- | --- 
| `<host>` | имя хоста веб службы
| `<userId>` | идентификатор студента
| `<challengeId>` | идентификатор задания

## `GET <host>/api/IvIsTime/<userId>/<challengeId>/encryptedpin`

Возвращает зашифрованный пин код, на фиксированном для задания ключе

| Парметр| Описание| 
| --- | --- 
| `<host>` | имя хоста веб службы
| `<userId>` | идентификатор студента
| `<challengeId>` | идентификатор задания

## `GET <host>/api/IvIsTime/time`

текущее время в формате unix time (секунды), используется в качестве IV.

| Парметр| Описание| 
| --- | --- 
| `<host>` | имя хоста веб службы
| `<userId>` | идентификатор студента
| `<challengeId>` | идентификатор задания

## `GET <host>/api/IvIsTime/<userId>/<challengeId>/validate`

Возвращает пин-код, используемый в задании.

| Парметр| Описание| 
| --- | --- 
| `<host>` | имя хоста веб службы
| `<userId>` | идентификатор студента
| `<challengeId>` | идентификатор задания