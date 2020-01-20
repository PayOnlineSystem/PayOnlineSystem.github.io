# **Термины и сокращения**


<table>
  <tr>
   <td><strong>Транзакция</strong>
   </td>
   <td>—
   </td>
   <td>банковская операция по переводу денег с одного счета на другой.
   </td>
  </tr>
  <tr>
   <td><strong>Банк-эмитент</strong>
   </td>
   <td>—
   </td>
   <td>банк, выдавший карту.
   </td>
  </tr>
  <tr>
   <td><strong>Авторизация</strong>
   </td>
   <td>—
   </td>
   <td>разрешение банка-эмитента на проведение платежа по банковской карте.
   </td>
  </tr>
  <tr>
   <td><strong>Система, PayOnline </strong>
   </td>
   <td>—
   </td>
   <td>система приема электронных платежей PayOnline.
   </td>
  </tr>
  <tr>
   <td><strong>ТСП, Мерчант </strong>
   </td>
   <td>—
   </td>
   <td>торгово-сервисное предприятие, подключенное к системе.
   </td>
  </tr>
</table>

# **Типы транзакций**

<table>
  <tr>
   <td><strong>Purchase</strong>
   </td>
   <td>—
   </td>
   <td>покупка, перевод денег со счета покупателя (держателя карты) на счет ТСП.
   </td>
  </tr>
    <tr>
   <td><strong>Refund</strong>
   </td>
   <td>—
   </td>
   <td>возврат, перевод денег со счета ТСП на счет покупателя (держателя карты).
   </td>
  </tr>
    <tr>
   <td><strong>Refill</strong>
   </td>
   <td>—
   </td>
   <td>пополнение, перевод денег со счета ТСП на счет держателя карты.
   </td>
  </tr>
    <tr>
   <td><strong>Arbitrary Purchase</strong>
   </td>
   <td>—
   </td>
   <td>платёж по «свободным» реквизитам.
   </td>
  </tr>
    <tr>
   <td><strong>Card to Card Transfer</strong>
   </td>
   <td>—
   </td>
   <td>перевод средств со счёта карты на счёт другой карты.
   </td>
  </tr>
</table>

# **Статусы транзакций**

**Pending** — авторизация транзакции подтверждена, деньги заблокированы на счету держателя в ожидании списания.

**Settled** — списание суммы транзакции со счета держателя карты подтверждено.

**Voided** — транзакция отменена (аннулирована), сумма транзакции разблокирована на счете держателя карты.

**Declined** — транзакция отклонена (авторизация не подтверждена).

**PreAuthorized** — предварительная авторизация. Разрешение на списание получено, деньги заблокированы на счету, но списание произойдет только после подтверждения транзакции ТСП. Если подтверждение не получено в установленный срок (оговаривается индивидуально), транзакция будет автоматически отменена.


# **Типы платежей**

**Прямые** — после авторизации деньги блокируются на счете держателя и автоматически списываются через сутки. Данный тип платежа включается для всех ТСП при подключении к системе.

**С предварительной авторизацией** — после авторизации деньги блокируются на счете, но списание происходит только после подтверждения транзакции ТСП. Данный тип платежа включается по требованию ТСП.


# **Переход статусов**

При запросе на авторизацию транзакции, данные проверяются в нескольких уровнях, и, в зависимости от результатов проверки, транзакция переходит в **Pending** (успешная авторизация) или **Declined** (отклоненная транзакция).

Ежесуточно осуществляется подтверждение всех авторизованных за последние сутки транзакций, при этом сумма авторизованной транзакции списывается со счета держателя карты. При этом транзакция переводится из статуса **Pending** в статус **Settled**.

Транзакцию в статусе **Pending** можно отменить в течение 24 часов с момента авторизации, выполнив операцию **Void**. При этом статус транзакции изменится на **Voided**. Отмену транзакции может производить только ТСП.

Транзакцию в статусе **Settled** отменить нельзя, т.к. она считается завершенной, однако, можно выполнить операцию по возврату денежных средств — **Refund**. При этой операции создается новая транзакция с типом **Refund**, которой могут быть присвоены статусы **Pending** или **Declined**. Сумма возврата может быть меньше или равна сумме покупки. Операции возврата денежных средств возможно осуществлять несколько раз, до тех пор, пока сумма возвращенных денег не будет равна сумме оригинальной транзакции.

# **Схемы переходов**

<img src="/img/auth.jpg">

<img src="/img/refund.jpg">

<img src="/img/preauth.jpg">


# **Методы API**

Все вызовы методов осуществляются по протоколу HTTPS.

При вычислении значения SecurityKey важным является как регистр символов, так и порядок следования параметров. \
При формировании запросов к методам важным является только регистр символов; порядок следования параметров не важен.


## **Метод Auth**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/auth/](https://secure.payonlinesystem.com/payment/transaction/auth/) \
Описание: авторизует карту и блокирует на ней указанную сумму. Передача параметров метода выполняется исключительно методом POST в кодировке UTF-8.

<p style="text-align: right">
<em>Параметры метода Auth</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. Обязательный параметр.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Идентификатор заказа в системе ТСП. Обязательный параметр.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Конечная сумма заказа. Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 15.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта заказа. \
Обязательный параметр.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>Ip
   </td>
   <td>IP-адрес плательщика. Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Строка в формате xxx.xxx.xxx.xxx, например, 66.11.130.105
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>E-Mail адрес плательщика. Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Срока, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>CardHolderName
   </td>
   <td>Имя плательщика, как отпечатано на карте. \
Обязательный параметр.
   </td>
   <td>Строка, только латинские символы, максимальная длина — 100 символов, например, Aleksey Petrov.
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Номер карты.  \
Обязательный параметр.
   </td>
   <td>13–19 цифр.
   </td>
  </tr>
  <tr>
   <td>CardExpDate
   </td>
   <td>Дата истечения срока действия карты. \
Обязательный параметр.
   </td>
   <td>Строка в формате MMYY — две цифры месяца и две цифры года, например, 0915.
   </td>
  </tr>
  <tr>
   <td>CardCvv
   </td>
   <td>Проверочный код карты — последние три цифры на обратной стороне после подписи. \
Обязательный параметр.
   </td>
   <td>3 цифры.
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Код страны плательщика. Необязательный параметр.
   </td>
   <td>2 символа в соответствии со стандартом <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Город плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Address
   </td>
   <td>Адрес плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 150 символов. 
   </td>
  </tr>
  <tr>
   <td>Zip
   </td>
   <td>Почтовый индекс плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 10 символов.
   </td>
  </tr>
  <tr>
   <td>State
   </td>
   <td>Штат плательщика — только для стран США и Канада. Необязательный параметр.
   </td>
   <td>Строка, два символа, например, FL.
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Телефон плательщика в международном формате. Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Issuer
   </td>
   <td>Наименование банка, выпустившего карту. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 100 символов.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>OrderDescription
   </td>
   <td>Описание заказа, отображаемое для плательщика на платёжных формах и в e-mail уведомлениях. \
Необязательный параметр.
   </td>
   <td>Строка в кодировке UTF-8 в закодированном виде (url encode). Максимум — 100 символов. Разрешенные символы: буквы, цифры, знаки препинания.
   </td>
  </tr>
  <tr>
   <td>Любые другие
   </td>
   <td>Любые другие параметры.
   </td>
   <td>Любой формат.
   </td>
  </tr>
</table>

Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>xml — результат в формате XML;
<p>
text — результат в текстовом формате.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)». Для формирования SecurityKey в запросе к методу Auth, может быть использован либо секретный ключ PrivateSecurityKey, либо ключ PaymentKey.

В случае использования PrivateSecurityKey, в качестве аргумента к функции вычисления хеша подставляется:

Если указан OrderDescription


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PrivateSecurityKey={PrivateSecurityKey}
```


Если OrderDescription не указан


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```


В случае использования PaymentKey, в качестве аргумента к функции вычисления хеша подставляется:

Если указан OrderDescription


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PaymentKey={PaymentKey}
```


Если OrderDescription не указан


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentKey={PaymentKey}
```

В ответ на запрос, метод отдает следующие данные:

Если была ошибка формата:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Тип данных</strong>
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код ошибки. Обязательный элемент.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Описание ошибки. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом: 


```
Code={Code}&Message={Message}
```


Если XML:


```
<error>
	<code>{Code}</code>
	<message>{Message}</message>
</error>
```


Если ошибок формата не было:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Идентификатор транзакции. Обязательный элемент.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Auth. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код результата. \
Если Code = 6001, то плательщик должен пройти 3DS-авторизацию на сайте эмитента. После 3DS-авторизации нужно вызвать метод 3DS, который описан в разделе «Метод 3DS».  \
Обязательный элемент.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Статус транзакции. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Код ошибки. Только, если платеж не выполнен.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Ссылка на повторные платежи по данной карте. Только, если платеж выполнен, и ТСП подписано на услугу повторных платежей.
   </td>
   <td>Строка, максимальная длина — 100 символов.
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Код страны, определенный по IP-адресу плательщика. Необязательный элемент.
   </td>
   <td>2 символа в соответствии со стандартом <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Код страны банка,  \
выпустившего карту. \
Необязательный элемент.
   </td>
   <td>2 символа в соответствии со стандартом <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>SpecialConditions
   </td>
   <td>Особые условия. \
Необязательный элемент.
   </td>
   <td>Строка, одно или несколько значений, перечисленных через запятую:  \
ValidationRequired — рекомендуется проверка плательщика.
   </td>
  </tr>
</table>




Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td colspan="3" ><strong> Варианты</strong>
   </td>
  </tr>
  <tr>
   <td colspan="2" >Result
   </td>
   <td>Ok — платеж выполнен;
<p>
Error — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td colspan="2" >Status
   </td>
   <td>Pending — платеж выполнен;
<p>
PreAuthorized — платеж выполнен, требуется подтверждение;
<p>
Declined — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td colspan="2" >ErrorCode
   </td>
   <td>1 — техническая проблема; плательщику стоит повторить запрос спустя некоторое время;
<p>
2 — провести платеж по банковской карте невозможно; плательщику стоит воспользоваться другим способом оплаты;
<p>
3 — платеж отклоняется банком-эмитентом карты; плательщику стоит, выяснить причину отказа в банке и повторить запрос.
   </td>
  </tr>
</table>


Если ошибок формата не было:

Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
Id={Id}&Operation=Auth&Result={Result}&Code={Code}&Status={Status}&binCountry={CountryCode}
```


Если XML:


```
<transaction>
	<id>{Id}</id>
	<operation>Auth</operation>
	<result>{Result}</result>
	<code>{Code}</code>
	<status>{Status}</status>
	<binCountry>{CountryCode}</binCountry>
</transaction>
```

## **Метод ApplePay**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/applepay/](https://secure.payonlinesystem.com/payment/transaction/applepay/) \
Описание: авторизует карту и блокирует на ней указанную сумму. Передача параметров метода выполняется исключительно методом POST в кодировке UTF-8.

<p style="text-align: right">
<em>Параметры метода ApplePay</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. Обязательный параметр.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Идентификатор заказа в системе ТСП. \
Обязательный параметр.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Конечная сумма заказа. Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта заказа. \
Обязательный параметр.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>Ip
   </td>
   <td>IP-адрес плательщика. Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Строка в формате xxx.xxx.xxx.xxx, например, 66.11.130.105
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>E-Mail адрес плательщика. Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Срока, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Код страны плательщика. Необязательный параметр.
   </td>
   <td>2 символа в соответствии со стандартом <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>PaymentToken
   </td>
   <td>Данные карты в зашифрованном виде, полученные от <a href="https://developer.apple.com/library/content/documentation/PassKit/Reference/PaymentTokenJSON/PaymentTokenJSON.html#//apple_ref/doc/uid/TP40014929-CH8-SW5">ApplePay</a>. \
Обязательный параметр.
   </td>
   <td>JSON
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Город плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Address
   </td>
   <td>Адрес плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 150 символов. 
   </td>
  </tr>
  <tr>
   <td>Zip
   </td>
   <td>Почтовый индекс плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 10 символов.
   </td>
  </tr>
  <tr>
   <td>State
   </td>
   <td>Штат плательщика — только для стран США и Канада. Необязательный параметр.
   </td>
   <td>Строка, два символа, например, FL.
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Телефон плательщика в международном формате. Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Issuer
   </td>
   <td>Наименование банка, выпустившего карту. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 100 символов.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. \
Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>OrderDescription
   </td>
   <td>Описание заказа, отображаемое для плательщика на платёжных формах и в e-mail уведомлениях. \
Необязательный параметр.
   </td>
   <td>Строка в кодировке UTF-8 в закодированном виде (url encode). Максимум — 100 символов. Разрешенные символы: буквы, цифры, знаки препинания.
   </td>
  </tr>
  <tr>
   <td>Любые другие
   </td>
   <td>Любые другие параметры.
   </td>
   <td>Любой формат.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>xml — результат в формате XML;
<p>
text — результат в текстовом формате.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)».

Для формирования SecurityKey в запросе к методу ApplePay, может быть использован либо секретный ключ PrivateSecurityKey, либо ключ PaymentKey.

В случае использования PrivateSecurityKey, в качестве аргумента к функции вычисления хеша подставляется:

Если указан OrderDescription


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PaymentToken={PaymentToken}&PrivateSecurityKey={PrivateSecurityKey}
```


Если OrderDescription не указан


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentToken={PaymentToken}&PrivateSecurityKey={PrivateSecurityKey}
```




В случае использования PaymentKey, в качестве аргумента к функции вычисления хеша подставляется:

Если указан OrderDescription


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PaymentToken={PaymentToken}&PaymentKey={PaymentKey}
```


Если OrderDescription не указан


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentToken={PaymentToken}&PaymentKey={PaymentKey}
```


В ответ на запрос, метод отдает следующие данные:

Если была ошибка формата:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Тип данных</strong>
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код ошибки. Обязательный элемент.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Описание ошибки. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
Code={Code}&Message={Message}
```


Если XML:


```
<error>
	<code>{Code}</code>
	<message>{Message}</message>
</error>
```


Если ошибок формата не было:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Идентификатор транзакции. Обязательный элемент.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Auth. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код результата. Если Code = 6001, то плательщик должен пройти 3DS-авторизацию на сайте эмитента. После 3DS-авторизации нужно вызвать метод 3DS, который описан в разделе «<a href="#/?id=Метод-3ds">Метод 3DS</a>».  \
Обязательный элемент.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Статус транзакции. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Код ошибки. Только, если платеж не выполнен.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Ссылка на повторные платежи по данной карте. Только, если платеж выполнен и ТСП подписано на услугу повторных платежей.
   </td>
   <td>Строка, максимальная длина — 100 символов.
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Код страны, определенный по IP-адресу плательщика. Необязательный элемент.
   </td>
   <td>2 символа в соответствии со стандартом <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Код страны банка, выпустившего карту. Необязательный элемент.
   </td>
   <td>2 символа в соответствии со стандартом <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>SpecialConditions
   </td>
   <td>Особые условия. \
Необязательный элемент.
   </td>
   <td>Строка, одно или несколько значений, перечисленных через запятую:  \
ValidationRequired — рекомендуется проверка плательщика.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td colspan="3" ><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td colspan="2" >Result
   </td>
   <td>Ok — платеж выполнен;
<p>
Error — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td colspan="2" >Status
   </td>
   <td>Pending — платеж выполнен;
<p>
PreAuthorized — платеж выполнен, требуется подтверждение;
<p>
Declined — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td colspan="2" >ErrorCode
   </td>
   <td>1 — техническая проблема; плательщику стоит повторить запрос спустя некоторое время;
<p>
2 — провести платеж по банковской карте невозможно; плательщику стоит воспользоваться другим способом оплаты;
<p>
3 — платеж отклоняется банком-эмитентом карты; плательщику стоит, выяснить причину отказа в банке и повторить запрос.
   </td>
  </tr>
</table>


Если ошибок формата не было:

Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
Id={Id}&Operation=Auth&Result={Result}&Code={Code}&Status={Status}&binCountry={CountryCode}
```


Если XML:


```
<transaction>
	<id>{Id}</id>
	<operation>Auth</operation>
	<result>{Result}</result>
	<code>{Code}</code>
	<status>{Status}</status>
	<binCountry>{CountryCode}</binCountry>
</transaction>
```



## **Метод Moto**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/moto/](https://secure.payonlinesystem.com/payment/transaction/moto/) \
Описание: авторизует карту и блокирует на ней указанную сумму. Передача параметров метода выполняется исключительно методом POST в кодировке UTF-8.

<p style="text-align: right">
<em>Параметры метода Moto</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. Обязательный параметр.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Идентификатор заказа в системе ТСП. Обязательный параметр.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Конечная сумма заказа. Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта заказа. \
Обязательный параметр.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>Ip
   </td>
   <td>IP-адрес плательщика. 
<p>
Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Строка в формате xxx.xxx.xxx.xxx, например, 66.11.130.105
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>E-Mail адрес плательщика. Необязательный параметр.
   </td>
   <td>Срока, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>CardHolderName
   </td>
   <td>Имя плательщика, как отпечатано на карте.  \
Обязательный параметр.
   </td>
   <td>Строка, только латинские символы, максимальная длина — 100 символов, например, Aleksey Petrov.
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Номер карты. \
Обязательный параметр.
   </td>
   <td>13–19 цифр.
   </td>
  </tr>
  <tr>
   <td>CardExpDate
   </td>
   <td>Дата истечения срока действия карты. \
Обязательный параметр.
   </td>
   <td>Строка в формате MMYY — две цифры месяца и две цифры года, например, 0915.
   </td>
  </tr>
  <tr>
   <td>CardCvv
   </td>
   <td>Проверочный код карты — последние три цифры на обратной стороне после подписи. Необязательный параметр.
   </td>
   <td>3 цифры.
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Код страны плательщика. Необязательный параметр.
   </td>
   <td>2 символа в соответствии со стандартом <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Город плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Address
   </td>
   <td>Адрес плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 150 символов. 
   </td>
  </tr>
  <tr>
   <td>Zip
   </td>
   <td>Почтовый индекс плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 10 символов.
   </td>
  </tr>
  <tr>
   <td>State
   </td>
   <td>Штат плательщика — только для стран США и Канада. Необязательный параметр.
   </td>
   <td>Строка, два символа, например, FL.
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Телефон плательщика в международном формате. Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Issuer
   </td>
   <td>Наименование банка, выпустившего карту. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 100 символов.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>OrderDescription
   </td>
   <td>Описание заказа, отображаемое для плательщика на платёжных формах и в e-mail уведомлениях. \
Необязательный параметр.
   </td>
   <td>Строка в кодировке UTF-8 в закодированном виде (url encode). Максимум — 100 символов. Разрешенные символы: буквы, цифры, знаки препинания.
   </td>
  </tr>
  <tr>
   <td>Любые другие
   </td>
   <td>Любые другие параметры.
   </td>
   <td>Любой формат.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>xml — результат в формате XML;
<p>
text — результат в текстовом формате.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)».  \
Для формирования SecurityKey в запросе к методу Moto, может быть использован либо секретный ключ PrivateSecurityKey, либо ключ PaymentKey.

В случае использования PrivateSecurityKey, в качестве аргумента к функции вычисления хеша подставляется:

Если указан OrderDescription


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PrivateSecurityKey={PrivateSecurityKey}
```


Если OrderDescription не указан


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```


В случае использования PaymentKey, в качестве аргумента к функции вычисления хеша подставляется:

Если указан OrderDescription


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PaymentKey={PaymentKey}
```


Если OrderDescription не указан


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentKey={PaymentKey}
```


В ответ на запрос, метод отдает следующие данные:

Если была ошибка формата:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Тип данных</strong>
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код ошибки. Обязательный элемент.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Описание ошибки. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом: 


```
Code={Code}&Message={Message}
```


Если XML:


```
<error>
	<code>{Code}</code>
	<message>{Message}</message>
</error>
```


Если ошибок формата не было:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Идентификатор транзакции. Обязательный элемент.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Auth. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код результата. \
Если Code = 6001, то плательщик должен пройти 3DS-авторизацию на сайте эмитента. После 3DS-авторизации нужно вызвать метод 3DS, который описан в разделе «Метод 3DS».  \
Обязательный элемент.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Статус транзакции. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Код ошибки. Только, если платеж не выполнен.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Ссылка на повторные платежи по данной карте. Только, если платеж выполнен, и ТСП подписано на услугу повторных платежей.
   </td>
   <td>Строка, максимальная длина — 100 символов.
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Код страны, определенный по IP-адресу плательщика. Необязательный элемент.
   </td>
   <td>2 символа в соответствии со стандартом <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Код страны банка,  \
выпустившего карту. \
Необязательный элемент.
   </td>
   <td>2 символа в соответствии со стандартом <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>SpecialConditions
   </td>
   <td>Особые условия. \
Необязательный элемент.
   </td>
   <td>Строка, одно или несколько значений, перечисленных через запятую:  \
ValidationRequired — рекомендуется проверка плательщика.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td colspan="3" ><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td colspan="2" >Result
   </td>
   <td>Ok — платеж выполнен;
<p>
Error — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td colspan="2" >Status
   </td>
   <td>Pending — платеж выполнен;
<p>
PreAuthorized — платеж выполнен, требуется подтверждение;
<p>
Declined — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td colspan="2" >ErrorCode
   </td>
   <td>1 — техническая проблема; плательщику стоит повторить запрос спустя некоторое время;
<p>
2 — провести платеж по банковской карте невозможно; плательщику стоит воспользоваться другим способом оплаты;
<p>
3 — платеж отклоняется банком-эмитентом карты; плательщику стоит, выяснить причину отказа в банке и повторить запрос.
   </td>
  </tr>
</table>


Если ошибок формата не было:

Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
Id={Id}&Operation=Auth&Result={Result}&Code={Code}&Status={Status}&binCountry={CountryCode}
```


Если XML:


```
<transaction>
	<id>{Id}</id>
	<operation>Auth</operation>
	<result>{Result}</result>
	<code>{Code}</code>
	<status>{Status}</status>
	<binCountry>{CountryCode}</binCountry>
</transaction>
```



## **Метод 3DS**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/auth/3ds/](https://secure.payonlinesystem.com/payment/transaction/auth/3ds/)  \
Описание: авторизует карту и блокирует на ней указанную сумму после прохождения плательщиком 3DS-авторизации. Используется для авторизации платежа по карте, подписанной на 3-D Secure. Следует использовать, если метод <a href="#/?id=Метод-auth">Auth</a> вернул ошибку 6001 и передал параметры **PaReq**, **ASCUrl** и **PD**.


    <p style="text-align: right">
<strong><em>Что такое 3-D Secure </em></strong></p>


Предложенный мировой платежной системой VISA и принятый за стандарт другими платежными системами, 3 D-Secure является XML-протоколом, который используется для двухфакторной аутентификации пользователя в качестве дополнительного уровня безопасности для онлайн-кредитных и дебетовых карт. \
В работе протокол 3-D Secure чаще всего выглядит так: держатель карты совершает оплату на сайте интернет-магазина, вводит данные карты, после этого направляется на страницу банка-эмитента, где вводит код, полученный в СМС. После этого оплата успешно завершается.

<img src="/img/3ds.jpg">

Название 3-D происходит от 3 Domain (три домена), так как в проверке платежа по данному протоколу участвуют организации на трех доменах: домен эмитента (плательщик и банк-эмитент), домен эквайера (банк, обрабатывающий платеж, и интернет-магазин) и домен взаимодействия (МПС). Так выглядит упрощенная схема проверки платежа по протоколу 3-D Secure:  \
Плательщик вводит данные карты в интернет-магазине, они достигают банка-эквайера (1), отправляются в МПС (2), откуда направляются в банк-эмитент (3). О том, какой банк эмитировал карту можно узнать по первым шести цифрам номера карты. Банк-эмитент сообщает, что карта подписана на 3-D Secure, формирует уникальный код, а также ссылку на страницу ввода кода (4). Ссылка возвращается в ТСП или IPSP (5), которые делают редирект в браузере держателя карты на эту страницу (6,7). В этот момент эмитент отправляет держателю карты по другому каналу (например, через СМС) временный пин-код, который он вводит на странице. В случае корректного ввода кода, банк-эмитент сообщает об успешном завершении проверки (8), и средства списываются с карты плательщика (9, 10).

Передача параметров метода выполняется исключительно методом POST в кодировке UTF-8

Если при вызове метода <a href="#/?id=Метод-auth">Auth</a> сервер вернул ошибку с кодом 6001 (Result=Error, Code=6001), это значит, что плательщик должен пройти 3-D Secure авторизацию на сайте банка-эмитента. \
Весь процесс проведения транзакции в этом случае состоит из следующих этапов:



*   
Вызов метода Auth и получение кода 6001;


*   
Перенаправление плательщика на сайт эмитента;
Обработка ответа от сайта эмитента;

Вызов метода 3DS с передачей параметров, полученных от сайта эмитента.

Вместе с кодом 6001 в ответ на вызов Auth сервер возвращает дополнительные параметры, относящиеся к 3-D Secure — **PaReq**, **ASCUrl** и **PD**. В случае, когда для ответа используется xml-формат, эти параметры помещаются в отдельный узел с именем threedSecure.

Пример:


```
<transaction>
	<id>1015368</id>
	<operation>Auth</operation>
	<result>Error</result>
	<status>Awaiting3DAuthentication</status>
	<code>6001</code>
	<errorCode>6001</errorCode>
	<errorCode>4</errorCode>
	<threedSecure>                 
		<pareq>
			eJxVUctuwkAM/BXEtRL7SHgUGUsUUMuBiLZQqcdosUjaJiSbTQv9+nrDqxwiecbe8WQMq8QSTV/J
			1JYQFlRV8ZZa6WbUVlJ1g15PSqnbCMvxC5UI32SrdJej6siOBnGG/NCaJM4dQmzKh3mEodah7oI4
			QcjIzqeodBAyeQSQxxmho8plBKIBYHZ17uwBZT8EcQZQ2y9MnCuGQpzHPQXiunZZ+6piiX26wflk
			vL35prOfxWr9G328j0D4CdjEjlBLpWRfDVpKDYNgGLC3hoc487tRd6TkfzgCKPyO8anjG/8J4AQt
			5eaAg5BbFwS0L3Y5+ScgLjWIq+HJk0/NOI7ELihKVvpt/Wmi2ePdc3mflMV2xJ5PA14t5UgUO2/k
			PADhJcTpRBxKc0Subo77BwN5ohE=
		</pareq>
		<acsurl>https://dropit.3dsecure.net:9443/PIT/ACS</acsurl>
		<pd>OXf4nrsM4Oi0N7TbFRrQZdbaFQ8M0Dc0WGZUOdBPZ3C2NXIrKlKObWBLTtzeknQY</pd>
	</threedSecure>
	<binCountry></binCountry>
</transaction>
```


Получив код 6001, необходимо направить плательщика на страницу, адрес которой указан в параметре ASCUrl. Переход на страницу ASCUrl должен быть осуществлен методом POST с передачей параметров PaReq, MD и TermUrl:

**PaReq** — значение параметра PaReq из ответа сервера PayOnline на вызов Auth.  \
**MD** — номер транзакции и значение параметра PD из ответа сервера PayOnline на вызов <a href="#/?id=Метод-auth">Auth</a>, разделенные точкой с запятой. Для примера выше MD будет равен


```
"1015368;OXf4nrsM4Oi0N7TbFRrQZdbaFQ8M0Dc0WGZUOdBPZ3C2NXIrKlKObWBLTtzeknQY"
```


**TermUrl —** адрес страницы на сервере мерчанта, которая будет обрабатывать результат авторизации плательщика на сайте банка-эмитента.

Пример:


```
<form method='post' action='https://dropit.3dsecure.net:9443/PIT/ACS'>
	<input 
	type='hidden' 
	name='PaReq' 
	value='eJxVUdFuwjAM/BXEB+AkLYMhY6mjk+hDEdpg711r0W5rC2m6wb5+SSkwHiL5zvbZOeMm18zhK6et
	ZsKYmybZ8aDI5kMp5Nh78IQQaki4Dl74QPjNuinqiuRIjBTCBdpGneZJZQiT9PAUrchXyldjhB5i
	yToKSSrPt+QZYJWUTIYbUzJCBzCt28roE4mJj3AB2Oovyo3ZzwAu5Y5CuI1dty5qrMSxyChaBLu7
	Fz7/xJvt7+ojniO4CswSw6SElGIipwMpZmM5U48IHY9J6WaTGglh/3AGuHczgj7jEv8JtA5qrtIT
	TX2buiLk476u2LUgXGOE28KLpXMtNdYSHfMq36i37WdWHpbvQRHFay+Y2537AqdWWEuk3byTcwDB
	SUB/ImtKd0Qb3R33D+2QoeE=' 
	/>
	<input type='hidden' name='TermUrl' value='http://yoursite.com/3ds.aspx' />
	<input type='hidden' name='MD' value='1015368;OXf4nrsM4Oi0N7TbFRrQZdbaFQ8M0
	Dc0WGZUOdBPZ3C2NXIrKlKObWBLTtzeknQY' />
	<input type='submit' value='Submit' />
</form>
```


После того, как плательщик введет авторизационные данные на странице ASCUrl, он будет перенаправлен на страницу, которую вы укажете в TermUrl. \
На страницу TermUrl будут переданы параметры **PARes** и **MD**. MD будет содержать то же значение, которое передавалось в запросе к ASCUrl. \
Из значения MD необходимо восстановить параметры TransactionID и PD, которые были получены во время вызова <a href="#/?id=Метод-auth">Auth</a>, после чего можно вызывать метод 3DS.

<p style="text-align: right">
<em>Параметры метода 3DS</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта.  \
Обязательный параметр.
   </td>
   <td>Целое число. 
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Идентификатор транзакции. \
Обязательный параметр.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>PARes
   </td>
   <td>Значение, полученное страницей TermUrl от банка-эмитента. Обязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>PD
   </td>
   <td>Значение, полученное при вызове метода <a href="#/?id=Метод-auth">Auth</a> с кодом ошибки 6001. Обязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. \
Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Любые другие
   </td>
   <td>Любые другие параметры.
   </td>
   <td>Любой формат.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>xml — результат в формате XML;
<p>
text — результат в текстовом формате.
   </td>
  </tr>
</table>


Параметр SecurityKey вычисляется хеш-функцией **md5 **от строки 


```
MerchantId={MerchantId}&TransactionId={TransactionId}&PARes={PARes}&PD={PD}&PrivateSecurityKey={PrivateSecurityKey}
```


{PrivateSecurityKey} — секретный ключ ТСП.

Вместо выражений в фигурных скобках подставляются значения параметров. Значение параметров MerchantId и PrivateSecurityKey ТСП получает при активации.

Формат ответа метода 3DS полностью совпадает с форматом ответа метода <a href="#/?id=Метод-auth">Auth</a>.


## **Метод Complete**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/complete/](https://secure.payonlinesystem.com/payment/transaction/complete/)

Описание: подтверждает предварительно авторизованные транзакции в статусе [PreAuthorized](#Статусы-транзакций). Передача параметров выполняется методом GET или POST.

<p style="text-align: right">
<em>Параметры метода Complete</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. Обязательный параметр.
   </td>
   <td>Целое число. 
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Идентификатор транзакции. \
Обязательный параметр.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма подтверждения в валюте оригинальной транзакции. Допускается полное или частичное подтверждение средств платежа, т.е. указанная сумма должна быть меньше или равна сумме транзакции. \
Необязательный параметр, по умолчанию происходит подтверждение на всю сумму транзакции.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>ExtendedData
   </td>
   <td>Отображение дополнительной информации (код авторизации шлюза банка и маска номера карты 6х4).  \
Необязательный параметр.
   </td>
   <td>Целое число. Допустимое значение — 1. При любых других значениях в ответе на запрос придет ошибка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>xml — результат в формате XML;
<p>
text — результат в текстовом формате.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)».

Если сумма подтверждения не указана, то в качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&TransactionId={TransactionId}&PrivateSecurityKey={PrivateSecurityKey}
```


Если сумма подтверждения присутствует, то в качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&TransactionId={TransactionId}&Amount={Amount}&PrivateSecurityKey={PrivateSecurityKey}
```


В ответ на запрос с корректными значениями параметров, метод возвращает следующие данные:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Идентификатор транзакции. \
Обязательный параметр.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Complete. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма подтверждения. Присутствует в ответе, только если параметр был указан в запросе.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Сообщение: в случае успешного списания — Completed, в противном случае — описание ошибки. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>AuthCode
   </td>
   <td>Код авторизации шлюза банка.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Card6x4
   </td>
   <td>Маска номера карты.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok — транзакция подтверждена;
<p>
Error — транзакция не подтверждена.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
TransactionId={TransactionId}&Operation=Complete&Amount={Amount}&Result={Result}&Message={Message}
```


Если XML и указана сумма подтверждения:


```
<transaction>
	<id>{id}</id>
	<operation>Complete</operation>
	<amount>{amount}</amount>
	<result>{result}</result>
	<message>{message}</message>
</transaction>
```



## **Метод Rebill**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/rebill/](https://secure.payonlinesystem.com/payment/transaction/rebill/)

Описание: создает новую транзакцию с типом [Purchase](#Статусы-транзакций) или [PreAuth](#Статусы-транзакций), в зависимости от настроек ТСП.  \
При создании транзакции используется та же платежная информация, которая была указана в одном из предыдущих вызовов метода <a href="#/?id=Метод-auth">Auth</a> или <a href="#/?id=Метод-applepay">ApplePay</a>.  \
Передача параметров выполняется методом GET или POST.


    <p style="text-align: right">
<strong><em>Внимание</em></strong></p>



    <p style="text-align: right">
<em>Если вы желаете получать параметр RebillAnchor в CallBack-ах, то адрес <a href="#Адрес-обратной-связи-callbackurl">CallBackURL</a> должен быть в <strong>защищенной зоне</strong> <strong>https://,</strong> так как<strong> </strong>в противном случае параметр RebillAnchor будет исключен из CallBack, о чем вы получите уведомление на email.</em></p>


<p style="text-align: right">
<em>Параметры метода Rebill</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта.  \
Обязательный параметр.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Ключ повторной транзакции, полученный в параметре RebillAnchor при вызове метода <a href="#/?id=Метод-auth">Auth</a> или <a href="#/?id=Метод-applepay">ApplePay</a>. \
Обязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Номер заказа. Обязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма. Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 10.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта. Обязательный параметр.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>OrderDescription
   </td>
   <td>Описание заказа, отображаемое для плательщика на платёжных формах и в e-mail уведомлениях. \
Необязательный параметр. 
   </td>
   <td>Строка в кодировке UTF-8 в закодированном виде (url encode). Максимум — 100 символов. Разрешенные символы: буквы, цифры, знаки препинания.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text — результат в текстовом формате
<p>
xml — результат в формате XML.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)». \
В качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&RebillAnchor={RebillAnchor}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```


В ответ на запрос с корректными значениями параметров, метод возвращает следующие данные:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Идентификатор транзакции. \
Обязательный параметр.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Rebill. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код результата. \
Обязательный элемент.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Статус транзакции. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Код ошибки. \
Только если платеж не выполнен.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. \
Только если использовалась \
маршрутизация.
   </td>
   <td>Целое число.
   </td>
  </tr>
</table>



    <p style="text-align: right">
<strong><em>MerchantId</em></strong></p>



    <p style="text-align: right">
<em>Данный параметр передаётся, если исходная транзакция была маршрутизирована на другой MerchantId. \
В этом случае для дальнейшей авторизации транзакции ТСП необходимо оперировать параметрами, пришедшими в ответ на вызов метода Rebill. \
В том числе при запросе <a href="#/?id=Метод-3ds">#3DS</a>
, надо использовать MerchantID (и его PrivateSecurityKey), содержащийся в ответе вызова метода Rebill.</em></p>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok — платеж выполнен;
<p>
Error — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Pending — платеж выполнен;
<p>
PreAuthorized — платеж выполнен, требуется подтверждение;
<p>
Declined — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>1 — техническая проблема, повторите запрос через некоторое время;
<p>
2 — транзакция отклонена фильтрами, повторите через СУТКИ. Число повторных попыток ограничивается индивидуальными настройками. Далее требуется повторное прохождение плательщика через форму;
<p>
3 — транзакция отклоняется банком-эмитентом. Возможен повтор попыток не более пяти раз в сутки в течение 3 дней;
<p>
4 — транзакция отклоняется банком-эмитентом.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
Id={Id}&Operation=Rebill&Result={Result}&Status={Status}&Code={Code}&ErrorCode={ErrorCode}&MerchantId={MerchantId}
```


Если XML:


```
<transaction>
	<id>{id}</id>
	<operation>Rebill</operation>
	<result>{result}</result>
	<status>{status}</status>
	<code>{code}</code>
	<errorCode>{errorCode}</errorCode>
	<merchantId>{merchantId}</merchantId>
</transaction>
```



    <p style="text-align: right">
<strong><em>Внимание</em></strong></p>



    <p style="text-align: right">
<em>В случае, если при авторизации платежа по привязанной карте (rebill) произошло отклонение транзакции с кодом из списка ниже, повторная попытка безакцептного списания по этой карте должна производиться не ранее, чем через четыре календарных дня с момента первоначального ответа Declined.</em></p>



    <p style="text-align: right">
<em>5201 Account not found \
5205 Insufficient funds \
5301 Card expired or incorrect date \
5302 Declined by issuer  \
5303 Unsupported transaction  \
5305 Lost or stolen card  \
5309 Card activity limit exceeded \
5310 Card amount limit exceeded</em></p>



## **Метод Refill**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/refill/](https://secure.payonlinesystem.com/payment/transaction/refill/)

Описание: создает новую транзакцию с типом <a href="#/?id=Метод-refill">Refill</a>. При создании транзакции используется та же платежная информация, которая была указана в одном из предыдущих вызовов <a href="#/?id=Метод-auth">Auth</a>. \
Передача параметров выполняется методом GET или POST.

<p style="text-align: right">
<em>Параметры метода Refill</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. Обязательный параметр.
   </td>
   <td>Целое число. 
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Ключ повторной транзакции, полученный в параметре RebillAnchor при вызове метода <a href="#/?id=Метод-auth">Auth</a>. Обязательный параметр.
   </td>
   <td>Строка. 
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Номер заказа. \
Обязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма для зачисления на карту. Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта. \
Обязательный параметр.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре. 
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>PaymentInformationId
   </td>
   <td>Уникальный идентификатор персональных данных. \
Необязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text — результат в текстовом формате
<p>
xml — результат в формате XML.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)». \
В качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&RebillAnchor={RebillAnchor}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&Operation=Refill&PrivateSecurityKey={PrivateSecurityKey}
```



    <p style="text-align: right">
<strong><em>Параметр Operation</em></strong></p>



    <p style="text-align: right">
<em>Параметр Operation всегда имеет значение Operation=Refill_OCT и участвует только в расчете SecurityKey. \
В запросе к методу данный параметр не передаётся.</em></p>


В ответ на запрос с корректными значениями параметров, метод возвращает следующие данные:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Идентификатор транзакции. \
Обязательный параметр.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Refill. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код результата.  \
Обязательный элемент.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Статус транзакции.  \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Код ошибки. Только, если платеж не выполнен.
   </td>
   <td>Число.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok — платеж выполнен;
<p>
Error — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Pending — платеж выполнен;
<p>
Declined — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>1 — техническая проблема, повторите запрос через некоторое время;
<p>
2 — транзакция отклонена фильтрами, повторите через СУТКИ, Число повторных попыток ограничивается индивидуальными настройками. Далее потребуется повторное прохождение плательщика через форму;
<p>
3 — транзакция отклоняется банком-эмитентом. Возможен повтор попыток не более пяти раз в сутки в течение 3 дней;
<p>
4 — транзакция отклоняется банком-эмитентом. Требуется прохождение 3DS.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
Id={Id}&Operation=Refill&Result={Result}&Status={Status}&Code={Code}&ErrorCode={ErrorCode}
```


Если XML:


```
<transaction>
	<id>{id}</id>
	<operation>Refill</operation>
	<result>{result}</result>
	<status>{status}</status>
	<code>{code}</code>
	<errorCode>{errorCode}</errorCode>
</transaction>
```



## **Метод Refill_OCT**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/refill_oct/](https://secure.payonlinesystem.com/payment/transaction/refill_oct/)

Описание: создает новую транзакцию с типом <a href="#/?id=Метод-refill">Refill</a>. При создании транзакции используется та же платежная информация, которая была указана в одном из предыдущих вызовов <a href="#/?id=Метод-auth">Auth</a>. \
Передача параметров выполняется методом GET или POST.

<p style="text-align: right">
<em>Параметры метода Refill_OCT</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. Обязательный параметр.
   </td>
   <td>Целое число. 
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Номер карты. \
Обязательный параметр.
   </td>
   <td>13–19 цифр.
   </td>
  </tr>
  <tr>
   <td>CardExpDate
   </td>
   <td>Дата истечения срока действия карты. \
Обязательный параметр.
   </td>
   <td>Строка в формате MMYY — две цифры месяца и две цифры года, например, 0915.
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Номер заказа. \
Обязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма для зачисления на карту. Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 15.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта.  \
Обязательный параметр.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре. 
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>PaymentInformationId
   </td>
   <td>Уникальный идентификатор персональных данных. \
Необязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>E-Mail адрес плательщика. Обязательный параметр.
   </td>
   <td>Срока, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>CardHolderName
   </td>
   <td>Имя плательщика, как отпечатано на карте. \
Обязательный параметр.
   </td>
   <td>Строка, только латинские символы, максимальная длина — 100 символов, например, Aleksey Petrov.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text — результат в текстовом формате
<p>
xml — результат в формате XML.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)».

В качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&CardNumber={CardNumber}&CardExpDate={CardExpDate}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&Operation=Refill_OCT&PrivateSecurityKey={PrivateSecurityKey}
```



    <p style="text-align: right">
<strong><em>Параметр Operation</em></strong></p>



    <p style="text-align: right">
<em>Параметр Operation всегда имеет значение Operation=Refill_OCT и участвует только в расчете SecurityKey. \
В запросе к методу данный параметр не передаётся.</em></p>


В ответ на запрос с корректными значениями параметров, метод возвращает следующие данные:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Идентификатор транзакции. \
Обязательный параметр.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Refill. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код результата.  \
Обязательный элемент.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Статус транзакции.  \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Код ошибки. Только, если платеж не выполнен.
   </td>
   <td>Число.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok — платеж выполнен;
<p>
Error — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Pending — платеж выполнен;
<p>
Declined — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>1 — техническая проблема, повторите запрос через некоторое время;
<p>
2 — транзакция отклонена фильтрами, повторите через СУТКИ, Число повторных попыток ограничивается индивидуальными настройками. Далее потребуется повторное прохождение плательщика через форму;
<p>
3 — транзакция отклоняется банком-эмитентом. Возможен повтор попыток не более пяти раз в сутки в течение 3 дней;
<p>
4 — транзакция отклоняется банком-эмитентом. Требуется прохождение 3DS.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
Id={Id}&Operation=Refill_OCT&Result={Result}&Status={Status}&Code={Code}&ErrorCode={ErrorCode}
```


Если XML:


```
<transaction>
	<id>{id}</id>
	<operation>Refill_OCT</operation>
	<result>{result}</result>
	<status>{status}</status>
	<code>{code}</code>
	<errorCode>{errorCode}</errorCode>
</transaction>
```



## **Метод Void**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/void/](https://secure.payonlinesystem.com/payment/transaction/void/)

Описание: отменяет выполненные транзакции в статусе [Pending](#Статусы-транзакций) или [PreAuthorized](#Статусы-транзакций). Передача параметров выполняется методом GET или POST.

<p style="text-align: right">
<em>Параметры метода Void</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. Обязательный параметр.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Идентификатор транзакции. \
Обязательный параметр.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text — результат в текстовом формате;
<p>
xml — результат в формате XML.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)», в качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&TransactionId={TransactionId}&PrivateSecurityKey={PrivateSecurityKey}
```


В ответ на запрос с корректными значениями параметров, метод возвращает следующие данные:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Идентификатор транзакции. \
Обязательный параметр.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Void. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Сообщение: в случае успешной отмены — Voided, в случае неуспешной — описание ошибки. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok — транзакция отменена,
<p>
Error — транзакция не отменена;
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
TransactionId={TransactionId}&Operation=Void&Result={Result}&Message={Message}
```


Если XML:


```
<transaction>
	<id>{id}</id>
	<operation>Void</operation>
	<result>{result}</result>
	<message>{message}</message>
</transaction>
```



## **Метод Refund**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/refund/](https://secure.payonlinesystem.com/payment/transaction/refund/)

Описание: создает новую транзакцию с типом [Refund](#Типы-транзакций) для возврата средств по транзакции в статусе [Settled](#Статусы-транзакций). \
Передача параметров выполняется методом GET или POST.

<p style="text-align: right">
<em>Параметры метода Refund</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. Обязательный параметр.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Идентификатор оригинальной транзакции. \
Обязательный параметр.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма для возврата в валюте оригинальной транзакции. Допускается полный или частичный возврат платежа, т.е. указанная сумма должна быть меньше или равна сумме оригинальной транзакции. \
Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text — результат в текстовом формате,
<p>
xml — результат в формате XML.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)».

В качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&TransactionId={TransactionId}&Amount={Amount}&PrivateSecurityKey={PrivateSecurityKey}
```


В ответ на запрос с корректными значениями параметров, метод возвращает следующие данные:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Идентификатор транзакции (возврата).  \
Обязательный элемент.
   </td>
   <td>Целое число (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Refund. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма возврата.  \
Обязательный элемент.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Сообщение: в случае успеха —Refunded, в случае неуспеха — описание ошибки. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok — возврат выполнен;
<p>
Error — возврат не выполнен.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
TransactionId={TransactionId}&Operation=Refund&Amount={Amount}&Result={Result}&Message={Message}
```


Если XML:


```
<transaction>
	<id>{id}</id>
	<operation>Refund</operation>
	<amount>{amount}</amount>
	<result>{result}</result>
	<message>{message}</message>
</transaction>
```



## **Метод Check**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/check /](https://secure.payonlinesystem.com/payment/transaction/check%20/) \
Описание: Метод проверяет информацию о карте плательщика и платеже в системе AntiFraud PayOnline. Передача параметров метода выполняется исключительно методом POST в кодировке UTF-8.

<p style="text-align: right">
<em>Параметры метода Check</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта.  \
Обязательный параметр.
   </td>
   <td>Целое число. 
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Идентификатор заказа в системе ТСП. Обязательный параметр.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Конечная сумма заказа. Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта заказа.  \
Обязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре. 
   </td>
  </tr>
  <tr>
   <td>Ip
   </td>
   <td>IP-адрес плательщика. Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Строка в формате xxx.xxx.xxx.xxx, например, 66.11.130.105
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>E-Mail адрес плательщика. Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Срока, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>CardHolderName
   </td>
   <td>Имя плательщика, как отпечатано на карте.  \
Обязательный параметр.
   </td>
   <td>Строка, только латинские символы, максимальная длина — 100 символов, например, Aleksey Petrov.
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Номер карты.  \
Обязательный параметр.
   </td>
   <td>13-19 цифр.
   </td>
  </tr>
  <tr>
   <td>CardExpDate
   </td>
   <td>Дата истечения срока действия карты.  \
Обязательный параметр.
   </td>
   <td>Строка в формате MMYY — две цифры месяца и две цифры года, например, 0915.
   </td>
  </tr>
  <tr>
   <td>CardCvv
   </td>
   <td>Проверочный код карты — последние три цифры на обратной стороне после подписи. Обязательный параметр.
   </td>
   <td>3 цифры.
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Код страны плательщика. Необязательный параметр.
   </td>
   <td>2 символа в соответствии со стандартом <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Город плательщика.  \
Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Address
   </td>
   <td>Адрес плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 150 символов. 
   </td>
  </tr>
  <tr>
   <td>Zip
   </td>
   <td>Почтовый индекс плательщика. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 10 символов.
   </td>
  </tr>
  <tr>
   <td>State
   </td>
   <td>Штат плательщика — только для стран США и Канада. Необязательный параметр.
   </td>
   <td>Строка, два символа, например, FL.
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Телефон плательщика в международном формате. Обязательность оговаривается индивидуально. По умолчанию должен быть передан.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Issuer
   </td>
   <td>Наименование банка, выпустившего карту. Необязательный параметр.
   </td>
   <td>Строка, максимальная длина — 100 символов.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Любые другие
   </td>
   <td>Любые другие параметры.
   </td>
   <td>Любой формат.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>xml — результат в формате XML; \
text — результат в текстовом формате.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)», в качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```


В ответ на запрос с корректными значениями параметров, метод возвращает следующие данные:

Если была ошибка формата:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Тип данных</strong>
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код ошибки. Обязательный элемент.
   </td>
   <td>Целое число. 
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Описание ошибки. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
Code={Code}&Message={Message}
```


Если XML:


```
<error>
	<code>{Code}</code>
	<message>{Message}</message>
</error>
```


Если ошибок формата не было:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Идентификатор транзакции. Обязательный элемент.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Check.  \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Представляет собой массив результатов выполнения фильтров, выполняющих проверку платежа в системе AntiFraud. Описание всех фильтров и настройки системы для ТСП производятся в индивидуальном порядке по запросу.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
Id={Id}&Operation=Check&Result={Result}
```


В данном случае массив результатов не возвращается, «Result» — это общий результат проверки: «ОК» в случае успеха, «Fail» в случае отклонения одним из фильтров.

Если XML:


```
<transaction>
	<id>{Id}</id>
	<operation>Check</operation>
	<result>
			<filter name="Filter1">
				<code >200<code>
				<description>OK</description>
			</filter>
			<filter name="Filter2">
				<code >2062<code>
				<description>
			Limit from one IP exceeded for 2:00 hours (192.168.1.1)
				</description>
			</filter>
	</result>
</transaction>>
```



## **Метод Search**

Адрес: [https://secure.payonlinesystem.com/payment/search/](https://secure.payonlinesystem.com/payment/search/)

Описание: возвращает информацию только об успешно проведенных платежах. Критерием поиска может быть OrderId или TransactionId. Передача параметров выполняется методом GET или POST.

<p style="text-align: right">
<em>Параметры метода Search</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. \
Обязательный параметр.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>OrderId 
   </td>
   <td>Номер заказа. \
Обязательный параметр, если не указан TransactionId.
   </td>
   <td>Строка, максимум 50 символов.
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Идентификатор транзакции. Обязательный параметр, если не указан OrderId.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text — результат в текстовом формате,
<p>
xml — результат в формате XML.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)». В качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&OrderId={OrderId}&PrivateSecurityKey={PrivateSecurityKey}
```


— в случае поиска транзакции по OrderId или


```
MerchantId={MerchantId}&TransactionId={TransactionId}&PrivateSecurityKey={PrivateSecurityKey}
```


— в случае поиска по TransactionId.

В ответ на запрос с корректными значениями параметров, и при условии, что платеж был выполнен, метод возвращает следующие данные:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Идентификатор транзакции. Обязательный элемент.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма платежа.  \
Обязательный элемент.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта платежа. \
Обязательный элемент.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Номер заказа.
   </td>
   <td>Строка, до 50 символов.
   </td>
  </tr>
  <tr>
   <td>DateTime
   </td>
   <td>Дата и время прохождение транзакции, Universal Coordinated Time (UTC).
   </td>
   <td>Дата и время в формате «yyyy-MM-dd HH<span>:</span>mm:ss», например, 2008-12-31 23<span>:</span>59:59
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Статус транзакции.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>PreAuthorized
<p>
Pending
<p>
Settled
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
TransactionId={TransactionId}&Amount={Amount}&Currency={Currency}&Order={OrderId}&DateTime={DateTime}&Status={Status}
```


Если XML:


```
<transaction>
	<id>{id}</id>
	<amount>{amount}</amount>
	<currency>{currency}</currency>
	<orderId>{orderId}</orderId>
	<dateTime>{dateTime}</dateTime>
	<status>{Status}</status>
</transaction>
```


Если платеж не найден, то возвращается пустая строка.


## **Метод List**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/list/](https://secure.payonlinesystem.com/payment/transaction/list/)

Описание: возвращает список всех транзакций за указанный период времени в часовом поясе UTC+0. Если параметры DateFrom и DateTill указаны в формате yyyy-MM-dd, то возвращаются транзакции с даты начала 00<span>:</span>00:00 до даты окончания 23<span>:</span>59:59. Передача параметров выполняется методом GET или POST.

<p style="text-align: right">
<em>Параметры метода List</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. \
Обязательный параметр.
   </td>
   <td>Целое число.
   </td>
  </tr>
  <tr>
   <td>DateFrom
   </td>
   <td>Начало периода. \
Обязательный параметр.
   </td>
   <td>Дата в формате yyyy-MM-dd или yyyy-MM-dd HH<span>:</span>mm:ss
   </td>
  </tr>
  <tr>
   <td>DateTill
   </td>
   <td>Окончание периода. Не более трех дней от начала периода. \
Обязательный параметр. 
   </td>
   <td>Дата в формате yyyy-MM-dd или yyyy-MM-dd HH<span>:</span>mm:ss
   </td>
  </tr>
  <tr>
   <td>Type
   </td>
   <td>Тип выгружаемых транзакций. Допускается указание несколько типов, перечисленных через запятую. Обязательный параметр. 
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Статус выгружаемых транзакций. Допускается указание нескольких статусов, перечисленных через запятую. Обязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре. 
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>ExtendedData
   </td>
   <td>Отображение дополнительной информации (код авторизации шлюза банка и маска номера карты 6х4). Необязательный параметр.
   </td>
   <td>Целое число. Допустимое значение — 1. При любых других значениях в ответе на запрос придет ошибка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Type
   </td>
   <td>Purchase — снятие денег;
<p>
Refund — возврат денег.
<p>
Refill — перевод денег.
<p>
Arbitrary Purchase — платёж по свободным реквизитам.
<p>
Card to Card Transfer — перевод денег со счета одной карты на счет другой.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>PreAuthorized,
<p>
Pending,
<p>
Settled,
<p>
Voided,
<p>
Declined.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text — результат в текстовом формате;
<p>
xml — результат в формате XML.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)». В качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&DateFrom={DateFrom}&DateTill={DateTill}&Type={Type}&Status={Status}&PrivateSecurityKey={PrivateSecurityKey}
```


Метод возвращает все транзакции, выполненные за период с даты начала 00<span>:</span>00:00.0000 до даты окончания 23<span>:</span>59:59.9999 по временной зоне UTC. \
В ответ на запрос с корректными значениями параметров, метод возвращает список транзакций, включающий следующие данные:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>Date
   </td>
   <td>Дата и время транзакции. Обязательный элемент.
   </td>
   <td>Дата и время в формате «yyyy-MM-dd HH<span>:</span>mm:ss».
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Идентификатор транзакции. Обязательный элемент.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Номер заказа. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Type
   </td>
   <td>Тип транзакции. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Статус транзакции. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Маска номера карты. Необязательный элемент.
   </td>
   <td>Строка, 16 символов.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма. \
Обязательный элемент.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта. Обязательный элемент.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>Gateway
   </td>
   <td>Тип шлюза. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>CardHolder
   </td>
   <td>Имя держателя карты. Необязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Телефон плательщика. Необязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>E-mail адрес плательщика. Необязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Город плательщика. Необязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>CountryCode
   </td>
   <td>Код страны плательщика. Необязательный элемент.
   </td>
   <td>Строка, два символа по <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Код страны, определенный по BIN эмитента карты. \
Необязательный элемент.
   </td>
   <td>Строка, два символа по <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>IpAddress
   </td>
   <td>IP-адрес плательщика. Необязательный элемент.
   </td>
   <td>Строка в формате xxx.xxx.xxx.xxx
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Код страны, определенный по IP-адресу. \
Необязательный элемент.
   </td>
   <td>Строка, два символа по <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>, например, RU.
   </td>
  </tr>
  <tr>
   <td>AuthCode
   </td>
   <td>Код авторизации шлюза банка. \
Необязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Card6x4
   </td>
   <td>Маска номера карты. Необязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>PresentmentDate
   </td>
   <td>Дата выплаты банком денежных средств по транзакции. Необязательный элемент.
   </td>
   <td>Дата в формате ГГГГ-ММ-ДД
   </td>
  </tr>
  <tr>
   <td>AccountID
   </td>
   <td>Идентификатор аккаунта клиента в системе мёрчанта
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>GatewayTransactionID
   </td>
   <td>Идентификатор транзакции на стороне шлюза
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Type
   </td>
   <td>Purchase — снятие денег;
<p>
Refund — возврат денег.
<p>
Refill — перевод денег.
<p>
Arbitrary Purchase — платёж по свободным реквизитам.
<p>
Card to Card Transfer — перевод денег со счета одной карты на счет другой.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Pending — транзакция в обработке;
<p>
Settled — транзакция завершена;
<p>
Voided — транзакция отменена;
<p>
Declined — транзакция отклонена.
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>Gateway
   </td>
   <td>Test — тестовый шлюз;
<p>
Real — «боевой» шлюз.
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то транзакции будут выгружены в формате CSV. Первой строкой указываются заголовки таблицы.

Если указан ContentType=xml, то транзакции будут выгружены в формате XML, название корневого элемента — list, содержимое элементов:


```
<transaction>
	<date>2010-02-17 16<span>:</span>00:31</date>
	<id>1011281</id>
	<orderId>f4f5e511-a0a8-4fa3-97ed-e70827b8775f</orderId>
	<type>Purchase</type>
	<status>Settled</status>
	<cardNumber>************0015</cardNumber>
	<amount>1000.00</amount>
	<currency>RUB</currency>
	<gateway>Test</gateway>
	<cardHolder>Test HolderName</cardHolder>
	<phone />
	<email>test@local.com</email>
	<city>Test City</city>
	<countryCode>RU</countryCode>
	<binCountry>RU</binCountry>
	<ipAddress>87.106.17.147</ipAddress>
	<ipCountry>RU</ipCountry>
	<accountId>17665964</accountId>
	<gatewayTransactionId>eab6ab45-83c5-43e9-aaed-f41bdb26d88</gatewayTransactionId>
</transaction>
```



## **Метод Card2Card**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/card2card/](https://secure.payonlinesystem.com/payment/transaction/card2card/)

Описание: создает новый перевод с карты на карту. При создании перевода используется та же платежная информация, которая была указана в одном из предыдущих вызовов <a href="#/?id=Метод-auth">Auth</a>.  \
Передача параметров выполняется методом GET или POST.

<p style="text-align: right">
<em>Параметры метода Card2Card</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. \
Обязательный параметр.
   </td>
   <td>Целое число. 
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Ключ повторной транзакции, полученный в параметре RebillAnchor при вызове метода <a href="#/?id=Метод-auth">Auth</a>. Обязательный параметр.
   </td>
   <td>Строка. 
   </td>
  </tr>
  <tr>
   <td>RecipientRebillAnchor
   </td>
   <td>Ключ повторной транзакции, полученный в параметре RebillAnchor при вызове метода <a href="#/?id=Метод-auth">Auth</a>. Обязательный параметр.
   </td>
   <td>Строка. 
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Номер заказа. \
Обязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма для зачисления на карту. \
Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта суммы зачисления и комиссии. \
Обязательный параметр.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>Commission
   </td>
   <td>Сумма комиссии за перевод. Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата – текст или xml. Необязательный параметр, по умолчанию text.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text — результат в текстовом формате;
<p>
xml — результат в формате XML.
   </td>
  </tr>
</table>


SecurityKey должен формироваться по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)».

В качестве аргумента к функции вычисления хеша подставляется:


```
MerchantId={MerchantId}&RebillAnchor={RebillAnchor}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&Commission={Commission}&RecipientRebillAnchor={RecipientRebillAnchor}&Operation=Card2Card&PrivateSecurityKey={PrivateSecurityKey}
```



    <p style="text-align: right">
<strong><em>Параметр Operation</em></strong></p>



    <p style="text-align: right">
<em>Параметр Operation всегда имеет значение Operation=Card2Card и участвует только в расчете SecurityKey. \
В запросе к методу данный параметр не передаётся.</em></p>


В ответ на запрос с корректными значениями параметров, метод возвращает следующие данные:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Идентификатор транзакции. \
Обязательный элемент.
   </td>
   <td>Целое число (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Название операции, Card2Card. \
Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Код результата. Обязательный элемент.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Статус транзакции. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Код ошибки. Только, если платеж не выполнен.
   </td>
   <td>Число.
   </td>
  </tr>
  <tr>
   <td>Pareq
   </td>
   <td>Параметр 3DS (см. описание метода 3DS)
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Acsurl
   </td>
   <td>Параметр 3DS
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Pd
   </td>
   <td>Параметр 3DS
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok — платеж выполнен;  \
Error — платеж не выполнен.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Pending — платеж выполнен;
<p>
Declined — платеж не выполнен;
<p>
Awaiting3DAuthentication — требуется проведение 3DS.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>1 — возникла техническая ошибка, повторите попытку через 10 мин.
<p>
2 — транзакция отклонена фильтрами, повторите через СУТКИ, но не более 5 попыток, после требуется повторное прохождение плательщика через форму;
<p>
3 —транзакция отклоняется банком-эмитентом. Возможен повтор попыток не более пяти раз в сутки в течение 3 дней;
<p>
4 — транзакция отклоняется банком-эмитентом. Требуется прохождение 3DS или следует прекратить дальнейшие операции Rebill с данным RebillAnchor. 
   </td>
  </tr>
</table>


Если входной параметр ContentType = text, то результат будет выглядеть следующим образом:


```
TransactionId={TransactionId}&Operation=Refill&Result={Result}&Status={Status}&Code={Code}&ErrorCode={ErrorCode}
```


Если XML:


```
<transaction>
	<id>{id}</id>
	<operation>Refill</operation>
	<result>{result}</result>
	<status>{status}</status>
	<code>{code}</code>
	<errorCode>{errorCode}</errorCode>
</transaction>
```



## **Метод Card2Card 3DS**

Адрес: [https://secure.payonlinesystem.com/payment/transaction/card2card/3ds/](https://secure.payonlinesystem.com/payment/transaction/card2card/3ds/)

Описание: завершает перевод с карты на карту после прохождения плательщиком 3DS-авторизации. Используется для завершения перевода по карте, подписанной на 3-D Secure. Следует использовать данный метод, если <a href="#/?id=Метод-card2card">Card2Card</a> вернул ошибку 6001 и передал параметры **PaReq**, **ASCUrl** и *.*PD**.

Процесс проведения перевода аналогичен процессу проведения платежа по карте, подписанной на 3D-Secure (см. описание <a href="#/?id=Метод-3ds">Метод 3DS</a>.

<p style="text-align: right">
<em>Параметры метода Card2Card 3DS:</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Идентификатор сайта. \
Обязательный параметр.
   </td>
   <td>Целое число. 
   </td>
  </tr>
  <tr>
   <td>PARes
   </td>
   <td>Значение, полученное страницей TermUrl от банка-эмитента
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>PD
   </td>
   <td>Значение, полученное при вызове метода <a href="#/?id=Метод-auth">Auth</a> с кодом ошибки 6001
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Формат вывода результата — текст или xml. Необязательный параметр, по умолчанию — text.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Любые другие
   </td>
   <td>Любые другие параметры
   </td>
   <td>Любой формат
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>xml — результат в формате XML; \
text — результат в текстовом формате.
   </td>
  </tr>
</table>


Параметр SecurityKey не вычисляется.

В ответ на запрос с корректными значениями параметров, метод возвращает следующие данные:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Идентификатор транзакции. Обязательный элемент.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Результат выполнения. Обязательный элемент.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Сообщение.
   </td>
   <td>Строка.
   </td>
  </tr>
</table>


Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok — платеж выполнен;  \
Error — платеж не выполнен.
   </td>
  </tr>
</table>



# **Адрес обратной связи (CallBackUrl)**

Адрес обратной связи (CallBackUrl) служит для уведомления сервера ТСП об успешных или неуспешных платежах. Параметры вызова, такие, как адрес уведомления об успешных платежах, адрес для уведомления о неуспешных платежах, метод и кодировка настраиваются в личном кабинете ТСП. \
Адрес вызывается со следующими параметрами:

<p style="text-align: right">
<em>Параметры вызова CallBackUrl</em></p>



<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Описание</strong>
   </td>
   <td><strong>Формат данных</strong>
   </td>
  </tr>
  <tr>
   <td>DateTime
   </td>
   <td>Дата и время платежа, часовой пояс GMT+0 (UTC).  \
Обязательный параметр.
   </td>
   <td>Дата и время в формате "yyyy-MM-dd HH<span>:</span>mm:ss", например 2008-12-31 23<span>:</span>59:59
   </td>
  </tr>
  <tr>
   <td>TransactionID
   </td>
   <td>Идентификатор транзакции. Обязательный параметр.
   </td>
   <td>Целое число (Long).
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Идентификатор заказа в системе ТСП. Обязательный параметр.
   </td>
   <td>Строка, максимальная длина — 50 символов.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Сумма транзакции. \
Обязательный параметр.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Валюта транзакции. Обязательный параметр.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Открытый ключ, подтверждающий целостность параметров запроса. Обязательный параметр.
   </td>
   <td>Строка, 32 символа в нижнем регистре.
   </td>
  </tr>
  <tr>
   <td>PaymentAmount
   </td>
   <td>Сумма транзакции в валюте платежа.
   </td>
   <td>Число с фиксированной точкой (Decimal), два знака после точки, например, 1500.99
   </td>
  </tr>
  <tr>
   <td>PaymentCurrency
   </td>
   <td>Валюта платежа.
   </td>
   <td>Строка, три символа.
   </td>
  </tr>
  <tr>
   <td>CardHolder
   </td>
   <td>Имя держателя карты. Необязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Маскированный номер карты — 12 символов ‘*’ и последние 4 цифры. \
Необязательный параметр.
   </td>
   <td>Строка, 13–16 символов.
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Код страны в соответствии с <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>. \
Необязательный параметр.
   </td>
   <td>Строка, 2 символа.
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Город. \
Необязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>Address
   </td>
   <td>Адрес. \
Необязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>IpAddress
   </td>
   <td>IP-адрес плательщика. \
Необязательный параметр.
   </td>
   <td>Строка.
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Код страны, определенный по IP-адресу. \
Необязательный параметр.
   </td>
   <td>Строка, два символа.
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Код страны, определенный по BIN эмитента карты. \
Необязательный параметр.
   </td>
   <td>Строка, два символа.
   </td>
  </tr>
  <tr>
   <td>SpecialConditions
   </td>
   <td>Особые условия. \
Необязательный параметр.
   </td>
   <td>Строка, одно или несколько значений, перечисленных через запятую:  \
ValidationRequired — рекомендуется проверка плательщика.
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Ссылка на повторные платежи по данной карте. Только, если платеж выполнен и ТСП подписано на услугу повторных платежей.
   </td>
   <td>Строка, максимальная длина — 100 символов.
   </td>
  </tr>
  <tr>
   <td>Все прочие параметры
   </td>
   <td>Все прочие параметры, которые были переданы в метод <a href="#/?id=Метод-auth">Auth</a>
   </td>
   <td>Любой тип данных.
   </td>
  </tr>
</table>

Возможные варианты перечислений:


<table>
  <tr>
   <td><strong>Название</strong>
   </td>
   <td><strong>Варианты</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>PaymentCurrency
   </td>
   <td>Код валюты в соответствии с <a href="http://www.iso.org/iso/ru/currency_codes">ISO 4217</a>
   </td>
  </tr>
</table>


SecurityKey формируется по алгоритму, описанному в разделе «[Параметр SecurityKey](#Параметр-SecurityKey)». В качестве аргумента к функции вычисления хеша подставляется:


```
DateTime={DateTime}&TransactionID={TransactionID}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```



# **Параметр SecurityKey**

Параметр «SecurityKey» вычисляется хеш-функцией **md5 **от строки запроса, дополненной секретным ключом ТСП.

Если строка запроса имеет вид


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}
```


то хеш вычисляется от строки:


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```


где {PrivateSecurityKey} — секретный ключ ТСП.

**Порядок следования параметров и регистр символов** **важен**!  \
Вместо выражений в фигурных скобках подставляются значения параметров. Значение параметров MerchantId и PrivateSecurityKey ТСП получает при активации.

Например, для параметров:  \
MerchantId = 12345,  \
OrderId = 56789, \
Amount = 9.99, \
Currency = USD, \
PrivateSecurityKey = 3844908d-4c2a-42e1-9be0-91bb5d068d22 \
хеш вычисляется от строки:


```
MerchantId=12345&OrderId=56789&Amount=9.99&Currency=USD&PrivateSecurityKey=3844908d-4c2a-42e1-9be0-91bb5d068d22
```


и будет равен 56a5663a5d72fe15124396754bbcb38c


    <p style="text-align: right">
<strong><em>Внимание</em></strong></p>



    <p style="text-align: right">
<em>Для вычисления параметра SecurityKey при формировании запроса к методам 

[Auth](#Метод-auth) и [ApplePay](#Метод-ApplePay), вместо PrivateSecurityKey может быть использован ключ <strong>PaymentKey</strong>.</em></p>


При использовании PaymentKey, если строка запроса имеет вид


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}
```


то хеш вычисляется от строки


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentKey={PaymentKey}
```


где {PaymentKey} — ключ PaymentKey. \
Значение параметра PaymentKey ТСП получает при активации.


# **Коды ответов**

Возможные значения параметра «Code» в результате выполнения методов 


[Auth](#Метод-auth), [ApplePay](#Метод-ApplePay) или [Rebill](#Метод-Rebill)

Код 200 — операция успешно выполнена. \
Код 1*** — технический сбой в работе системы. \
Код 2*** — операция заблокирована системой безопасности. \
Код 3*** — техническая ошибка на стороне банка. \
Код 6*** — требуется дополнительная аутентификация.

<table>
  <tr>
   <td><strong>Код</strong>
   </td>
   <td><strong>Значение</strong>
   </td>
  </tr>
  <tr>
   <td>4004
   </td>
   <td>Incorrect card number
   </td>
  </tr>
  <tr>
   <td>4005
   </td>
   <td>Incorrect card holder name
   </td>
  </tr>
  <tr>
   <td>4006
   </td>
   <td>Incorrect card verification code
   </td>
  </tr>
  <tr>
   <td>4007
   </td>
   <td>Incorrect card expiration date
   </td>
  </tr>
  <tr>
   <td>4008
   </td>
   <td>Incorrect country
   </td>
  </tr>
  <tr>
   <td>4009
   </td>
   <td>Incorrect city
   </td>
  </tr>
  <tr>
   <td>4010
   </td>
   <td>Incorrect state
   </td>
  </tr>
  <tr>
   <td>4011
   </td>
   <td>Incorrect zip code
   </td>
  </tr>
  <tr>
   <td>4012
   </td>
   <td>Incorrect address
   </td>
  </tr>
  <tr>
   <td>4013
   </td>
   <td>Incorrect bank issuer name
   </td>
  </tr>
  <tr>
   <td>4014
   </td>
   <td>Card expired
   </td>
  </tr>
  <tr>
   <td>4015
   </td>
   <td>Incorrect card type
   </td>
  </tr>
  <tr>
   <td>4016
   </td>
   <td>Order already paid
   </td>
  </tr>
  <tr>
   <td>4017
   </td>
   <td>Incorrect e-mail
   </td>
  </tr>
  <tr>
   <td>4018
   </td>
   <td>Incorrect ip-address
   </td>
  </tr>
  <tr>
   <td>4019
   </td>
   <td>Incorrect phone
   </td>
  </tr>
  <tr>
   <td>4021
   </td>
   <td>Incorrect RebillAnchor
   </td>
  </tr>
  <tr>
   <td>4022
   </td>
   <td>Rebill not allowed
   </td>
  </tr>
  <tr>
   <td>4024
   </td>
   <td>Incorrect security key
   </td>
  </tr>
  <tr>
   <td>4025
   </td>
   <td>Incorrect order ID
   </td>
  </tr>
  <tr>
   <td>4026
   </td>
   <td>Incorrect currency
   </td>
  </tr>
  <tr>
   <td>4027
   </td>
   <td>Incorrect merchant
   </td>
  </tr>
  <tr>
   <td>4028
   </td>
   <td>Incorrect valid until
   </td>
  </tr>
  <tr>
   <td>4029
   </td>
   <td>Incorrect transaction ID
   </td>
  </tr>
  <tr>
   <td>4030
   </td>
   <td>Incorrect IData
   </td>
  </tr>
  <tr>
   <td>4032
   </td>
   <td>Incorrect phone number
   </td>
  </tr>
  <tr>
   <td>4033
   </td>
   <td>Incorrect phone number format
   </td>
  </tr>
  <tr>
   <td>4300
   </td>
   <td>Incorrect params
   </td>
  </tr>
  <tr>
   <td>5201 
   </td>
   <td>Account not found
   </td>
  </tr>
  <tr>
   <td>5204
   </td>
   <td>Unable to process
   </td>
  </tr>
  <tr>
   <td>5205
   </td>
   <td>Insufficient funds
   </td>
  </tr>
  <tr>
   <td>5212
   </td>
   <td>Issuer suspected fraud
   </td>
  </tr>
  <tr>
   <td>5213
   </td>
   <td>Security or law violation
   </td>
  </tr>
  <tr>
   <td>5301
   </td>
   <td>Card expired or incorrect date
   </td>
  </tr>
  <tr>
   <td>5302
   </td>
   <td>Declined by issuer
   </td>
  </tr>
  <tr>
   <td>5303
   </td>
   <td>Unsupported transaction
   </td>
  </tr>
  <tr>
   <td>5304
   </td>
   <td>Financial institution prohibited
   </td>
  </tr>
  <tr>
   <td>5305
   </td>
   <td>Lost or stolen card
   </td>
  </tr>
  <tr>
   <td>5306
   </td>
   <td>Incorrect card status
   </td>
  </tr>
  <tr>
   <td>5307
   </td>
   <td>Limited card
   </td>
  </tr>
  <tr>
   <td>5308
   </td>
   <td>Unable to authorize
   </td>
  </tr>
  <tr>
   <td>5309
   </td>
   <td>Card activity limit exceeded
   </td>
  </tr>
  <tr>
   <td>5310
   </td>
   <td>Card amount limit exceeded
   </td>
  </tr>
  <tr>
   <td>5320
   </td>
   <td>Unsupported card
   </td>
  </tr>
  <tr>
   <td>5333
   </td>
   <td>Format error
   </td>
  </tr>
  <tr>
   <td>5334
   </td>
   <td>Processing timeout
   </td>
  </tr>
  <tr>
   <td>5396
   </td>
   <td>Processing server error
   </td>
  </tr>
  <tr>
   <td>5401
   </td>
   <td>Call issuer
   </td>
  </tr>
  <tr>
   <td>5410
   </td>
   <td>Incorrect 3D-Secure data
   </td>
  </tr>
  <tr>
   <td>5411
   </td>
   <td>Incorrect CVV2/CVC2
   </td>
  </tr>
  <tr>
   <td>5501
   </td>
   <td>Card expired—must eliminate
   </td>
  </tr>
  <tr>
   <td>5502
   </td>
   <td>Declined by issuer—must eliminate
   </td>
  </tr>
  <tr>
   <td>5809
   </td>
   <td>Gateway server error
   </td>
  </tr>
</table>