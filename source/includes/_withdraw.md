<h1 id="withdraw">برداشت</h1>

<h2 id="withdraw-request">ثبت درخواست برداشت</h2>

```shell
curl -X POST 'https://api.nobitex.ir/users/wallets/withdraw' \
  -H 'Authorization: Token yourTOKENhereHEX0000000000' \
  -H 'X-TOTP: 123456' \
  -H 'Content-Type: application/json' \
  --data '{"wallet": 3456, "invoice": "lnbc123m1pskcu80pp5qqqsyqcyq5rqwz..."}'
```

```plaintext
http POST https://api.nobitex.ir/users/wallets/withdraw \
  wallet=3456 invoice=lnbc123m1pskcu80pp5qqqsyqcyq5rqwz...
```

> در صورت فراخوانی درست، پاسخ به این صورت خواهد بود:

```json
{
  "status": "ok",
  "withdraw": {
    "id": 432,
    "createdAt": "2021-12-11T10:13:42.957103+00:00",
    "status": "New",
    "amount": "0.0123",
    "currency": "btc",
    "network": "BTCLN",
    "invoice": "lnbc123m1pskcu80pp5qqqsyqcyq5rqwz...",
    "address": "SaMpLeWaLlEtAdDrEsS",
    "tag": "123456",
    "wallet_id": 3456,
    "blockchain_url": "https://nobitex.ir/receipt/Bitcoin/ewd23d...",
    "is_cancelable": true
  }
}
```

> پس از ثبت درخواست برداشت، یک کد تایید یکبارمصرف به کاربر ایمیل یا پیامک می‌شود.

برای ثبت درخواست برداشت از این نوع درخواست استفاده نمایید:

* آدرس: `POST /users/wallets/withdraw`

* پارامترها:

| پارامتر      | نوع      | پیش‌فرض | توضیحات                 | نمونه                                |
|--------------|----------|---------|-------------------------|--------------------------------------|
| wallet       | int      | الزامی  | شناسه کیف پول کاربر     | 3456                                 |
| network      | string   | اختیاری | شبکه انتقال<sup>1</sup> | "BTCLN"                              |
| invoice      | string   | اختیاری | صورت‌حساب<sup>2</sup>   | "lnbc123m1pskcu80pp5qqqsyqcyq5rqwz..." |
| amount       | monetary | اختیاری | مقدار<sup>3</sup>       | "0.123"                             |
| address      | string   | اختیاری | آدرس مقصد<sup>4</sup>   | "SaMpLeWaLlEtAdDrEsS"                |
| explanations | string   | اختیاری | توضیحات                 | "sample explanation"                 |
| noTag        | boolean  | false   | بدون تگ<sup>5</sup>     | false                                |
| tag          | string   | اختیاری | تگ انتقال<sup>6</sup>   | "123456"                             |

1. **شبکه انتقال:** مقادیر <span class="network-symbols"></span> قابل قبول است. در مواردی که چند شبکه برداشت برای رمزارز درخواستی پشتیبانی می‌شود، می‌توان شبکه انتقال را تعیین نمود.
2. **صورت‌حساب:** در انتقال‌های شبکه `BTCLN` الزامی است. در صورت وجود invoice پارامترهای amount و  address از آن استخراج شده و در بدنه درخواست بلااستفاده خواهند شد. شبکه صورت‌حساب باید با network در صورت مشخص شدن در درخواست مطابقت داشته باشد.
3. **مقدار:** در صورتی که بدون invoice درخواست برداشت ثبت می‌کنید، الزامی می‌شود.
4. **آدرس مقصد:** برای برداشت‌های ریالی [شناسه حساب بانکی](/#88cb25e727 "دریافت از پروفایل") تایید شده کاربر جهت دریافت وجه و برای برداشت‌های رمزارزی آدرس کیف پول مقصد می‌باشد. در صورتی که از invoice برای برداشت استفاده می‌کنید، بلااستفاده خواهد شد.
5. **بدون تگ:** برای برداشت از شبکه‌های <span class="tag-required-networks"></span> که تگ انتقال در آن‌ها ضروری است بدون درج تگ، مقدار آن را true قرار دهید.
6. **تگ انتقال:** در شبکه‌های <span class="tag-required-networks"></span>بدون فعال کردن noTag الزامی است.

<aside class="notice">
این درخواست نیاز به <a href="/#dbf936619c">شناسایی دوعاملی</a> دارد.
</aside>

<aside class="notice">
کاربر درخواست دهنده بایستی اجازه برداشت از کیف پول خود را داشته باشد.
(مثلا دارای حداقل سطح مجاز برای برداشت و شماره تماس تایید شده بوده و سقف برداشت مجاز روزانه و ماهانه خود را رد نکرده باشد و شرایط و پیشنیازات ذکر شده در مقررات نوبیتکس را رعایت کرده باشد.)
</aside>

<aside class="notice">
محدودیت فراخوانی : 10 درخواست در 3 دقیقه
</aside>

<h2 id="withdraw-confirm">تایید درخواست برداشت</h2>

```shell
curl -X POST 'https://api.nobitex.ir/users/wallets/withdraw-confirm' \
  -H 'Authorization: Token yourTOKENhereHEX0000000000' \
  -H 'Content-Type: application/json' \
  --data '{"withdraw": 432, "otp": 623005}'
```

```plaintext
http POST https://api.nobitex.ir/users/wallets/withdraw-confirm \
  withdraw=432 otp=623005
```

> در صورت فراخوانی درست، پاسخ به این صورت خواهد بود:

```json
{
  "status": "ok",
  "withdraw": {
    "id": 432,
    "createdAt": "2021-12-11T10:13:42.957103+00:00",
    "status": "Verified",
    "amount": "0.0123",
    "currency": "btc",
    "network": "BTCLN",
    "invoice": "lnbc123m1pskcu80pp5qqqsyqcyq5rqwz...",
    "address": "SaMpLeWaLlEtAdDrEsS",
    "tag": "123456",
    "wallet_id": 3456,
    "blockchain_url": "https://nobitex.ir/receipt/Bitcoin/ewd23d...",
    "is_cancelable": true
  }
}
```

برای تایید درخواست برداشت با ارسال رمز یکبارمصرف از این نوع درخواست استفاده نمایید:

* **درخواست:** `POST /users/wallets/withdraw-confirm`
* **محدودیت فراخوانی:** 30 درخواست در ساعت

### پارامترهای ورودی

| پارامتر  | نوع | پیش‌فرض | توضیحات                 | نمونه  |
|----------|-----|---------|-------------------------|--------|
| withdraw | int | الزامی  | شناسه درخواست برداشت    | 432    |
| otp      | int | الزامی  | رمز یکبارمصرف ارسال شده | 623005 |

<aside class="notice">
برای تایید برداشت باید کد ارسال شده به شماره تلفن همراه یا ایمیل حساب کاربری خود را از طریق کد به صورت خودکار دریافت کنید و به این API ارسال نمایید. همچنین امکان استفاده از آی‌پی اختصاصی و آدرس‌بوک به زودی فراهم خواهد شد.
</aside>


<h2 id="withdraw-list">فهرست برداشت‌ها</h2>

```shell
curl 'https://api.nobitex.ir/users/wallets/withdraws/list' \
  -H 'Authorization: Token yourTOKENhereHEX0000000000'
```


```plaintext
http GET https://api.nobitex.ir/users/wallets/withdraws/list
```

> در صورت فراخوانی درست، پاسخ به این صورت خواهد بود:

```json
{
  "status": "ok",
  "withdraws": [
    {
      "id": 432,
      "createdAt": "2021-12-11T10:13:42.957103+00:00",
      "status": "Canceled",
      "amount": "0.0123",
      "currency": "btc",
      "network": "BTCLN",
      "invoice": "lnbc123m1pskcu80pp5qqqsyqcyq5rqwz...",
      "address": "SaMpLeWaLlEtAdDrEsS",
      "tag": "123456",
      "wallet_id": 3456,
      "blockchain_url": "https://nobitex.ir/receipt/Bitcoin/ewd23d...",
      "is_cancelable": true
    },
    {
      "id": 238,
      "createdAt": "2020-09-19T14:17:23.441723+00:00",
      "status": "Done",
      "amount": "1000000",
      "currency": "rls",
      "network": "FIAT_MONEY",
      "invoice": null,
      "address": "\u062a\u062c\u0627\u0631\u062a: IR140180000000003333333333",
      "tag": null,
      "wallet_id": 3451,
      "blockchain_url": null,
      "is_cancelable": true
    },
    {
      "id": 239,
      "createdAt": "2018-10-04T12:59:38.196935+00:00",
      "status": "Done",
      "amount": "1",
      "currency": "ltc",
      "network": "LTC",
      "address": "Lgn1zc77mEjk72KvX...",
      "tag": null,
      "wallet_id": 3454,
      "blockchain_url": "https://live.blockcypher.com/ltc/tx/c1ed4229e598d4cf...",
      "is_cancelable": false
    }
  ]
}
```

برای دریافت لیست آخرین برداشت‌ها از این نوع درخواست استفاده نمایید:

* آدرس: `GET /users/wallets/withdraws/list`

* پارامترها:

| پارامتر | نوع    | پیش‌فرض | توضیحات             | نمونه |
|---------|--------|---------|---------------------|-------|
| wallet  | string | all     | شناسه کیف پول کاربر | 3456  |

<aside class="notice">
برای هر کیف پول حداکثر 20 برداشت تایید شده آخر نمایش داده می‌شود.
</aside>

<aside class="notice">
محدودیت فراخوانی : 60 درخواست در 2 دقیقه
</aside>

<h2 id="withdraw-view">مشاهده برداشت</h2>

```shell
curl 'https://api.nobitex.ir/withdraws/433' \
  -H 'Authorization: Token yourTOKENhereHEX0000000000'
```


```plaintext
http GET https://api.nobitex.ir/withdraws/433
```

> در صورت فراخوانی درست، پاسخ به این صورت خواهد بود:

```json
{
  "status": "ok",
  "withdraw": {
    "id": 433,
    "createdAt": "2021-12-14T06:53:11.593812+00:00",
    "status": "Processing",
    "amount": "0.123",
    "currency": "eth",
    "network": "BSC",
    "invoice": null,
    "address": "SaMpLeWaLlEtAdDrEsS",
    "tag": null,
    "wallet_id": 3458,
    "blockchain_url": "https://etherscan.io/tx/tx111111",
    "is_cancelable": true
  }
}
```

برای مشاهده وضعیت یک درخواست برداشت از این نوع درخواست استفاده نمایید:

* آدرس: `GET /withdraws/WITHDRAW`

* پارامترها:

| پارامتر     | نوع | پیش‌فرض | توضیحات              | نمونه |
|-------------|-----|---------|----------------------|-------|
| WITHDRAW | int | الزامی  | شناسه درخواست برداشت | 433   |

<aside class="notice">
محدودیت فراخوانی : 60 درخواست در 2 دقیقه
</aside>
