# Описание

### <span>Google Pay &trade;</span> это быстрый и безопасный метод оплаты в «один клик». 
Используя Google Pay API покупатель может оплачивать с помощью:

1) карты привязанной к его личному Google Pay аккаунту (Токенизированная карта). В этом случае При нажатии на кнопку Google Pay генерируется токен, который потом надо передать в метод Auth. И на этом оплата завершается.

2) непривязанной карты. (Нетокенизированная карта). В этом случае вы должны получить токен,  передать его в метод Auth, и пройти процедуру 3ds аутентификации.

 
# Условия для интеграции:

* ваш сайт должен работать по схеме HTTPS и поддерживать протокол TLS версии 1.2. 
* домен сайта должен быть зарегистрирован и подтвержден в Google.

# Шаги интеграции

## 1. Добавить кнопку Google Pay

### Web.

Для интеграции Google Pay с вашим сайтом, необходимо добавить кнопку оплаты Google Pay (см. код ниже) и следовать правилам Google Pay.

* [Google Pay Web developer documentation](https://developers.google.com/pay/api/web/overview)

* [Google Pay Web integration checklist](https://developers.google.com/pay/api/web/guides/test-and-deploy/integration-checklist)

По нажатию необходимо получить токен, отправить его себе на бэкенд, и уже там вызвать API PayOnline.

При использовании Google Pay API для оплаты через PayOnline можно использовать платёжные системы:

* Visa 
* MasterCard.

При инициализации Google Pay API необходимо указать

* gateway: 'payonline'
* gatewayMerchantId: '123' - ваш MID в системе PayOnline

Пример кода на javascript
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

### Android.

Для интеграции Google Pay с вашим Android приложением также необходимо добавить кнопку google и следовать правилам:

* [Google Pay Android developer documentation](https://developers.google.com/pay/api/android/overview)

* [Google Pay Android integration checklist](https://developers.google.com/pay/api/android/guides/test-and-deploy/integration-checklist).

* [Google Pay Android brand guidelines](https://developers.google.com/pay/api/android/guides/brand-guidelines).

Код с реализованной кнопкой GooglePay:

* [Пример](https://github.com/PayOnlineSystem/PayOnline.AndroidSample) использования Google Pay API для оплаты через PayOnline

* [Пример](https://github.com/google-pay/android-quickstart) использования Google Pay API от Google.

В вашей реализации необходимо ввести следующие данные

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

Также необходимо имплементировать PayOnline API самостоятельно или использовать [SDK для Android](https://github.com/PayOnlineSystem/PayOnline.SDK.Android)

Логика работы та же, что и в web приложении
* Нажать на кнопку и получить токен
* Вызвать <a href="#/api?id=Метод-googlepay">Google Pay</a> метод
* Если пришел код ошибки Awaiting3DS, то необходимо пройти 3DS аутенфикацию (<a href="#/en/api?id=complete-method">Complete</a> method)


## 2. Получить одобрение и зарегистрироваться в Google.

Для этого надо заполнить [форму](https://services.google.com/fb/forms/googlepayAPIenable). После этого с вами свяжется представитель Google и проинструктирует о дальнейших шагах. Будьте готовы отправить в Google ссылку на вашу интеграцию или apk файл для оценки.


# Обязательные условия использования Google Pay
 
* Условия использования[ GOOGLE PAY API TERMS OF SERVICE](https://payments.developers.google.com/terms/sellertos)

* Список товаров и услуг, запрещённых к оплате через Google Pay:[ GOOGLE PAY APIs ACCEPTABLE USE POLICY](https://payments.developers.google.com/terms/aup)
