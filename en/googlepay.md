# 
# Description

### <span>Google Pay&trade;</span> is a fast and secured one-click payment method.
By Google Pay API, the customer can pay using:

1) Tokenized card. In this case, when you click on the Google Pay button, a token is generated, then token must be passed to the Auth method. And that is all.

2) Non-Tokenized card. In this case, you should generate the token, pass it to the Auth method, and go through the 3ds authentication procedure.
 

# Prerequisites

* Your site should serve an HTTPS webpage with a TLS domain-validated certificate.
* Adhere to [Google Pay API Acceptable Use Policy](https://payments.developers.google.com/terms/aup)
* Accept [GOOGLE PAY API TERMS OF SERVICE](https://payments.developers.google.com/terms/sellertos)

# Integration steps

## 1. Add Google Pay Button

### Web.

Step by step short instruction:
* Get token (press Google Pay button)
* Pass token to your backend
* Call <a href="#/en/api?id=googlepay-method">GooglePay</a> method of PayOnline API
* If you got Awaiting3DS, then you should pass extra 3ds authentication and call <a href="#/en/api?id=complete-method">Complete</a> method

Customers will be able to use next payment systems:
* Visa
* MasterCard

Parameters you should specify to process transactions via PayOnline gateway:

* gateway: 'payonline'
* gatewayMerchantId: '123' - your MID in PayOnline system

Example shows how initialize Google Pay button with PayOnline specific parameters, get token and pass to backend. 
```javascript
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

 
When implement integration please use:

* [Google Pay Web developer documentation](https://developers.google.com/pay/api/web/overview)

* [Google Pay Web integration checklist](https://developers.google.com/pay/api/web/guides/test-and-deploy/integration-checklist)

* [Google Pay Web Brand Guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines)

### Android.

To work with PayOnline API you need to use [PayOnline SDK for Android](https://github.com/PayOnlineSystem/PayOnline.SDK.Android)

Step by step short instruction:
* Press Google Pay button to get token
* Call <a href="#/en/api?id=googlepay-method">GooglePay</a> method of PayOnline API
* If you got Awaiting3DS, then you should pass extra 3ds authentication and call <a href="#/en/api?id=complete-method">Complete</a> method

Customers will be able to use next payment systems:
* Visa
* MasterCard

In order to process transactions via PayOnline gateway you should specify next parameters:
* gateway: 'payonline'
* gatewayMerchantId: '123' - your MID in PayOnline system

```kotlin
val PAYMENT_GATEWAY_TOKENIZATION_PARAMETERS = mapOf(
        "gateway" to "payonline",
        "gatewayMerchantId" to "123" //Your MID in PayOnline system
    )
```

```kotlin
val SUPPORTED_NETWORKS = listOf(
        "MASTERCARD",
        "VISA")
```

Full example you can find in our github repository [PayOnline.AndroidSample](https://github.com/PayOnlineSystem/PayOnline.AndroidSample).
Also you can see [Example](https://github.com/google-pay/android-quickstart) of using Google Pay API from Google.

When implement integration please use:

* [Google Pay Android developer documentation](https://developers.google.com/pay/api/android/overview)

* [Google Pay Android integration checklist](https://developers.google.com/pay/api/android/guides/test-and-deploy/integration-checklist).

* [Google Pay Android brand guidelines](https://developers.google.com/pay/api/android/guides/brand-guidelines).

 

## 2. Get Google approve.

Please complete [application](https://services.google.com/fb/forms/googlepayAPIenable). After that, a Google point of contact will reach out you and instruct on further steps. In this step, you will provide your Google point of contact with a link to your integration or apk file to check the integration.
