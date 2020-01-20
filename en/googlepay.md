**Description**

GooglePay is a fast and secure one-click payment method. Using GooglePay, the buyer can pay using:

1) Tokenized card. In this case, when you click on the G-Pay button, a token is generated, token then must be passed to the Auth method. And that is all.

2) Non-Tokenized card. In this case, you should generate the token, pass it to the Auth method, and go through the 3ds authentication procedure.

 

**To start using GooglePay you need**

**1) Add G-Pay Button**

**For web page.**

In short: you should get a token and pass it to your backend system. Then your backend calls the PayOnline API.

*When using the Google Pay API to pay via PayOnline, customers will be able to use payment systems: Visa and MasterCard.*

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

 
When implementing Google Pay integration please use:

1) [Google Pay Web developer documentation](https://developers.google.com/pay/api/web/overview), [Google Pay Web integration checklist](https://developers.google.com/pay/api/web/guides/test-and-deploy/integration-checklist).

2) [ Google Pay Android brand guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines).

**Для андроид приложения.**

Пример кода…

[Example](https://github.com/PayOnlineSystem/PayOnline.AndroidSample) of using the Google Pay API to pay via PayOnline

[Example](https://github.com/google-pay/android-quickstart) of using Google Pay API from Google.

Google integration guide:  [Google Pay Android developer documentation](https://developers.google.com/pay/api/android/overview), [Google Pay Android integration checklist](https://developers.google.com/pay/api/android/guides/test-and-deploy/integration-checklist).

Google’s branding guidelines when adding a button:  [Google Pay Android brand guidelines](https://developers.google.com/pay/api/android/guides/brand-guidelines).

 

**2) Get Google approve.**

Для этого надо заполнить[ форму](https://services.google.com/fb/forms/googlepayAPIenable). После этого с вами свяжется представитель Google и проинструктирует о дальнейших шагах. На этом шаге вы отправите в Google ссылку на вашу интеграцию или apk файл для оценки интеграции.

 

**Обязательные условия при успользовании Google Pay.**

1)Условия использования[ GOOGLE PAY API TERMS OF SERVICE](https://payments.developers.google.com/terms/sellertos)

2)Список товаров и услуг, запрещённых к оплате через Google Pay:[ GOOGLE PAY APIs ACCEPTABLE USE POLICY](https://payments.developers.google.com/terms/aup)
