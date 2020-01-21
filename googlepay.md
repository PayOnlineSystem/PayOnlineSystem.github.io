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

## 1. Добавить кнопку Google Pay

### Web.

Краткая пошаговая инструкция:
* Получить токен с помощью нажатия на кнопку Google Pay
* Передать токен на ваш бэкенд
* Вызвать <a href="#/en/api?id=googlepay-method">GooglePay</a> метод из PayOnline API
* Если вы получили код Awaiting3DS, тогда вам надо пройти дополнительную 3ds аутентификацию, и вызвать <a href="#/en/api?id=complete-method">Complete</a> метод для завершения платежа

Покупателям будут доступны платёжные системы:
* Visa 
* MasterCard

Параметры, которые необходимо указать при проведении транзакции через шлюз PayOnline
* gateway: 'payonline'
* gatewayMerchantId: '123' - ваш MID в системе PayOnline

Пример на javascript показывает, как инициализировать кнопку Google Pay с параметрами для шлюза PayOnline
```javascript
const allowedCardNetworks = ["MASTERCARD", "VISA"];
    const allowedCardAuthMethods = ["PAN_ONLY", "CRYPTOGRAM_3DS"];
    const tokenizationSpecification = {
        type: 'PAYMENT_GATEWAY',
        parameters: {
          'gateway': 'payonline',
          'gatewayMerchantId': '123'//ваш MID в системе PayOnline
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

    // Информация, полученная от google
    function getGooglePaymentDataRequest() {
        const paymentDataRequest = Object.assign({}, baseRequest);
        paymentDataRequest.allowedPaymentMethods = [cardPaymentMethod];
        paymentDataRequest.transactionInfo = getGoogleTransactionInfo();
        paymentDataRequest.merchantInfo = {
            merchantName: 'Example Merchant',
            merchantId: "YourGoogleMerchantId"
        };
        return paymentDataRequest;
    }

    function getGooglePaymentsClient() {
        if (paymentsClient === null) {
            paymentsClient = new google.payments.api.PaymentsClient({ environment: 'PRODUCTION' });
        }
        return paymentsClient;
    }
    
    //Обработчик загрузки Google Pay
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

    //получение цены
    function getGoogleTransactionInfo() {
        return {
            currencyCode: 'RUB',
            totalPriceStatus: 'FINAL',
            totalPrice: '1.00'
        };
    }

    //обработчик нажатия кнопки G-Pay
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
        console.log(token);
        //отправка токена на бэкенд ...
        $.post("/GooglePay", token).then(function (result) {
        if (result.Success) {
            //оплата успешно завершена
        } else {
            if (result.AwaitingThreeDS) { 
                //требуется 3ds аутентификация
            } else {
                //оплата отклонена
            }
        }
    });
    }
```


При интеграции следуйте правилам:

* [Google Pay Web developer documentation](https://developers.google.com/pay/api/web/overview)

* [Google Pay Web integration checklist](https://developers.google.com/pay/api/web/guides/test-and-deploy/integration-checklist)

* [Google Pay Web Brand Guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines)

### Android.

Для работы с PayOnline API вам надо использовать [PayOnline SDK for Android](https://github.com/PayOnlineSystem/PayOnline.SDK.Android)

Краткая пошаговая инструкция:
* Нажать на кнопку Google Pay для получения токена
* Вызвать <a href="#/en/api?id=googlepay-method">GooglePay</a> метод из PayOnline API
* Если вы получили код Awaiting3DS, тогда вам надо пройти дополнительную 3ds аутентификацию, и вызвать <a href="#/en/api?id=complete-method">Complete</a> метод для завершения платежа

Покупателям будут доступны платёжные системы::
* Visa
* MasterCard

Чтобы провести транзакцию через шлюз PayOnline вам надо указать следующие параметры:
* gateway: 'payonline'
* gatewayMerchantId: '123' - ваш MID в системе PayOnline

```kotlin
val PAYMENT_GATEWAY_TOKENIZATION_PARAMETERS = mapOf(
        "gateway" to "payonline",
        "gatewayMerchantId" to "123" //ваш MID в системе PayOnline
    )
```

```kotlin
val SUPPORTED_NETWORKS = listOf(
        "MASTERCARD",
        "VISA")
```

Полный пример вы можете найти в нашем github репозитории [PayOnline.AndroidSample](https://github.com/PayOnlineSystem/PayOnline.AndroidSample).
Также можете посмотреть [пример](https://github.com/google-pay/android-quickstart) использования Google Pay API от Google.

При интеграции следуйте правилам:

* [Google Pay Android developer documentation](https://developers.google.com/pay/api/android/overview)

* [Google Pay Android integration checklist](https://developers.google.com/pay/api/android/guides/test-and-deploy/integration-checklist).

* [Google Pay Android brand guidelines](https://developers.google.com/pay/api/android/guides/brand-guidelines).

## 2. Получить одобрение и зарегистрироваться в Google.

Для этого вам надо заполнить [форму](https://services.google.com/fb/forms/googlepayAPIenable). После этого с вами свяжется представитель Google и проинструктирует о дальнейших шагах. Будьте готовы отправить в Google ссылку на вашу интеграцию или apk файл для оценки.
