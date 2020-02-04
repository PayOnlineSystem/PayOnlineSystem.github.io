# Описание

### <span>Google Pay&trade;</span> это быстрый и безопасный метод оплаты в «один клик». 
Используя Google Pay API покупатель может оплачивать с помощью:

1) карты привязанной к его личному Google Pay аккаунту (Токенизированная карта). В этом случае При нажатии на кнопку Google Pay генерируется токен, который потом надо передать в метод Auth. И на этом оплата завершается.

2) непривязанной карты. (Нетокенизированная карта). В этом случае вы должны получить токен,  передать его в метод Auth, и пройти процедуру 3ds аутентификации.

 
# Условия для интеграции:

* ваш сайт должен работать по схеме HTTPS и иметь валидный TLS сертификат для домена. 
* вы должны соблюдать [Правила использования Google Pay API](https://payments.developers.google.com/terms/aup)
* вы должны принять [Условия использования Google Pay API](https://payments.developers.google.com/terms/sellertos)

# Шаги интеграции

## Web.

### Доступные методы авторизации
"PAN_ONLY" и "CRYPTOGRAM_3DS"
```javascript
    const allowedCardAuthMethods = ["PAN_ONLY", "CRYPTOGRAM_3DS"];
```

### Доступные платёжные системы
Visa и MasterCard
```javascript
    const allowedCardNetworks = ["MASTERCARD", "VISA"];
```

### Параметры которые требуется указать, чтобы использовать PayOnline в качестве шлюза
* gateway: 'payonline'
* gatewayMerchantId: '123' - your MID in PayOnline system

### BillingAddress и ShippingAddress
Никаких дополнительных BillingAddress или ShippingAddress параметров не требуется

### Как отправить зашифрованные данные в PayOnline
Используйте <a href="#/en/api?id=googlepay-method">GooglePay</a> метод из PayOnline API\
Вы должны передать зашифрованные данные в параметре запроса "PaymentToken".

### Все шаги вместе
1. Добавить кнопку на страницу
2. Получить токен (нажав на кнопку Google Pay)
3. Передать токен вам на бэкенд
4. Вызвать на бэкенде <a href="#/en/api?id=googlepay-method">GooglePay</a> метод из PayOnline API
5. Если получите в ответе Awaiting3DS, тогда вам надо пройти дополнительную  3ds аутентификацию и вызвать метод <a href="#/en/api?id=complete-method">Complete</a>

### Пример кода
Пример показывает 1, 2 и 3 пункты [пошаговой инструкции](#все-шаги-вместе) (показывет как инициализировать кнопку Google Pay button с параметрами для PayOnline, получить токен и передать его на бэкенд). 
```javascript

    const baseRequest = {
        apiVersion: 2,
        apiVersionMinor: 0
    };

    const allowedCardNetworks = ["MASTERCARD", "VISA"];
    const allowedCardAuthMethods = ["PAN_ONLY", "CRYPTOGRAM_3DS"];
    const tokenizationSpecification = {
        type: 'PAYMENT_GATEWAY',
        parameters: {
          'gateway': 'payonline',
          'gatewayMerchantId': '123'//your MID in PayOnline system
        }
      };
    const baseCardPaymentMethod = {
        type: 'CARD',
        parameters: {
            allowedAuthMethods: allowedCardAuthMethods,
            allowedCardNetworks: allowedCardNetworks
        }
    };
    const cardPaymentMethod = Object.assign(
        {},
        baseCardPaymentMethod,
        {
            tokenizationSpecification: tokenizationSpecification
        }
    );
    let paymentsClient = null;
    function getGoogleIsReadyToPayRequest() {
        return Object.assign(
            {},
            baseRequest,
            {
                allowedPaymentMethods: [baseCardPaymentMethod]
            }
        );
    }
    function getGooglePaymentDataRequest() {
        const paymentDataRequest = Object.assign({}, baseRequest);
        paymentDataRequest.allowedPaymentMethods = [cardPaymentMethod];
        paymentDataRequest.transactionInfo = getGoogleTransactionInfo();
        paymentDataRequest.merchantInfo = {
            merchantName: 'Example Merchant'
        };
        return paymentDataRequest;
    }

    function getGooglePaymentsClient() {
        if (paymentsClient === null) {
            paymentsClient = new google.payments.api.PaymentsClient({ environment: 'PRODUCTION' });
        }
        return paymentsClient;
    }
    
    //Google Pay loaded callback
    function onGooglePayLoaded() {
        const paymentsClient = getGooglePaymentsClient();
        paymentsClient.isReadyToPay(getGoogleIsReadyToPayRequest())
            .then(function (response) {
                if (response.result) {
                    addGooglePayButton();
                }
            })
            .catch(function (err) {
                console.error(err);
            });
    }

    function addGooglePayButton() {
        const paymentsClient = getGooglePaymentsClient();
        const button =
            paymentsClient.createButton({
                onClick: onGooglePaymentButtonClicked,
                buttonColor: 'black',
                buttonType: 'long'
            });
        $("#gpay-container")[0].appendChild(button);
    }

    //getting price
    function getGoogleTransactionInfo() {
        return {
            currencyCode: 'RUB',
            totalPriceStatus: 'FINAL',
            totalPrice: '1.00'
        };
    }

    //Googe Pay button handler
    function onGooglePaymentButtonClicked() {
        const paymentDataRequest = getGooglePaymentDataRequest();
        paymentDataRequest.transactionInfo = getGoogleTransactionInfo();

        const paymentsClient = getGooglePaymentsClient();
        paymentsClient.loadPaymentData(paymentDataRequest)
            .then(function (paymentData) {
                processPayment(paymentData);
            })
            .catch(function (err) {
                console.error(err);
            });
    }

    function processPayment(paymentData) {
        var token = JSON.stringify(paymentData);
        //pass token to your backend, then call method googlePay of PayOnline API
        $.post("/GooglePay", token).then(function (result) {
           if (result.Success) {
               //payment successfully completed
           } else {
               if (result.AwaitingThreeDS) { 
                   //3ds authentication required
               } else {
                   //payment declined
               }
           }
        });
    }
```
В конце страницы
```javascript
<script async src="https://pay.google.com/gp/p/js/pay.js" onload="onGooglePayLoaded()"></script>
````


### При интеграции следуйте правилам:

* [Google Pay Web developer documentation](https://developers.google.com/pay/api/web/overview)

* [Google Pay Web integration checklist](https://developers.google.com/pay/api/web/guides/test-and-deploy/integration-checklist)

* [Google Pay Web Brand Guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines)

## Android.

### Доступные методы авторизации
"PAN_ONLY" и "CRYPTOGRAM_3DS"
```kotlin
    val SUPPORTED_METHODS = listOf(
            "PAN_ONLY",
            "CRYPTOGRAM_3DS")
```

### Доступные платёжные системы
Visa и MasterCard
```kotlin
val SUPPORTED_NETWORKS = listOf(
        "MASTERCARD",
        "VISA")
```

### Параметры которые требуется указать, чтобы использовать PayOnline в качестве шлюза
* gateway: 'payonline'
* gatewayMerchantId: '123' - your MID in PayOnline system
```kotlin
val PAYMENT_GATEWAY_TOKENIZATION_PARAMETERS = mapOf(
        "gateway" to "payonline",
        "gatewayMerchantId" to "123" //Your MID in PayOnline system
    )
```

### BillingAddress и ShippingAddress
Никаких дополнительных BillingAddress или ShippingAddress параметров не требуется

### Как отправить зашифрованные данные в PayOnline
Используйте <a href="#/en/api?id=googlepay-method">GooglePay</a> метод из PayOnline API\
Вы должны передать зашифрованные данные в параметре запроса "PaymentToken".\
Для работы с PayOnline API в android вы можете использовать [PayOnline SDK for Android](https://github.com/PayOnlineSystem/PayOnline.SDK.Android)

### Все шаги вместе
1. Добавить кнопку на view
2. Нажать кнопку, чтобы получить токен
3. Вызвать <a href="#/en/api?id=googlepay-method">GooglePay</a> метод из PayOnline API
4. Если вы получите в ответе Awaiting3DS, тогда вам надо пройти дополнительную 3ds аутентификацию и вызвать метод <a href="#/en/api?id=complete-method">Complete</a>

Для работы с PayOnline API вы можете использовать [PayOnline SDK for Android](https://github.com/PayOnlineSystem/PayOnline.SDK.Android)

### Пример кода
Полный пример вы можете найти в нашем репозитории [PayOnline.AndroidSample](https://github.com/PayOnlineSystem/PayOnline.AndroidSample).\
Так-же вы можете просмотреть [Пример](https://github.com/google-pay/android-quickstart) использования Google Pay API от Google.




### При интеграции следуйте правилам

* [Google Pay Android developer documentation](https://developers.google.com/pay/api/android/overview)

* [Google Pay Android integration checklist](https://developers.google.com/pay/api/android/guides/test-and-deploy/integration-checklist).

* [Google Pay Android brand guidelines](https://developers.google.com/pay/api/android/guides/brand-guidelines).

## 2. Получить одобрение и зарегистрироваться в Google.

Для этого вам надо заполнить [форму](https://services.google.com/fb/forms/googlepayAPIenable). После этого с вами свяжется представитель Google и проинструктирует о дальнейших шагах. Будьте готовы отправить в Google ссылку на вашу интеграцию или apk файл для оценки.
