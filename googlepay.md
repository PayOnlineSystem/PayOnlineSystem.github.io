**Способ использования GooglePay**

GooglePay это быстрый и безопасный метод оплаты в «один клик». Используя GooglePay покупатель может оплачивать с помощью:

1)карты привязанной к его личному google-pay аккаунту (Токенизированная карта). В этом случае При нажатии на кнопку G-Pay генерируется токен, который потом надо передать в метод Auth. И на этом оплата завершается.

2)непривязанной карты. (Нетокенизированная карта). В этом случае вы должны получить токен,  передать его в метод Auth, и пройти процедуру 3ds аутентификации.

 

**Чтобы начать использовать GooglePay надо**

**1)Добавить кнопку G-Pay**

**Для веб страницы.**

Вкратце: вы получаете токен и отправляете его себе на бэкенд используя javaScript. И уже на бэкенде вызывает API PayOnline.

При использовании Google Pay API для оплаты через PayOnline покупатели смогут использовать платёжные системы: Visa и MasterCard.

Пример кода…

AAAAAAAAAAAAAAA

 

Руководство по интеграции от google:  [Google Pay Web developer documentation](https://developers.google.com/pay/api/web/overview), [Google Pay Web integration checklist](https://developers.google.com/pay/api/web/guides/test-and-deploy/integration-checklist).

Правила использования бренда от Google, при добавлении кнопки:[ Google Pay Android brand guidelines](https://developers.google.com/pay/api/web/guides/brand-guidelines).

**Для андроид приложения.**

Пример кода…

Пример использования Google Pay API для оплаты через PayOnline (ссылка на гит-хаб)

[Пример](https://github.com/google-pay/android-quickstart) использования Google Pay API от Google.

Руководство по интеграции от google:  [Google Pay Android developer documentation](https://developers.google.com/pay/api/android/overview), [Google Pay Android integration checklist](https://developers.google.com/pay/api/android/guides/test-and-deploy/integration-checklist).

Правила использования бренда от Google, при добавлении кнопки:  [Google Pay Android brand guidelines](https://developers.google.com/pay/api/android/guides/brand-guidelines).

 

**2)Получить одобрение и зарегистрироваться в Google.**

Для этого надо заполнить[ форму](https://services.google.com/fb/forms/googlepayAPIenable). После этого с вами свяжется представитель Google и проинструктирует о дальнейших шагах.

 

**Обязательные условия при успользовании Google Pay.**

1)      Условия использования[ GOOGLE PAY API TERMS OF SERVICE](https://payments.developers.google.com/terms/sellertos)

Список товаров и услуг, запрещённых к оплате через Google Pay:[ GOOGLE PAY APIs ACCEPTABLE USE POLICY](https://payments.developers.google.com/terms/aup)
