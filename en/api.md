# **Terms and abbreviations**


<table>
  <tr>
   <td><strong>Authorization</strong>
   </td>
   <td>—
   </td>
   <td>permission of the issuing bank to make the payment via bankcard.
   </td>
  </tr>
  <tr>
   <td><strong>Issuing bank</strong>
   </td>
   <td>—
   </td>
   <td>payment card issuer.
   </td>
  </tr>
  <tr>
   <td><strong>Merchant</strong>
   </td>
   <td>—
   </td>
   <td>retailer and service outlet, connected to the system.
   </td>
  </tr>
  <tr>
   <td><strong>IPS</strong>
   </td>
   <td>—
   </td>
   <td>International payment system.
   </td>
  </tr>
  <tr>
   <td><strong>System, PayOnline</strong>
   </td>
   <td>—
   </td>
   <td>Internet Payment Service Provider PayOnline.
   </td>
  </tr>
  <tr>
   <td><strong>Transaction</strong>
   </td>
   <td>—
   </td>
   <td>operation of transferring money from one account to another one.
   </td>
  </tr>
</table>


# **Types of transactions**

**Purchase**—a purchase, money transfer from the customer (cardholder)account  to the merchant account.

**Refund**—refund, money transfer from the merchant account to the customer (cardholder) account .

**Refill**—refill, money transfer from the merchant account to the customer (cardholder) account.

**Arbitrary Purchase**—payment to bank account .

**Card to Card Transfer**—money transfer from the card account to another card account.

**Statuses of transactions**



*   **Pending**—transaction authorization is approved, money is blocked on the cardholder account for debiting.
*   **Settled**—transaction amount debiting from the cardholder account is approved.
*   **Voided**—transaction is cancelled (annulled), amount of the transaction is unblocked at the cardholder account.
*   **Declined**—transaction is declined (authorization is not approved).
*   **PreAuthorized**—preliminary authorization. Debiting permission is received, money is blocked on the account, but debiting will happen only after transaction approval by the merchant. If permission is not received at the stated time (discussed individually), the transaction will be declined automatically.

# **Types of payments**

**Direct** — after authorization money is blocked on the account of cardholder and is written off automatically in 24 hours. This type of payment is set for all merchants integrated to the system by default.

**With preliminary authorization** — after authorization money is blocked on the account, but writing off happens only after transaction approval by merchant. This payment type is set at the  merchant request.

# **Transition statuses**

At the moment of transaction authorization request, data is verified on several levels and depending on the results of verification, the transaction changes its status to **Pending** (successful authorization) or **Declined** (declined transaction).

On a daily basis, confirmation of all transactions authorized during last 24 hours is fulfilled with sum of the authorized transaction being debited from the cardholder account, the status of the transaction changes from status **Pending** to status **Settled**.

Transaction in **Pending** status can be cancelled within 24 hours from the moment of authorization by making the **Void** operation. Such transaction status changes to **Voided**. Only merchant can void the transaction.

Transaction in status **Settled** cannot be cancelled, as it is considered to be accomplished, however it is possible to fulfill the refund operation — **Refund**. When making this operation a new transaction with type **Refund** is created, and statuses **Pending** or **Declined** can be assigned to it. Refundable amount can be less or equal to the   purchase amount. Refund operation can be made several times, unless the amount of the refunded money is equal to the amount of the original transaction.

# **Transition schema**

## **Direct payment**

<img src="en/img/auth.jpg">

## **Refund**

<img src="en/img/refund.jpg">

## **Payment with preliminary authorization**

<img src="en/img/preauth.jpg">

# **API Methods**

API method is a URL wich accepts POST HTTP-requests. \
All methods must be called over HTTPS protocol.

**Response format**

Every API method accepts parameter named ContentType, which defines the method response format. Response format can be either **_text _**or **_xml_**. In case of **text the **response body will contain a string with _name=value _pairs separated by ampersand:


```
Param1=Value1&Param2=Value2&…&ParamN=ValueN
```


For **xml **response body will contain xml with the root name “transaction”:


```
<transaction>
	<param1>{id}</param1>
	<param2>Refund</param2>
	…
	<paramN>{amount}</paramN>
</transaction>
```


## **Auth method**

Endpoint url:_ [https://secure.payonlinesystem.com/payment/transaction/auth/](https://secure.payonlinesystem.com/payment/transaction/auth/) \
Description:_ Authorizes a payment transaction. \
Http-method: POST \
Encoding: UTF-8

<p style="text-align: right">
<em>Auth request parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String, max length—50 chars
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Total amount of the order. Mandatory
   </td>
   <td>Fixed-point number (Decimal), two digits after point, e.g. 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency of the order. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public security key, confirming integrity of request parameters. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>Ip
   </td>
   <td>Customer’s IP address. \
Mandatory.
   </td>
   <td>String in format xxx.xxx.xxx.xxx, for example 66.11.130.105
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>Email address of the payer.  \
Optional
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>CardHolderName
   </td>
   <td>Name as printed on the card. Mandatory
   </td>
   <td>String, 100 letters max  \
for example John Smith
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Card number. Mandatory
   </td>
   <td>13–19 digits
   </td>
  </tr>
  <tr>
   <td>CardExpDate
   </td>
   <td>Card expiration date. Mandatory
   </td>
   <td>String in format MMYY—2 digits of the month and 2 of the year, for example 0915
   </td>
  </tr>
  <tr>
   <td>CardCvv
   </td>
   <td>Card verification value—last 3 digits on reverse side of the card (after the signature). Mandatory
   </td>
   <td>3 digits
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Customer’s residence country Mandatory, butspecified individually.
   </td>
   <td><a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> country code for example RU
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Customer’s city. Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Address
   </td>
   <td>Customer’s address. Mandatory, butspecified individually.
   </td>
   <td>String, 150 characters max
   </td>
  </tr>
  <tr>
   <td>Zip
   </td>
   <td>Customer’s ZIP code. Mandatory, but specified individually.
   </td>
   <td>String, 10 characters max
   </td>
  </tr>
  <tr>
   <td>State
   </td>
   <td>State for US or Canada citizens. Mandatory, butspecified individually.
   </td>
   <td>String, 2 letters, for example FL.
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Customer's phone number. Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Issuer
   </td>
   <td>Issuer name. Mandatory, butspecified individually.
   </td>
   <td>String, 100 characters max
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>OrderDescription
   </td>
   <td>Order description displayed for the payer on the payment form and e-mail notifications. Optional
   </td>
   <td>String UTF-8 encoded (url encode). 100 characters max. Symbols allowed: letters, numbers, punctuation marks
   </td>
  </tr>
  <tr>
   <td>Any number of custom parameters
   </td>
   <td>Any custom parameters. Custom parameters are returned in callback as they are. Optional
   </td>
   <td>Any format
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format. \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”

It is possible to use either private key PrivateSecurityKey or key PaymentKey to compose SecurityKey in Auth method request.

In case PrivateSecurityKey is used, the argument to the function, which calculates the hash, is described as:

In case OrderDescription is indicated 


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PrivateSecurityKey={PrivateSecurityKey}
```


In case OrderDescription is not indicated


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```


In case PaymentKey is used, the argument to the function, which calculates the hash, is described as:

In case OrderDescription is indicated


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PaymentKey={PaymentKey}
```


In case OrderDescription is not indicated


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentKey={PaymentKey}
```


**Response parameters**

In case of an error, the following parameters are returned:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Error code. Described in section “Error codes”. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Error description. Mandatory
   </td>
   <td>String
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
Code={Code}&Message={Message}
```


In case of XML:


```
<error>
	<code>{Code}</code>
	<message>{Message}</message>
</error>
```


In case of success, the following parameters are returned:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Opeation name, always Auth. andatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Operation result. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Result code. If Code = 6001, payer must pass 3DS authorization on Issuing bank webpage. After 3DS authorization  3DS method needs to be called, which is described in “3DS method” chapter. Mandatory
   </td>
   <td>Numeric. See section “<a href="#/en/api?id=response-codes">Response codes</a>” for possible value
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Transaction status. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Error code in case of error. Only if payment was not done.
   </td>
   <td>Numeric
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Token for repeated payments for this card. Only if the payment was completed and   merchant is signed for the recurring payments service. Optional
   </td>
   <td>String. Max length—100 chars
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Country code determined by customer’s IP-Address. Optional
   </td>
   <td>2 symbols, country <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> code, i.e. RU
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Country code, determined according to BIN of the card issuer.Optional
   </td>
   <td>2 symbols, country <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> code, i.e. RU
   </td>
  </tr>
  <tr>
   <td>SpecialConditions
   </td>
   <td>Special conditions. Optional
   </td>
   <td>String, one or several values, separated by commas: ValidationRequired—payer verification recomended.
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td colspan="2" ><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td colspan="2" >Result
   </td>
   <td>Ok—payment is performed, \
Error—payment is not performed.
   </td>
  </tr>
  <tr>
   <td colspan="2" >Status
   </td>
   <td>Pending—payment is performed, \
Declined—payment is not performed, \
Awaiting3DAuthentication—needed to perform 3DS.
   </td>
  </tr>
  <tr>
   <td colspan="2" >ErrorCode
   </td>
   <td>1—there is a technical error, the customer should try again to pay some time later, \
2—it is not possible to accept payment by bank card. Customer should use another payment method, \
3—payment is declined by the card issuing bank. customer should contact the bank to understand the decline reason and  try again
   </td>
  </tr>
</table>


**In case an error in format did NOT appear**

In case input parameter ContentType = text, the result will look like:


```
Id={Id}&Operation=Auth&Result={Result}&Code={Code}&Status={Status}&binCountry={CountryCode}
```


In case of XML:


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


## **ApplePay method**

Endpoint url:_ [https://secure.payonlinesystem.com/payment/transaction/applepay/](https://secure.payonlinesystem.com/payment/transaction/applepay/) \
Description:_ Authorizes a payment transaction. \
Http-method: POST \
Encoding: UTF-8

<p style="text-align: right">
<em>ApplePay request parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String, max length—50 chars
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Total amount of the order. Mandatory
   </td>
   <td>Fixed-point number (Decimal), two digits after point, e.g. 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency of the order. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public security key, confirming integrity of request parameters. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>Ip
   </td>
   <td>Customer’s IP address. Mandatory.
   </td>
   <td>String in format xxx.xxx.xxx.xxx, for example 66.11.130.105
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>Email address of the payer.  \
Optional
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Customer’s residence country. Mandatory, but specified individually.
   </td>
   <td><a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> country code for example RU
   </td>
  </tr>
  <tr>
   <td>PaymentToken
   </td>
   <td>Card data in encrypted form received from <a href="https://developer.apple.com/library/content/documentation/PassKit/Reference/PaymentTokenJSON/PaymentTokenJSON.html#//apple_ref/doc/uid/TP40014929-CH8-SW5">ApplePay</a>. Mandatory
   </td>
   <td>JSON
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Customer’s city. Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Address
   </td>
   <td>Customer’s address. Mandatory, but specified individually.
   </td>
   <td>String, 150 characters max
   </td>
  </tr>
  <tr>
   <td>Zip
   </td>
   <td>Customer’s ZIP code. Mandatory, but specified individually.
   </td>
   <td>String, 10 characters max
   </td>
  </tr>
  <tr>
   <td>State
   </td>
   <td>State for US or Canada citizens. Mandatory, but specified individually.
   </td>
   <td>String, 2 letters, for example FL.
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Customer's phone number. Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Issuer
   </td>
   <td>Issuer name. Mandatory, but specified individually.
   </td>
   <td>String, 100 characters max
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>OrderDescription
   </td>
   <td>Order description displayed for the payer on the payment form and e-mail notifications. Optional
   </td>
   <td>String UTF-8 encoded (url encode). 100 characters max. Symbols allowed: letters, numbers, punctuation marks
   </td>
  </tr>
  <tr>
   <td>Any number of custom parameters
   </td>
   <td>Any custom parameters. Custom parameters are returned in callback as they are. Optional
   </td>
   <td>Any format
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format. \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”

It is possible to use either private key PrivateSecurityKey or key PaymentKey to compose  SecurityKey in ApplePay method request. 

In case PrivateSecurityKey is used, the argument to the function, which calculates the hash, is described as:

In case OrderDescription is indicated 


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PaymentToken={PaymentToken}&PrivateSecurityKey={PrivateSecurityKey}
```


In case OrderDescription is not indicated 


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentToken={PaymentToken}&PrivateSecurityKey={PrivateSecurityKey}
```


In case PaymentKey is used, the argument to the function, which calculates the hash, is described as:

In case OrderDescription is indicated 


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PaymentToken={PaymentToken}&PaymentKey={PaymentKey}
```


In case OrderDescription is not indicated 


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentToken={PaymentToken}&PaymentKey={PaymentKey}
```


**Response parameters**

In case of an error, the following parameters are returned:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Error code. Described in section “Error codes”. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Error description. Mandatory
   </td>
   <td>String
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
Code={Code}&Message={Message}
```


In case of XML:


```
<error>
	<code>{Code}</code>
	<message>{Message}</message>
</error>
```


In case of success, the following parameters are returned:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Operation name, always Auth. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Operation result. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Result code. If Code = 6001, payer must pass 3DS authorization on Issuing bank webpage. After 3DS authorization  3DS method needs to be called, which  is described in “[3DS](#3DS-method)” chapter. Mandatory
   </td>
   <td>Numeric.  \
See section “<a href="#/en/api?id=response-codes">Response codes</a>” for possible value
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Transaction status. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Error code in case of error. \
Only if payment was not done.
   </td>
   <td>Numeric
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Token for repeated payments for this card. Only if the payment was completed and merchant is signed for the recurring payments service. Optional
   </td>
   <td>String. Max length—100 chars
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Country code determined by customer’s IP-Address. Optional
   </td>
   <td>2 symbols, country <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> code, i.e. RU
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Country code, determined according to BIN of the card issuer.Optional
   </td>
   <td>2 symbols, country <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> code, i.e. RU
   </td>
  </tr>
  <tr>
   <td>SpecialConditions
   </td>
   <td>Special conditions. Optional
   </td>
   <td>String, one or several values, separated by commas: ValidationRequired—payer verification recomended.
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td colspan="2" ><strong>Name</strong>
   </td>
   <td colspan="2" ><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td colspan="3" >Ok—payment is performed, \
Error—payment is not performed.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td colspan="3" >Pending—payment is performed, \
Declined—payment is not performed, \
Awaiting3DAuthentication—needed to perform 3DS.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td colspan="3" >1—there is a technical error, the customer should try again to pay some time later, \
2—it is not possible to accept payment by bank card. customer should use another payment method, \
3—payment is declined by the card issuing bank. customer should contact the bank to understand the decline reason and  try again
   </td>
  </tr>
</table>


**In case an error in format did NOT appear**

In case input parameter ContentType = text, the result will look like:


```
Id={Id}&Operation=Auth&Result={Result}&Code={Code}&Status={Status}&binCountry={CountryCode}
```


In case of XML:


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

## **GooglePay method**

Endpoint url:_ [https://secure.payonlinesystem.com/payment/transaction/googlepay/](https://secure.payonlinesystem.com/payment/transaction/googlepay/) \
Description:_ Authorizes a payment transaction. \
Http-method: POST \
Encoding: UTF-8

<p style="text-align: right">
<em>ApplePay request parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String, max length—50 chars
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Total amount of the order. Mandatory
   </td>
   <td>Fixed-point number (Decimal), two digits after point, e.g. 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency of the order. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public security key, confirming integrity of request parameters. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>Ip
   </td>
   <td>Customer’s IP address. Mandatory.
   </td>
   <td>String in format xxx.xxx.xxx.xxx, for example 66.11.130.105
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>Email address of the payer.  \
Optional
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Customer’s residence country. Mandatory, but specified individually.
   </td>
   <td><a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> country code for example RU
   </td>
  </tr>
  <tr>
   <td>PaymentToken
   </td>
   <td>Card data in encrypted form received from <a href="https://pay.google.com/intl/ru_ru/about/">GooglePay</a>. Mandatory
   </td>
   <td>JSON
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Customer’s city. Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Address
   </td>
   <td>Customer’s address. Mandatory, but specified individually.
   </td>
   <td>String, 150 characters max
   </td>
  </tr>
  <tr>
   <td>Zip
   </td>
   <td>Customer’s ZIP code. Mandatory, but specified individually.
   </td>
   <td>String, 10 characters max
   </td>
  </tr>
  <tr>
   <td>State
   </td>
   <td>State for US or Canada citizens. Mandatory, but specified individually.
   </td>
   <td>String, 2 letters, for example FL.
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Customer's phone number. Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Issuer
   </td>
   <td>Issuer name. Mandatory, but specified individually.
   </td>
   <td>String, 100 characters max
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>OrderDescription
   </td>
   <td>Order description displayed for the payer on the payment form and e-mail notifications. Optional
   </td>
   <td>String UTF-8 encoded (url encode). 100 characters max. Symbols allowed: letters, numbers, punctuation marks
   </td>
  </tr>
  <tr>
   <td>Any number of custom parameters
   </td>
   <td>Any custom parameters. Custom parameters are returned in callback as they are. Optional
   </td>
   <td>Any format
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format. \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”

It is possible to use either private key PrivateSecurityKey or key PaymentKey to compose  SecurityKey in ApplePay method request. 

In case PrivateSecurityKey is used, the argument to the function, which calculates the hash, is described as:

In case OrderDescription is indicated 


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PaymentToken={PaymentToken}&PrivateSecurityKey={PrivateSecurityKey}
```


In case OrderDescription is not indicated 


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentToken={PaymentToken}&PrivateSecurityKey={PrivateSecurityKey}
```


In case PaymentKey is used, the argument to the function, which calculates the hash, is described as:

In case OrderDescription is indicated 


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PaymentToken={PaymentToken}&PaymentKey={PaymentKey}
```


In case OrderDescription is not indicated 


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentToken={PaymentToken}&PaymentKey={PaymentKey}
```


**Response parameters**

In case of an error, the following parameters are returned:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Error code. Described in section “Error codes”. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Error description. Mandatory
   </td>
   <td>String
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
Code={Code}&Message={Message}
```


In case of XML:


```
<error>
	<code>{Code}</code>
	<message>{Message}</message>
</error>
```


In case of success, the following parameters are returned:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Operation name, always Auth. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Operation result. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Result code. If Code = 6001, payer must pass 3DS authorization on Issuing bank webpage. After 3DS authorization  3DS method needs to be called, which  is described in “[3DS](#3DS-method)” chapter. Mandatory
   </td>
   <td>Numeric.  \
See section “<a href="#/en/api?id=response-codes">Response codes</a>” for possible value
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Transaction status. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Error code in case of error. \
Only if payment was not done.
   </td>
   <td>Numeric
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Token for repeated payments for this card. Only if the payment was completed and merchant is signed for the recurring payments service. Optional
   </td>
   <td>String. Max length—100 chars
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Country code determined by customer’s IP-Address. Optional
   </td>
   <td>2 symbols, country <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> code, i.e. RU
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Country code, determined according to BIN of the card issuer.Optional
   </td>
   <td>2 symbols, country <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> code, i.e. RU
   </td>
  </tr>
  <tr>
   <td>SpecialConditions
   </td>
   <td>Special conditions. Optional
   </td>
   <td>String, one or several values, separated by commas: ValidationRequired—payer verification recomended.
   </td>
  </tr>
  <tr>
   <td>PAReq
   </td>
   <td>PAReq. Optional.
   </td>
   <td>PAReq sent if <a href="#/?id=Метод-3ds">3DS authentication</a> required 
   </td>
  </tr>

  <tr>
   <td>AcsUrl
   </td>
   <td>AcsUrl. Optional.
   </td>
   <td>Url you need to go in case of <a href="#/?id=Метод-3ds">3DS authentication</a>
   </td>
  </tr>

  <tr>
   <td>PD
   </td>
   <td>PD. Optional.
   </td>
   <td>PD sent if <a href="#/?id=Метод-3ds">3DS authentication</a> required 
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td colspan="2" ><strong>Name</strong>
   </td>
   <td colspan="2" ><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td colspan="3" >Ok—payment is performed, \
Error—payment is not performed.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td colspan="3" >Pending—payment is performed, \
Declined—payment is not performed, \
Awaiting3DAuthentication—needed to perform 3DS.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td colspan="3" >1—there is a technical error, the customer should try again to pay some time later, \
2—it is not possible to accept payment by bank card. customer should use another payment method, \
3—payment is declined by the card issuing bank. customer should contact the bank to understand the decline reason and  try again
   </td>
  </tr>
</table>


**In case an error in format did NOT appear**

In case input parameter ContentType = text, the result will look like:


```
Id={Id}&Operation=Auth&Result={Result}&Code={Code}&Status={Status}&binCountry={CountryCode}
```


In case of XML:


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


## **Moto method**

Endpoint url:_ [https://secure.payonlinesystem.com/payment/transaction/moto/](https://secure.payonlinesystem.com/payment/transaction/moto/) \
Description:_ Authorizes a payment transaction. \
Http-method: POST \
Encoding: UTF-8

<p style="text-align: right">
<em>Moto request parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String, max length—50 chars
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Total amount of the order. Mandatory
   </td>
   <td>Fixed-point number (Decimal), two digits after point, e.g. 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency of the order. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public security key, confirming integrity of request parameters. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>Ip
   </td>
   <td>Customer’s IP address. \
Mandatory.
   </td>
   <td>String in format xxx.xxx.xxx.xxx, for example 66.11.130.105
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>Email address of the payer.  \
Optional
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>CardHolderName
   </td>
   <td>Name as printed on the card. Mandatory
   </td>
   <td>String, 100 letters max  \
for example John Smith
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Card number. Mandatory
   </td>
   <td>13–19 digits
   </td>
  </tr>
  <tr>
   <td>CardExpDate
   </td>
   <td>Card expiration date. Mandatory
   </td>
   <td>String in format MMYY—2 digits of the month and 2 of the year, for example 0915
   </td>
  </tr>
  <tr>
   <td>CardCvv
   </td>
   <td>Card verification value—last 3 digits on reverse side of the card (after the signature). Optional
   </td>
   <td>3 digits
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Customer’s residence country Mandatory, butspecified individually.
   </td>
   <td><a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> country code for example RU
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Customer’s city. Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Address
   </td>
   <td>Customer’s address. Mandatory, butspecified individually.
   </td>
   <td>String, 150 characters max
   </td>
  </tr>
  <tr>
   <td>Zip
   </td>
   <td>Customer’s ZIP code. Mandatory, but specified individually.
   </td>
   <td>String, 10 characters max
   </td>
  </tr>
  <tr>
   <td>State
   </td>
   <td>State for US or Canada citizens. Mandatory, butspecified individually.
   </td>
   <td>String, 2 letters, for example FL.
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Customer's phone number. Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Issuer
   </td>
   <td>Issuer name. Mandatory, butspecified individually.
   </td>
   <td>String, 100 characters max
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>OrderDescription
   </td>
   <td>Order description displayed for the payer on the payment form and e-mail notifications. Optional
   </td>
   <td>String UTF-8 encoded (url encode). 100 characters max. Symbols allowed: letters, numbers, punctuation marks
   </td>
  </tr>
  <tr>
   <td>Any number of custom parameters
   </td>
   <td>Any custom parameters. Custom parameters are returned in callback as they are. Optional
   </td>
   <td>Any format
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format. \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”

It is possible to use either private key PrivateSecurityKey or key PaymentKey to compose SecurityKey in Moto method request.

In case PrivateSecurityKey is used, the argument to the function, which calculates the hash, is described as:

In case OrderDescription is indicated 


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PrivateSecurityKey={PrivateSecurityKey}
```


In case OrderDescription is not indicated


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```


In case PaymentKey is used, the argument to the function, which calculates the hash, is described as:

In case OrderDescription is indicated


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&OrderDescription={OrderDescription}&PaymentKey={PaymentKey}
```


In case OrderDescription is not indicated


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PaymentKey={PaymentKey}
```


**Response parameters**

In case of an error, the following parameters are returned:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Error code. Described in section “Error codes”. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Error description. Mandatory
   </td>
   <td>String
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
Code={Code}&Message={Message}
```


In case of XML:


```
<error>
	<code>{Code}</code>
	<message>{Message}</message>
</error>
```


In case of success, the following parameters are returned:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Opeation name, always Auth. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Operation result. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Result code. If Code = 6001, payer must pass 3DS authorization on Issuing bank webpage. After 3DS authorization  3DS method needs to be called, which is described in “3DS method” chapter. Mandatory
   </td>
   <td>Numeric. See section “<a href="#/en/api?id=response-codes">Response codes</a>” for possible value
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Transaction status. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Error code in case of error. Only if payment was not done.
   </td>
   <td>Numeric
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Token for repeated payments for this card. Only if the payment was completed and   merchant is signed for the recurring payments service. Optional
   </td>
   <td>String. Max length—100 chars
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Country code determined by customer’s IP-Address. Optional
   </td>
   <td>2 symbols, country <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> code, i.e. RU
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Country code, determined according to BIN of the card issuer.Optional
   </td>
   <td>2 symbols, country <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO-3166</a> code, i.e. RU
   </td>
  </tr>
  <tr>
   <td>SpecialConditions
   </td>
   <td>Special conditions. Optional
   </td>
   <td>String, one or several values, separated by commas: ValidationRequired—payer verification recomended.
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td colspan="2" ><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td colspan="2" >Result
   </td>
   <td>Ok—payment is performed, \
Error—payment is not performed.
   </td>
  </tr>
  <tr>
   <td colspan="2" >Status
   </td>
   <td>Pending—payment is performed, \
Declined—payment is not performed, \
Awaiting3DAuthentication—needed to perform 3DS.
   </td>
  </tr>
  <tr>
   <td colspan="2" >ErrorCode
   </td>
   <td>1—there is a technical error, the customer should try again to pay some time later, \
2—it is not possible to accept payment by bank card. Customer should use another payment method, \
3—payment is declined by the card issuing bank. customer should contact the bank to understand the decline reason and  try again
   </td>
  </tr>
</table>


**In case an error in format did NOT appear**

In case input parameter ContentType = text, the result will look like:


```
Id={Id}&Operation=Auth&Result={Result}&Code={Code}&Status={Status}&binCountry={CountryCode}
```


In case of XML:


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


## **3DS method**

Endpoint url: [https://secure.payonlinesystem.com/payment/transaction/auth/3ds/](https://secure.payonlinesystem.com/payment/transaction/auth/3ds/) Description: Authorizes a payment transaction after the customer completes 3DS-authorization_._Method_ _is used when [Auth](#auth-method) returns error code 6001 and passes parameters **PaReq**, **ASCUrl** и **PD**. \
Http-method: POST \
Encoding: UTF-8


    <p style="text-align: right">
<strong><em>What is 3D Secure</em></strong></p>


Offered by international payment system VISA and adopted as a standard by other payment systems, 3-D Secure is an XML protocol, which is used for two-factor user authentication as an additional layer of security for online credit and debit cards. In action 3-D Secure protocol often looks like this: cardholder makes a payment online in Internet store, enters his card data, that is sent then to issuing bank page to enter the code received in SMS. After that, the payment is successfully completed.

<img src="/en/img/3ds.png">

3D title comes from the 3 Domain (three domains), as in the verification of payment by this protocol involves the organization based on three domains: Issuer Domain (the payer and the issuing bank), Acquirer Domain (the bank that handles the payment and the online store) and Interaction Domain (IPS). Simplified diagram of a payment verification by 3-D Secure protocol looks like this:_

Payer enters the card data in online store, it reaches the acquirer bank (1), then is send to the IPS (2), finally, gets to the issuing bank (3). Issuing bank can be found by the first six digits of the card number. The issuing bank informs that the card is signed on 3-D Secure, generates a unique code and a link to the page with code entering (4). Link returns to the merchant or IPSP (5), which makes a redirect in the cardholders browser to this page (6.7). At this point, the issuer sends the cardholder a temporary PIN code by a different channel (eg via SMS), which he enters on the page. In case the correct code is entered, the issuing bank announces the successful completion of the test (8) and money is debited from the payer cards (9, 10)._

In case [Auth](#auth-method) returns error with code 6001 (Result=Error, Code=6001), the customer must be transferred to the issuer site to complete 3DS-authorization. \
In this case the transaction process consists of the following steps:



1. A call to Auth method returns error code 6001.
2. The customer is redirected to the issuer web-site.
3. Merchant processes the response from issuer web-site.
4. Merchant calls 3DS method and passes parameters received from issuer web-site.

Along with the code 6001 Auth method returns additional parameters related to 3D Secure—**PaReq**, **ASCUrl **and **PD**. If XML is used for the response, these additional parameters are enclosed in element named **threedSecure**:

[Example](http://www.lingvo-online.ru/ru/Search/Translate/GlossaryItemExtraInfo?text=%d0%bf%d1%80%d0%b8%d0%bc%d0%b5%d1%80&translation=example&srcLang=ru&destLang=en):


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


When Auth method returns code 6001, customer must be transferred to the page with the url contained in ASCUrl parameter. The transfer must be made using HTTP POST method and the parameters PaReq, MD and TermUrl should be passed to ASCUrl:

**PaReq**—the value returnd by auth-method.

**MD**—values of TransactionId and PD returned by [Auth](#auth-method) delimited by semicolon. For the example above MD will be 


```
"1015368,OXf4nrsM4Oi0N7TbFRrQZdbaFQ8M0Dc0WGZUOdBPZ3C2NXIrKlKObWBLTtzeknQY"
```


**TermUrl**—url of the page on merchant’s site that will handle the result of 3D Secure authorization and call 3DS method of PayOnline gateway.

Example:


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
	<input type='hidden' name='MD' value='1015368,OXf4nrsM4Oi0N7TbFRrQZdbaFQ8M0
	Dc0WGZUOdBPZ3C2NXIrKlKObWBLTtzeknQY' />
	<input type='submit' value='Submit' />
</form>
```


Once the customer completes the authorization on ASCUrl page, he is transferred to the TermUrl page. Parameters **PARes **and **MD **are passed to the TermUrl. Value of MD is exactly the same that was passed to ASCUrl. The values of TransactionId and PD must be extracted from MD, which were received during Auth method request, and then 3DS method can be initiated.

<p style="text-align: right">
<em>3DS method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>PARes
   </td>
   <td>Value received by TermUrl web page from the bank issuer. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>PD
   </td>
   <td>The value obtained by calling <a href="#/en/api?id=auth-method">Auth</a> with error code 6001. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public security key, confirming integrity of request parameters. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Any number of custom parameters
   </td>
   <td>Any custom parameters. Custom parameters are returned in callback as they are. Optional
   </td>
   <td>Any format
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format. \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey is calculated as **md5**-hash of the string:


```
MerchantId={MerchantId}&TransactionId={TransactionId}&PARes={PARes}&PD={PD}&PrivateSecurityKey={PrivateSecurityKey}
```


{PrivateSecurityKey}_—_secret merchant's key.

Order of parameters and case of characters are important. \
Parameter values should be substituted instead of expressions in braces.  \
Merchant receives parameter values such as MerchantId and PrivateSecurityKey at account activation. \
The response format of 3DS method is the same as response format of [Auth](#auth-method).

## **Complete method**

Endpoint url: [https://secure.payonlinesystem.com/payment/transaction/complete/](https://secure.payonlinesystem.com/payment/transaction/complete/) \
Description: _confirms preliminarily authorized transactions in status [PreAuthorized](#Transition=statuses).  \
Http-method: POST \
Encoding: UTF-8

<p style="text-align: right">
<em>Complete method request parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number(Long)
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Confirmed amount in the currency of the original transaction. Full or partial amount confirmation is possible. The stated amount should be less or equal to the amount of the transaction. Optional, the full amount confirmation occurs by default.
   </td>
   <td>Fixed-point number (Decimal), two digits after point, e.g. 1500.99
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public key, confirming integrity of parameters of the order. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ExtendedData
   </td>
   <td>Displaying additional information (bank gateway authorization code and mask 6x4 of the card number). Optional
   </td>
   <td>Integer number. Valid value—1. Any other value in the response returns error.
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format. \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”.

If the confirmation amount  is not specified, then the argument to the function, which  calculates the hash, is described as:


```
MerchantId={MerchantId}&TransactionId={TransactionId}&PrivateSecurityKey={PrivateSecurityKey}
```


If the confirmation amount is specified, then the argument to the function, which  calculates the hash, is described as:


```
MerchantId={MerchantId}&TransactionId={TransactionId}&Amount={Amount}&PrivateSecurityKey={PrivateSecurityKey}
```


Method returns the following data in response to request made with correct parametervalues:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Operation name, always Complete. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Confirmed amount. It is provided in the response only if this parameter was stated in the request.
   </td>
   <td>Numeric Fixed point number (Decimal), two digits after the point, i.e.: 1500.99
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Result of the action. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Message: in case of successful debiting—Completed, in case of not successful one—error description. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>AuthCode
   </td>
   <td>Bank gateway authorization code. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Card6x4
   </td>
   <td>Mask of the card number. Optional
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td colspan="3" ><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td colspan="2" >Result
   </td>
   <td colspan="2" >Ok—payment  is approved, \
Error—payment is not approved.
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
TransactionId={TransactionId}&Operation=Complete&Amount={Amount}&Result={Result}&Message={Message}
```


In case of XML and confirmed amount is indicated:


```
<transaction>
	<id>{id}</id>
	<operation>Complete</operation>
	<amount>{amount}</amount>
	<result>{result}</result>
	<message>{message}</message>
</transaction>
```


## **Rebill method**

Endpoint url: [https://secure.payonlinesystem.com/payment/transaction/rebill/](https://secure.payonlinesystem.com/payment/transaction/rebill/) \
Description: Authorizes a subsequent payment transaction_. _Transaction is created using the same billing info (card number etc.) which was used during the first [Auth](#auth-method) or [ApplePay](#ApplePay-method) request, when customer specified his billing information. \
Http-method: POST \
Encoding: UTF-8


    <p style="text-align: right">
<em>If you wish to receive RebillAnchor parameter in CallBacks, the address [CallbackURL](#CallbackURL) must be in the protected area https: //, as otherwise RebillAnchor option will be excluded from CallBack, and you will receive  \
an email notification.</em></p>


<p style="text-align: right">
<em>Rebill method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Rebill transaction key, received in parameter RebillAnchor in previous payment Callback. Mandatory
   </td>
   <td>String. Max length—100 chars
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Payment amount. Mandatory
   </td>
   <td>Fixed point number (Decimal), two digits after the point, i.e.: 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Payment currency. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public key, confirming integrity of parameters of the order. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>OrderDescription
   </td>
   <td>Order description displayed for the payer on the payment form and e-mail notifications. Optional
   </td>
   <td>String UTF-8 encoded (url encode). 100 characters max. Symbols allowed: letters, numbers, punctuation marks
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format, \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey](file:///C:\\Users\\Olga\\Downloads\\Tolko_NOVYJ_text!.docx#_Параметр_SecurityKey)<span style="text-decoration:underline;"> parameter</span>”.

The argument to the function, which calculates the hash, is described as:


```
MerchantId={MerchantId}&RebillAnchor={RebillAnchor}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```


Method returns the following data in response to the request made with correct parameter values:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Method name, Rebill. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Method result. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Method result code. \
Mandatory
   </td>
   <td>Number
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Transaction status. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Error code in case of error.
   </td>
   <td>Numeric
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline.
<p>
Only if routing was used.
   </td>
   <td>Integer number
   </td>
  </tr>
</table>



    <p style="text-align: right">
<strong><em>MerchantId </em></strong></p>



    <p style="text-align: right">
<em>This parameter is passed, if the original transaction was routed  \
to another MerchantId. \
In this case, for further transaction authorization it is necessary to use parameters that were received in response to  Rebill method call. \
It is also necessary to use MerchantID (and PrivateSecurityKey) at [3DS](#3DS-method) request, which are contained in the response to Rebill method call</em>.</p>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok—payment is performed, \
Error—payment is not performed.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Pending—payment is approved, \
PreAuthorized—payment is approved, but confirmation is required, \
Declined—payment is declined.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>1—there is a technical error, try again after some time, \
2—transaction is rejected by filters, repeat after 24 HOURS, the number of retries is limited by individual settings. Afterwards, the payer shall re-pass through the  form \
3—transaction is rejected by the issuing bank. Possible repeated attempts are upto five times per day for 3 days, \
4—transaction is rejected by the issuing bank.
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
Id={Id}&Operation=Rebill&Result={Result}&Status={Status}&Code={Code}&ErrorCode={ErrorCode}&MerchantId={MerchantId}
```


In case of XML:


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
<strong><em>Note</em></strong></p>



    <p style="text-align: right">
<em>In case if during payment authorization made  by binded card (rebill) the transaction was declined with the code from the list below, the further retry to perform the payment from this card must be made no earlier than four calendar days  \
after the initial declined response. \
5201 Account not found \
5205 Insufficient funds \
5301 Card expired or incorrect date \
5302 Declined by issuer  \
5303 Unsupported transaction  \
5305 Lost or stolen card  \
5309 Card activity limit exceeded \
5310 Card amount limit exceeded</em></p>


## **Refill method**

Endpoint URL: [https://secure.payonlinesystem.com/payment/transaction/refill/](https://secure.payonlinesystem.com/payment/transaction/refill/)<span style="text-decoration:underline;"> \
Description: _</span>creates a new transaction with [Refill type](#Types-of-transactions). Transaction is created using the same billing info, which was used during the first [Auth](#auth-method) request, when customer specified his billing information.. Http- Http-method: GET or POST \
Encoding: UTF-8

<p style="text-align: right">
<em>Refill method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Rebill transaction key, received in parameter RebillAnchor in <a href="#/en/api?id=auth-method">Auth</a> method call. Mandatory
   </td>
   <td>String 
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Payment amount. Mandatory
   </td>
   <td>Fixed point number (Decimal), two digits after the point, i.e.: 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Payment currency. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public key, confirming integrity of parameters of the request. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>PaymentInformationId
   </td>
   <td>Unique identifier of personal data. Mandatory is specified individually.
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format. \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”.

The argument to the function, which calculates the hash, is described as:


```
MerchantId={MerchantId}&RebillAnchor={RebillAnchor}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&Operation=Refill&PrivateSecurityKey={PrivateSecurityKey}
```


Method returns the following data in response to  request made with correct parameter values:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Operation name, Refill. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Performance results. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Result code. Mandatory
   </td>
   <td>Numeric
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Transaction status. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Error Code. Only in case if payment is not performed.
   </td>
   <td>Numeric
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok—payment is performed, \
Error—payment is not performed.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Pending—payment is performed, \
Declined—payment is not performed.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>1—a technical problem, try again after some time, \
2—transaction is rejected by filters, repeat after 24 HOURS, the number of retries is limited by individual settings. Afterwards, the payer should re-pass through the form, \
3—transaction is rejected by the issuing bank. Possible repeated attempts are upto five times per day for 3 days, \
4—transaction is rejected by the issuing bank. Requires passing 3DS.
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
Id={Id}&Operation=Refill&Result={Result}&Status={Status}&Code={Code}&ErrorCode={ErrorCode}
```


In case of XML:


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


## **Refill_OCT method**

Endpoint URL: [https://secure.payonlinesystem.com/payment/transaction/refill_oct/](https://secure.payonlinesystem.com/payment/transaction/refill_oct/)<span style="text-decoration:underline;"> \
Description: _</span>creates a new transaction with [Refill type](#Types-of-transactions).  Transaction is created using the same billing info, which was used during the first [Auth](#auth-method) request, when customer specified his billing information. 

Http- Http-method: _GET or_ POST \
Encoding: UTF-8

<p style="text-align: right">
<em>Refill_OCT method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Card number. Mandatory
   </td>
   <td>13–19 digits
   </td>
  </tr>
  <tr>
   <td>CardExpDate
   </td>
   <td>Card expiration date. Mandatory
   </td>
   <td>String in format MMYY—2 digits of the month and 2 of the year, for example 0915
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Payment amount. Mandatory
   </td>
   <td>Fixed point number (Decimal), two digits after the point, i.e.: 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Payment currency. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public key, confirming in tegrity of parameters of the request. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>PaymentInformationId
   </td>
   <td>Unique identifier of personal data. Mandatory is specified individually.
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>Email address of the payer. Mandatory
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>CardHolderName
   </td>
   <td>Name as printed on the card. Mandatory
   </td>
   <td>String, 100 letters max  \
for example John Smith
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format. \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”.

The argument to the function, which calculates the hash, is described as:


```
MerchantId={MerchantId}&CardNumber={CardNumber}&CardExpDate={CardExpDate}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&Operation=Refill_OCT&PrivateSecurityKey={PrivateSecurityKey}
```


Method returns the following data in response to request made with correct parameter values:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Operation name, Refill. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Performance results. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Result code. Mandatory
   </td>
   <td>Numeric
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Transaction status. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Error Code. Only in case if payment is not performed.
   </td>
   <td>Numeric
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok—payment is performed, \
Error—payment is not performed.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Pending—payment is performed, \
Declined—payment is not performed.
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>1—a technical problem, try again after some time, \
2—transaction is rejected by filters, repeat after 24 HOURS, the number of retries is limited by individual settings. Afterwards, the payer should re-passing through the form, \
3—transaction is rejected by the issuing bank. Possible repeated attempts are up to five times per day for 3 days, \
4—transaction is rejected by the issuing bank. Requires to pass 3DS.
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
Id={Id}&Operation=Refill_OCT&Result={Result}&Status={Status}&Code={Code}&ErrorCode={ErrorCode}
```


In case of XML:


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


## **Void method**

Endpoint url: [https://secure.payonlinesystem.com/payment/transaction/void/](https://secure.payonlinesystem.com/payment/transaction/void/) \
Description: _Cancels transaction in [Pending](#Transition-statuses) or [PreAuthorized](#Transition-statuses) status or a previously authorized transaction.  \
Http-method: POST or GET \
Encoding: UTF-8

<p style="text-align: right">
<em>Void method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public key, confirming integrity of parameters of the order. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format, \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”.

The argument to the function, which calculates the hash, is described as:


```
MerchantId={MerchantId}&TransactionId={TransactionId}&PrivateSecurityKey={PrivateSecurityKey}
```


Method returns the following data in response to request made with correct parameter values:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Name of the operation, Void. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Result of the action. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Message: in case of successful cancellation—Voided, in case of not successful one—error description. \
Mandatory
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok—transaction is cancelled \
Error—transaction is not cancelled
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
TransactionId={TransactionId}&Operation=Void&Result={Result}&Message={Message}
```


In case of XML:


```
<transaction>
	<id>{id}</id>
	<operation>Void</operation>
	<result>{result}</result>
	<message>{message}</message>
</transaction>
```


## **Refund method**

Endpoint url: [https://secure.payonlinesystem.com/payment/transaction/refund/](https://secure.payonlinesystem.com/payment/transaction/refund/) \
Description: _Creates a new transaction with type [Refund](#Refund-method) for transferring money back to the customer account. Valid for transactions in status [Settled](#Transition-statuses). \
Http-method: GET or POST \
Encoding: UTF-8

<p style="text-align: right">
<em>Refund method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Refund amount in currency of the original transaction. Full or partial refund is possible, that is the stated amount should be less or equal to the amount of the original transaction. Mandatory
   </td>
   <td>Fixed-point number (Decimal), two digits after point, e.g. 1500.99
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public security key, confirming integrity of request parameters. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format. \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”. 

The argument to the function, which calculates the hash, is described as:


```
MerchantId={MerchantId}&TransactionId={TransactionId}&Amount={Amount}&PrivateSecurityKey={PrivateSecurityKey}
```


Method returns the following data in response to request made with correct parameter values:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Name of the operation, Refund. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Amount of refund. Mandatory
   </td>
   <td>Fixed-point number (Decimal), two digits after point, e.g. 1500.99
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Result of the action. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Message: in case of success—Refunded, in case of failure—error description. Mandatory
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok—refund is issued,
<p>
Error—refund is not issued.
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
TransactionId={TransactionId}&Operation=Refund&Amount={Amount}&Result={Result}&Message={Message}
```


In case of XML:


```
<transaction>
	<id>{id}</id>
	<operation>Refund</operation>
	<amount>{amount}</amount>
	<result>{result}</result>
	<message>{message}</message>
</transaction>
```


## **Check method**

Endpoint URL: [https://secure.payonlinesystem.com/payment/transaction/check /](https://secure.payonlinesystem.com/payment/transaction/check%20/) \
Description:_ Method checks information about payer’s card and the payment in PayOnline AntiFraud system. \
Http-method: POST \
Encoding: UTF-8

<p style="text-align: right">
<em>Check method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String, max length—50 chars
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Total amount of the order. Mandatory
   </td>
   <td>Fixed-point number (Decimal), two digits after point, e.g. 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency of the order. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public security key, confirming integrity of request parameters. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>Ip
   </td>
   <td>Payer IPaddress. Mandatory, but specified individually.
   </td>
   <td>String in format, for example 66.11.130.105
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>Email address of the payer. Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>CardHolderName
   </td>
   <td>Name as printed on the card. \
Mandatory
   </td>
   <td>String, Latin characters only, max length—100 symbols, for example John Smith
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Card number. Mandatory
   </td>
   <td>13–19 digits
   </td>
  </tr>
  <tr>
   <td>CardExpDate
   </td>
   <td>Card expiration date. Mandatory
   </td>
   <td>String in format MMYY—2 digits of the month and 2 of the year, for example 0915
   </td>
  </tr>
  <tr>
   <td>CardCvv
   </td>
   <td>Card verification value—last 3 digits on reverse side of the card(after the signature). Mandatory
   </td>
   <td>3 digits
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Customer’s residence country. Mandatory, but specified individually.
   </td>
   <td>2 characters according to <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO3166</a> standard, for example RU.
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>Customer’s city. Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Address
   </td>
   <td>Customer’s address. Mandatory, but specified individually.
   </td>
   <td>String, 150 characters max
   </td>
  </tr>
  <tr>
   <td>Zip
   </td>
   <td>Customer’s ZIP code. Mandatory, but specified individually.
   </td>
   <td>String, 10 characters max
   </td>
  </tr>
  <tr>
   <td>State
   </td>
   <td>State for US or Canada citizens. Mandatory, but specified individually.
   </td>
   <td>String, 2 characters, for example FL
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Customer's phone number(international format). Mandatory, but specified individually.
   </td>
   <td>String, 50 characters max
   </td>
  </tr>
  <tr>
   <td>Issuer
   </td>
   <td>Issuer name. Mandatory, but specified individually.
   </td>
   <td>String, 100 characters max
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Any number of Custom parameters
   </td>
   <td>Any number of custom parameters
   </td>
   <td>Any format
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format, \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”. \
The argument to the function, which calculates the hash, is described as:


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```


Method returns the following data in response to request made with correct parameter values:

In case an error in format appeared:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Error code. Mandatory
   </td>
   <td>Integer
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Error description. Mandatory
   </td>
   <td>String
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
Code={Code}&Message={Message}
```


In case of XML:


```
<error>
	<code>{Code}</code>
	<message>{Message}</message>
</error>
```


In case an error in format did NOT appear:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Transaction identifier, Check. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Operation result. Mandatory
   </td>
   <td>It shows an array of results generated by Antifraud filters that perform payment check . Description of all filters and system settings for the merchant are made individually on request.
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
Id={Id}&Operation=Check&Result={Result}
```


In this case, array does not return results, “Result”—is a general test result: “OK” on success, “Fail” in the case of rejection by one of the filters.

In case of XML:


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
			Limit from one IP exceeded for 2:00 hours (192.168.1.1)
				</description>
			</filter>
	</result>
</transaction>>
```


## **Search method**

Endpoint url: [https://secure.payonlinesystem.com/payment/search/](https://secure.payonlinesystem.com/payment/search/) \
Description: returns information only on successful payments. The search can be based on OrderId or TransactionId parameter. \
Http-method: GET or POST \
Encoding: UTF-8

<p style="text-align: right">
<em>Search method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>OrderId 
   </td>
   <td>Identification code of the order in the merchant system. It is a mandatory parameter, if TransactionId is not stated.
   </td>
   <td>Line, max length—50 chars.
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Identification code of the transaction. Mandatory if OrderId is not stated.
   </td>
   <td>Integer number(Long)
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public security key, confirming integrity of request parameters. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format. \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”.

The argument to the function, which calculates the hash, is described as:


```
MerchantId={MerchantId}&OrderId={OrderId}&PrivateSecurityKey={PrivateSecurityKey}
```


In case of transaction search by OrderId or


```
MerchantId={MerchantId}&TransactionId={TransactionId}&PrivateSecurityKey={PrivateSecurityKey}
```


In case of transaction search by TransactionId.

Method returns the following data in response to request made with correct parameter values and after payment completion:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>TransactionId
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Amount of the payment. Mandatory
   </td>
   <td>Fixed-point number (Decimal), \
two digits after point, e.g. 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency of the payment. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>Line, max length—50 signs.
   </td>
  </tr>
  <tr>
   <td>DateTime
   </td>
   <td>Date and time of  transaction processing, Universal Coordinated Time (UTC). Mandatory
   </td>
   <td>Date and time “yyyy–MM–dd
<p>
HH<span>:</span>mm:ss”, for example
<p>
2008–12–31 23<span>:</span>59:59
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Status of the transaction. Mandatory
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>PreAuthorized, \
Pending, \
Settled.
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
TransactionId={TransactionId}&Amount={Amount}&Currency={Currency}&Order={OrderId}&DateTime={DateTime}&Status={Status}
```


In case of XML:


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


If payment is not found, then empty string is returned. 

## **List method**

Endpoint url: [https://secure.payonlinesystem.com/payment/transaction/list/](https://secure.payonlinesystem.com/payment/transaction/list/) \
Description_: returns the list of all transactions for the stated period of time in UTC+0 time zone. If parameters DateFrom and DateTill are specified in yyyy–MM–dd format, then transactions are returned from the start date 00<span>:</span>00:00 to the end date 23<span>:</span>59:59. \
Http-method: GET or POST \
Encoding_: UTF-8

<p style="text-align: right">
<em>List method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>DateFrom 
   </td>
   <td>Beginning of the period. Mandatory
   </td>
   <td>Date in format yyyy–MM–dd or yyyy–MM–dd HH<span>:</span>mm:ss
   </td>
  </tr>
  <tr>
   <td>DateTill
   </td>
   <td>End of the period. Not more than three days from the beginning of the period. Mandatory
   </td>
   <td>Date in format yyyy–MM–dd or yyyy–MM–dd HH<span>:</span>mm:ss
   </td>
  </tr>
  <tr>
   <td>Type
   </td>
   <td>Type of  transactions. Several transaction types can be specified, separated by comma. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Status of  transactions. Several transaction statuses can be specified, separated by comma.  Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public key, confirming integrity of parameters of the order.
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ExtendedData
   </td>
   <td>Displaying additional information (bank gateway authorization code and mask 6x4 of the card number). Optional
   </td>
   <td>Integer number. Valid value—1. When in any other values in the response comes error.
   </td>
  </tr>
</table>


Possible options for enumerations:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Type
   </td>
   <td>Purchase—transaction for charging, \
Refund—transaction for refunding,
<p>
Refill—transaction for money transfer from merchants account to customers account,
<p>
Arbitrary Purchase—payment by  bank requisites,
<p>
Card to Card Transfer—transaction for money transfer from one card account to another.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>PreAuthorized, \
Pending, \
Settled, \
Voided, \
Declined.
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Text—result in text format, \
xml—result in format XML.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”. 

The argument to the function, which calculates the hash, is described as:


```
MerchantId={MerchantId}&DateFrom ={DateFrom }&DateTill={DateTill}&Type={Type}&Status={Status}&PrivateSecurityKey={PrivateSecurityKey}
```


The method returns all transactions made for the period from the start date 00<span>:</span>00:00.0000 to the end date 23<span>:</span>59:59.9999 in UTC time zone.

Method returns the following data in response to request made with correct parameter values:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>Date
   </td>
   <td>Date and time of the transaction. Mandatory
   </td>
   <td>Date and time in format “yyyy–MM–dd HH<span>:</span>mm:ss”.
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Type
   </td>
   <td>Transaction type. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Transaction status. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Concealed card number. Optional
   </td>
   <td>String, 16 chars.
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Amount. Mandatory
   </td>
   <td>Fixed-point number (Decimal), two digits after point, e.g. 120.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>Gateway
   </td>
   <td>Gateway type. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>CardHolder
   </td>
   <td>Card holder name. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Phone
   </td>
   <td>Phone number of the payer. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Email
   </td>
   <td>E-mail of the payer. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>City
   </td>
   <td>City of the payer. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>CountryCode
   </td>
   <td>Country code of the payer according to <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>. Optional
   </td>
   <td>String, two chars, for example US
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Country code, determined according to BIN of the card issuer. Optional
   </td>
   <td>String, two chars, for example US
   </td>
  </tr>
  <tr>
   <td>IpAddress
   </td>
   <td>IP-address of the payer. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Country code, determined according to IP-address. Optional
   </td>
   <td>String, two chars, for example US
   </td>
  </tr>
  <tr>
   <td>AuthCode
   </td>
   <td>Bank gateway authorization code. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Card6x4
   </td>
   <td>Mask of the card number. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>PresentmentDate
   </td>
   <td>Date of transaction settlement by the bank to the merchant bank account. Optional
   </td>
   <td>Date in format  \
YYYY-MM-DD
   </td>
  </tr>
  <tr>
   <td>AccountID
   </td>
   <td>Client’s account identification code in the merchant system.
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>GatewayTransactioID
   </td>
   <td>Transaction identification code on the gateway side
   </td>
   <td>String 
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Type
   </td>
   <td>Purchase—transaction for charging, \
Refund—transaction for refunding,
<p>
Refill—transaction for money transfer from merchants account to customers account,
<p>
Arbitrary Purchase—payment by  bank requisites,
<p>
Card to Card Transfer—transaction for money transfer from one card account to another.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Pending—transaction is being processed, \
Settled—transaction is accomplished, \
Voided—transaction is cancelled, \
Declined—transaction is declined.
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>Gateway
   </td>
   <td>Test—test gateway, \
Real—“live” gateway.
   </td>
  </tr>
</table>


If input parameter Content Type = text, the transaction will be uploaded in CSV format. First line indicates table headers.

If ContentType = xml, then the transaction will be uploaded in XML format, name of the root element—list, the elements content:


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
       <accountId>RU</accountId>
	<gatewayTransactionIdRU</gatewayTransactionId>


</transaction>
```


## **Card2Card method**

Endpoint url: [https://secure.payonlinesystem.com/payment/transaction/card2card/](https://secure.payonlinesystem.com/payment/transaction/card2card/) \
Description_: Creates new transfer from card to card. Transaction is created using the same billing info, which was used during the first <a href="#/en/api?id=auth-method">Auth</a> request, when customer specified his billing information. \
Http-method: GET or POST \
Encoding_: UTF-8

<p style="text-align: right">
<em>Card2Card method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Rebill transaction key, received in parameter RebillAnchor in <a href="#/en/api?id=auth-method">Auth</a> call. Mandatory
   </td>
   <td>String 
   </td>
  </tr>
  <tr>
   <td>RecipientRebillAnchor
   </td>
   <td>Rebill transaction key, received in parameter RebillAnchor in 

<p id="gdcalert23" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: undefined internal link (link text: "Auth method"). Did you generate a TOC? </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert24">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>

<a href="#heading=h.17dp8vu">Auth method</a> call. Mandatory
   </td>
   <td>String 
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Amount to be transferred to the card. Mandatory
   </td>
   <td>Numeric Fixed point number (Decimal), two digits after the point, i.e.: 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Amount currency and commission. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>Commission
   </td>
   <td>Commission fee for the transfer. Mandatory
   </td>
   <td>Numeric Fixed point number (Decimal), two digits after the point, i.e.: 1500.99
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public key, confirming integrity of parameters of the order. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format, \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”. The argument to the function, which calculates the hash, is described as:


```
MerchantId={MerchantId}&RebillAnchor={RebillAnchor}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&Commission={Commission}&RecipientRebillAnchor={RecipientRebillAnchor}&Operation=Card2Card&PrivateSecurityKey={PrivateSecurityKey}
```


Method returns the following data in response to request made with correct parameter values:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Operation
   </td>
   <td>Opeation name, Card2Card. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Operation result. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Code
   </td>
   <td>Result code. Mandatory
   </td>
   <td>Numeric
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Transaction status. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ErrorCode
   </td>
   <td>Error Code. Only in case if payment is not performed.
   </td>
   <td>Numeric
   </td>
  </tr>
  <tr>
   <td>Pareq
   </td>
   <td>3DS parameter (see method description 3DS)
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Acsurl
   </td>
   <td>3DS parameter
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Pd
   </td>
   <td>3DS parameter
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok—payment is performed, \
Error—payment is not performed.
   </td>
  </tr>
  <tr>
   <td>Status
   </td>
   <td>Pending—payment is performed, \
Declined—payment is not performed, \
Awaiting3Dauthentication—need to pass 3DS.
   </td>
  </tr>
</table>





<table>
  <tr>
   <td>ErrorCode
   </td>
   <td>1—technical problem, try again in 10 minutes, \
2—transaction is rejected by filters, repeat after 24 HOURS, the number of retries is limited by 5 times. Afterwards, the payer should re-pass through the form,
<p>
3—transaction is rejected by the issuing bank. Possible repeated attempts are upto five times per day for 3 days, \
4—transaction is rejected by the issuing bank. Requires to pass 3DS or  stop further Rebill operations with RebillAnchor data.
   </td>
  </tr>
</table>


In case input parameter ContentType = text, the result will look like:


```
TransactionId={TransactionId}&Operation=Refill&Result={Result}&Status={Status}&Code={Code}&ErrorCode={ErrorCode}
```


In case of XML:


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


## **Card2Card 3DS Method**

Endpoint url: [https://secure.payonlinesystem.com/payment/transaction/card2card/3ds/](https://secure.payonlinesystem.com/payment/transaction/card2card/3ds/) \
Description: _Creates the transfer from one card to another after payer 3DS authorization. It is used to complete the card transfer, signedfor  3 D Secure.  \
This method must be used if the [Card2Card](#Card2Card-method) returned an error 6001 and passed parameters PaReq, ASCUrl and PD.

The process of making a transfer is similar to the process of making payment by card, which was signed for 3D Secure (see 

[3DS](#3DS-method).

<p style="text-align: right">
<em>Card2Card 3DS method parameters</em></p>



<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>MerchantId
   </td>
   <td>Identification code, assigned by PayOnline. Mandatory
   </td>
   <td>Integer number
   </td>
  </tr>
  <tr>
   <td>PARes
   </td>
   <td>Value received by TermUrl web page from the bank issuer. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>PD
   </td>
   <td>The value obtained by calling <a href="#/en/api?id=auth-method">Auth</a> method with error code 6001.
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>Result format—text or xml. Default is text. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Any number of custom parameters
   </td>
   <td>Any number of custom parameters. Optional
   </td>
   <td>Any format
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>ContentType
   </td>
   <td>text—result in text format, \
xml—result in XML format.
   </td>
  </tr>
</table>


SecurityKey parameter is not used.

Method returns the following data in response to request made with correct parameter values:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>Id
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Transaction result. Mandatory
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>Message
   </td>
   <td>Message. Mandatory
   </td>
   <td>String
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Result
   </td>
   <td>Ok—payment is performed, \
Error—payment is not performed.
   </td>
  </tr>
</table>


# **CallbackURL**

CallBackUrl is the merchant defined URL, where he receives information about successful or unsuccessful payments after [Auth](#auth-method), [ApplePay](#ApplePay-method) or [Rebill](#rebill-method) method is called. CallBack parameters, such as URL for successful payments and URL for unsuccessful payments, method and coding are set in the Merchant account.The following parameters are passed to CallbackUrl:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
  </tr>
  <tr>
   <td>DateTime
   </td>
   <td>Date and time of the payment, time zone GMT+0 (UTC). Mandatory
   </td>
   <td>Date and time “yyyy–MM–dd HH<span>:</span>mm:ss”, for example 2008–12–31 23<span>:</span>59:59
   </td>
  </tr>
  <tr>
   <td>TransactionID
   </td>
   <td>Identification code of the transaction. Mandatory
   </td>
   <td>Integer number (Long)
   </td>
  </tr>
  <tr>
   <td>OrderId
   </td>
   <td>Identification code of the order in the merchant system. Mandatory
   </td>
   <td>String, max length—50 chars
   </td>
  </tr>
  <tr>
   <td>Amount
   </td>
   <td>Amount of the transaction (payment). Mandatory
   </td>
   <td>Fixed-point number (Decimal), two digits after point, e.g. 1500.99
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency of the order. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>SecurityKey
   </td>
   <td>Public security key, confirming integrity of request parameters. Mandatory
   </td>
   <td>String, 32 chars lowercase
   </td>
  </tr>
  <tr>
   <td>PaymentAmount
   </td>
   <td>Amount to be transferred to the card in the currency of the payment. Mandatory
   </td>
   <td>Numeric Fixed point number (Decimal), two digits after the point, i.e.: 1500.99
   </td>
  </tr>
  <tr>
   <td>PaymentCurrency
   </td>
   <td>Payment currency. Mandatory
   </td>
   <td>String, 3 chars
   </td>
  </tr>
  <tr>
   <td>CardHolder
   </td>
   <td>Cardholder name. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>CardNumber
   </td>
   <td>Concealed card number—12 chars ‘*’ and the last 4 numbers. Optional
   </td>
   <td>String, 13–16 chars
   </td>
  </tr>
  <tr>
   <td>Country
   </td>
   <td>Country code according to <a href="http://www.iso.org/iso/home/standards/country_codes.htm">ISO 3166</a>. Optional
   </td>
   <td>String, 2 chars
   </td>
  </tr>
  <tr>
   <td>
    City
   </td>
   <td>City. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>
    Address
   </td>
   <td>Address. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>IpAddress
   </td>
   <td>IP-address of the payer. Optional
   </td>
   <td>String
   </td>
  </tr>
  <tr>
   <td>IpCountry
   </td>
   <td>Country code, determined according to IP-address. Optional
   </td>
   <td>String (two chars)
   </td>
  </tr>
  <tr>
   <td>BinCountry
   </td>
   <td>Country code, determined according to BIN of the card issuer. Optional
   </td>
   <td>String (two chars)
   </td>
  </tr>
  <tr>
   <td>SpecialConditions
   </td>
   <td>Special conditions.Optional
   </td>
   <td>String, one or sevral values, separated by commas: ValidationRequired—payer verification recomended.
   </td>
  </tr>
  <tr>
   <td>RebillAnchor
   </td>
   <td>Token for repeated payments for this card. Only if the payment was completed and merchant is signed for the recurring payments service. Optional
   </td>
   <td>String. Max length—100 chars
   </td>
  </tr>
  <tr>
   <td>All other parameters
   </td>
   <td>All other parameters, which were passed to the <a href="#/en/api?id=auth-method">Auth</a>, <a href="#/en/api?id=ApplePay-method">ApplePay</a> or <a href="#/en/api?id=rebill-method">Rebill</a> method.
   </td>
   <td>Any data type
   </td>
  </tr>
</table>


Possible options:


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Options</strong>
   </td>
  </tr>
  <tr>
   <td>Currency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
  <tr>
   <td>PaymentCurrency
   </td>
   <td>Currency code according to <a href="http://www.iso.org/iso/currency_codes">ISO 4217</a>
   </td>
  </tr>
</table>


SecurityKey should be formed according to the algorithm described in “[SecurityKey parameter](#SecurityKey-parameter)”. \
The argument to the function, which calculates the hash, is described as:


```
DateTime={DateTime}&TransactionID={TransactionID}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&PrivateSecurityKey={PrivateSecurityKey}
```


# **SecurityKey parameter**

Parameter “SecurityKey” is calculated by the hash function **md5 **from the query line, supplemented with merchant's secrect key.

If query line looks like:


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&ValidUntil={ValidUntil}
```


then hash is calculated from the string:


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&ValidUntil={ValidUntil}&PrivateSecurityKey={PrivateSecurityKey}
```


where {PrivateSecurityKey}—merchant's secret key.

Order of the parameters and case are important!  \
Parameter values should be substituted instead of expressions in braces.  \
Merchant receives MerchantId and PrivateSecurityKey at his account activation.

For example, for parameters: MerchantId = 12345, OrderId = 56789, Amount = 9.99, Currency = USD, ValidUntil =2010-01-29 16<span>:</span>10:00, PrivateSecurityKey = 3844908d-4c2a-42e1-9be0-91bb5d068d22 hash is calculated from the line:


```
MerchantId=12345&OrderId=56789&Amount=9.99&Currency=USD&ValidUntil=2010-01-29 16:10:00&PrivateSecurityKey=3844908d-4c2a-42e1-9be0-91bb5d068d22
```


and will be equal to d31abce556db686f4b097392a00d7a27


    <p style="text-align: right">
<strong><em>Note</em></strong></p>



    <p style="text-align: right">
<strong><em>PaymentKey can be used instead of PrivateSecurityKey key to calculate the Security Key parameter when forming the request for [Auth](#auth-method) or [ApplePay](#ApplePay-method) method.</em></strong></p>


When using Payment Key, if the query string looks like:


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&ValidUntil={ValidUntil}
```


then hash is calculated from the string:


```
MerchantId={MerchantId}&OrderId={OrderId}&Amount={Amount}&Currency={Currency}&ValidUntil={ValidUntil}&PaymentKey={PaymentKey}
```


where {PaymentKey} —PaymentKey key.

Merchant receives PaymentKey parameter at his account activation.

# **Response codes**

Possible values of “Code” parameter received as the result of [Auth](#auth-method), [ApplePay](#ApplePay-method) or [Rebill](#rebill-method) methods: \
Code 200 — operation completed successfully. \
Code 1*** — technical failure of the system. \
Code 2*** — operation is blocked by the security system. \
Code 3*** — a technical error on the bank side. \
Code 6*** — requires additional authentication.


<table>
  <tr>
   <td><strong>Code</strong>
   </td>
   <td><strong>Value</strong>
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
   <td>Incorrect card verification code
   </td>
  </tr>
  <tr>
   <td>4007
   </td>
   <td>Incorrect card expiration date
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
   <td>Incorrect email
   </td>
  </tr>
  <tr>
   <td>4018
   </td>
   <td>Incorrect ipaddress
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
   <td>Incorrect transaction ID
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
   <td>Unable to process
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
   <td>5205
   </td>
   <td>Insufficient funds
   </td>
  </tr>
  <tr>
   <td>5301
   </td>
   <td>Card expired or incorrect date
   </td>
  </tr>
  <tr>
   <td>5302
   </td>
   <td>Declined by issuer
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
   <td>Financial institution prohibited
   </td>
  </tr>
  <tr>
   <td>5305
   </td>
   <td>Lost or stolen card
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
   <td>Unable to authorize
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
   <td>Format error
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
   <td>Declined by issuer—must eliminate
   </td>
  </tr>
  <tr>
   <td>5809
   </td>
   <td>Gateway server error
   </td>
  </tr>
</table>
