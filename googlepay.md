# Описание

GooglePay это быстрый и безопасный метод оплаты в «один клик». Используя GooglePay покупатель может оплачивать с помощью:

1)карты привязанной к его личному google-pay аккаунту (Токенизированная карта). В этом случае При нажатии на кнопку G-Pay генерируется токен, который потом надо передать в метод Auth. И на этом оплата завершается.

2)непривязанной карты. (Нетокенизированная карта). В этом случае вы должны получить токен,  передать его в метод Auth, и пройти процедуру 3ds аутентификации.

 

# Чтобы начать использовать Google Pay надо

## 1. Добавить кнопку Google Pay

### Web.

Вкратце: вы получаете токен и отправляете его себе на бэкенд используя javaScript. И уже на бэкенде вызывает API PayOnline.

При использовании Google Pay API для оплаты через PayOnline покупатели смогут использовать платёжные системы: Visa и MasterCard.

При инициализации Google Pay API вы должны указать

* gateway: 'payonline'
* gatewayMerchantId: ваш MID в системе PayOnline

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

При проведении интеграции Google Pay пожалуйста используйте: 

* [Google Pay Web developer documentation](https://developers.google.com/pay/api/web/overview), [Google Pay Web integration checklist](https://developers.google.com/pay/api/web/guides/test-and-deploy/integration-checklist).

* [ Google Pay Android brand guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines).

### Android.

Пример кода…

Также используйте в качестве примера:

* [Пример](https://github.com/PayOnlineSystem/PayOnline.AndroidSample) использования Google Pay API для оплаты через PayOnline (ссылка на гит-хаб)

* [Пример](https://github.com/google-pay/android-quickstart) использования Google Pay API от Google.

Когда будете имплементировать интеграцию, пожалуйста используйте:

* Руководство по интеграции от google:  [Google Pay Android developer documentation](https://developers.google.com/pay/api/android/overview), [Google Pay Android integration checklist](https://developers.google.com/pay/api/android/guides/test-and-deploy/integration-checklist).

* Правила использования бренда от Google, при добавлении кнопки:  [Google Pay Android brand guidelines](https://developers.google.com/pay/api/android/guides/brand-guidelines).

 

## 2. Получить одобрение и зарегистрироваться в Google.

Для этого надо заполнить [форму](https://services.google.com/fb/forms/googlepayAPIenable). После этого с вами свяжется представитель Google и проинструктирует о дальнейших шагах. Будьте готовы отправить в Google ссылку на вашу интеграцию или apk файл для оценки.

 

# Обязательные условия при успользовании Google Pay.

* Условия использования[ GOOGLE PAY API TERMS OF SERVICE](https://payments.developers.google.com/terms/sellertos)

* Список товаров и услуг, запрещённых к оплате через Google Pay:[ GOOGLE PAY APIs ACCEPTABLE USE POLICY](https://payments.developers.google.com/terms/aup)
