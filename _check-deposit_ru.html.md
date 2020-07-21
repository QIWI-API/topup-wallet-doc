# Проверка учетной записи клиента {#check-deposit}

###### Последнее обновление: 2020-07-14 | [Редактировать на GitHub](https://github.com/QIWI-API/topup-wallet-doc/blob/master/_check-deposit_ru.html.md)

Данным запросом вы можете проверить, зарегистрирована ли учетная запись Клиента в системе QIWI Wallet и	возможно ли пополнение данной учетной записи. 

Если вам необходима только проверка регистрации учетной записи, то можно использовать [этот запрос](#check-user).

## Формат запроса

### Параметры запроса

~~~xml
<?xml version="1.0" encoding="utf-8"?>
<request>
  <request-type>check-deposit-possible</request-type>
  <terminal-id>123</terminal-id>
  <extra name="password">XXXXX</extra>
  <extra name="phone">79031234567</extra>
  <extra name="income_wire_transfer">1</extra>
</request>
~~~

Тег|Описание
--------|------
*request*|Группирующий тег
*request-type* | Тип запроса (идентификатор запроса проверки учетной записи Клиента в системе: `check-deposit-possible`)
*terminal-id* | Идентификатор агента в системе QIWI Wallet
*extra name="password"* | Экстра-поле, содержащее пароль для аутентификации агента в системе QIWI Wallet
*extra name="phone"* | Экстра-поле, содержащее номер телефона Клиента, регистрацию учетной записи которого необходимо проверить
*extra name="ccy"* | Экстра-поле, содержащее код валюты учетной записи Клиента. Опциональный параметр. В случае его передачи проверяется наличие у Клиента учетной записи в данной валюте. В качестве значения используется цифровой или буквенный код валюты по ISO 4217.
*extra name="income_wire_transfer"* | Экстра-поле, содержащее целочисленный признак безналичных (1) или наличных (0) средств, полученных от Клиента для пополнения его учетной записи в системе QIWI Wallet.

## Формат ответа

### Ответ без ошибок обработки запроса


~~~xml
<response>
  <result-code fatal="false">0</result-code>
  <exist>1</exist>
  <deposit-possible>1</deposit-possible>
</response>
~~~

Параметры ответа:

 Тег|Описание|Атрибуты
--------|------|---------
*result-code* | [Код](#tech_error) ошибки обработки запроса.| `fatal` – логический признак фатальности ошибки обработки запроса.
*exist* | Целочисленный флаг, указывающий на существование учетной записи Клиента в системе QIWI Wallet. Флаг передается в ответе только в случае удачной обработки запроса (с кодом ошибки 0). Флаг может принимать значения:<br>`0` – учетная запись Клиента не зарегистрирована в системе QIWI Wallet (в случае если в исходном запросе указана валюта (тег `<extra name="ccy">`), это означает, что у Клиента нет учетной записи в данной валюте);<br>`1` – учетная запись Клиента зарегистрирована в системе QIWI Wallet (в случае если в исходном запросе указана валюта (тег `<extra name="ccy">`), это означает, что Клиент имеет учетную запись в данной валюте).|Отсутствуют.
*deposit-possible* | Целочисленный флаг, указывающий на возможность пополнения учетной записи Клиента в системе QIWI Wallet. Флаг передается в ответе только в случае удачной обработки запроса (с кодом ошибки 0). Флаг может принимать значения:<br>`0` – учетную запись Клиента нельзя пополнить указанным в запросе типом средств;<br>`1` – учетную запись Клиента можно пополнить указанным в запросе типом средств. | Отсутствуют.


### Ответ с ошибками обработки запроса

Если сервер не смог обработать запрос, он возвращает xml-ответ с описанием произошедшей ошибки.

~~~xml
<response>
  <result-code fatal="false">300</result-code>
</response>
~~~

Параметры ответа:

 Тег|Описание|Атрибуты
--------|------|---------
*result-code* | [Код](#tech_error) ошибки обработки запроса.| `fatal` – логический признак фатальности ошибки обработки запроса.