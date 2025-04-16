# Frontend Hosted Payments Page Test

In this frontend technical test candidates will be presented with an opportunity to demonstrate their proficiency in building modern and user-friendly web applications. The test will assess candidates' skills in designing responsive and interactive user interfaces, implementing efficient state management, and handling data retrieval and rendering with API integration. Emphasis will be placed on writing clean and maintainable code, adhering to best practices, and showcasing a deep understanding of React or Next.js frameworks. Candidates will be expected to leverage their knowledge of component-based architecture, routing, and state management techniques to create a seamless and visually appealing user experience.

<img width="2306" alt="hpp" src="https://github.com/BVNK-Interviews/frontend-hpp-test/assets/132915473/7be815d2-38d2-44f8-9f06-3a1e7b678af9">

## Video Demo
<div>
  <a href="https://www.loom.com/share/2ae3a8880b814c2a981e97c0705caf7f">
    <p>Watch Video</p>
  </a>
  <a href="https://www.loom.com/share/2ae3a8880b814c2a981e97c0705caf7f">
    <img style="max-width:300px;" src="https://cdn.loom.com/sessions/thumbnails/2ae3a8880b814c2a981e97c0705caf7f-with-play.gif">
  </a>
</div>

## The Test 
Create a **Public** GitHub repository for a Hosted Payment Page (HPP) application using React or Next.js, TypeScript, and earn bonus points by adding test coverage. Feel free to utilize any CSS frameworks and JavaScript libraries of your choice.

#### Postman 
This repository offers a 'Postman Pack' designed to swiftly generate payments through our sandbox API. It encompasses:
- Collection - `payments.postman_collection.json`
- Environment - `sandbox.postman_environment.json`

Import both files into Postman, in the screenshot below make sure the "Create Payment In" is using the "Sandbox" environment:

<img width="917" alt="paymentIn" src="https://github.com/BVNK-Interviews/frontend-hpp-test/assets/132915473/9fc61eb2-fb7c-4f24-be41-fdac6af0875f">

The Collection utilizes predefined Environment variables stored in `sandbox.postman_environment.json` to effortlessly configure the required authentication headers for initiating a payment creation process. Simply click 'Send' and upon a successful response, a new payment will be generated. The response payload will contain a `uuid` and the necessary data for your application development.

Kindly be aware that after accepting or upon the expiration of a quote, you will need to generate a fresh payment to validate your application's steps.

#### Setup
- The application should run on `localhost:3000`. 
- Please provide steps in the README to run the application. 

#### Application Routes
- "Accept Quote": `<DOMAIN>/payin/<UUID>`
- "Pay Quote": `<DOMAIN>/payin/<UUID>/pay`
- "Expiry": `<DOMAIN>/payin/<UUID>/expired`

#### Accept Quote Page
This page marks the initial stage of the customer's payment journey. Here, the customer is provided with essential details to review before proceeding. These details include the merchant's name, the payable amount, the reference for the transaction, and the option to choose their preferred currency for payment.
  - Onload: 

    `GET` `https://api.sandbox.bvnk.com/api/v1/pay/<UUID>/summary`
  - "Pay with" dropdown currencies:
    - Bitcoin `BTC`
    - Ethereum `ETH`
    - Litecoin `LTC`
  - When the customer selects a currency from the dropdown eg. Bitcoin (BTC):
  
    `PUT` `https://api.sandbox.bvnk.com/api/v1/pay/<UUID>/update/summary`
    ```
    {
      "currency": "BTC",
      "payInMethod": "crypto"
    }
    ```
  - On API success `200`:
    - Show the "Amount due" (payload: `paidCurrency.amount` and `paidCurrency.currency`)
    - "Quoted price expires in" (payload: `acceptanceExpiryDate`)
    - "Confirm" button. 
  - When the customer clicks "Confirm":
    
    `PUT` `https://api.sandbox.bvnk.com/api/v1/pay/<UUID>/accept/summary`
    ```
    {
      "successUrl": "no_url"
    }
    ```
  - On success `200` redirect to "Pay Quote".

#### Pay Quote Page
Moving forward in the payment journey, we arrive at a pivotal stage. Here, the customer encounters a page titled "Pay with Bitcoin," a short payment summary. Vital information such as the amount due, the BTC address for payment, a convenient QR code, and the remaining time before the quote expires are all prominently presented on this page. 
- Onload: 

  `GET` `https://api.sandbox.bvnk.com/api/v1/pay/<UUID>/summary`

- "Amount due" (payload: `paidCurrency.amount` and `paidCurrency.currency`)
- "Crypto Address" (payload: `address.address`)
- "Time left to pay" (payload: `expiryDate`)

#### Expiry Page
Should a quote expire, this page will be displayed to the user.

#### Timers 
1. "Accept Quote" - Once a currency is selected the quote should be refreshed every ±30 seconds, use the `acceptanceExpiryDate` value to determine when to call the api `PUT` `https://api.sandbox.bvnk.com/api/v1/pay/<UUID>/summary` to refresh the quote and UI.
2. "Pay Quote" has an expiry date set by the api, use the `expiryDate` to add an expiry count down timer.

#### Copy to clipboard
"Pay Quote" has 2 fields that should be copied to the clipboard when individually clicked
- Amount due (payload: `paidCurrency.amount`)
- Address (payload: `address.address`)

#### Redirects
  - `quoteStatus: "ACCEPTED"` redirect to Pay Quote `<DOMAIN>/payin/<UUID>/pay`
  - `status: "EXPIRED"` redirect to Expiry `<DOMAIN>/payin/<UUID>/expired`


## Resources

#### Figma 
https://www.figma.com/file/5rSrG0uy1ELR1DxxkZPOpK/HPP-Test?type=design&mode=design&t=uNATVIOjHSBlBX2G-1

#### Postman



#### Pay In example responses

`GET` `https://api.sandbox.bvnk.com/api/v1/pay/<UUID>/summary`

```
{
    "uuid": "fcbacea9-070f-4d69-96ce-db873999c95a",
    "merchantDisplayName": "Merchant Display Name",
    "merchantId": "ab9435fa-16fe-4aa1-be9c-8128ce7e72de",
    "dateCreated": 1690372347000,
    "expiryDate": 1690545147000,
    "quoteExpiryDate": null,
    "acceptanceExpiryDate": null,
    "quoteStatus": "TEMPLATE",
    "reference": "REF292970",
    "type": "IN",
    "subType": "merchantPayIn",
    "status": "PENDING",
    "displayCurrency": {
        "currency": "EUR",
        "amount": 200,
        "actual": 0
    },
    "walletCurrency": {
        "currency": "EUR",
        "amount": 200,
        "actual": 0
    },
    "paidCurrency": {
        "currency": null,
        "amount": 0,
        "actual": 0
    },
    "feeCurrency": {
        "currency": "EUR",
        "amount": 0,
        "actual": 0
    },
    "displayRate": null,
    "exchangeRate": null,
    "address": null,
    "returnUrl": "",
    "redirectUrl": "https://pay.sandbox.bvnk.com/payin?uuid=fcbacea9-070f-4d69-96ce-db873999c95a",
    "transactions": [],
    "refund": null,
    "refunds": []
}
```

`PUT` `https://api.sandbox.bvnk.com/api/v1/pay/<UUID>/accept/summary`

```
{
    "uuid": "fcbacea9-070f-4d69-96ce-db873999c95a",
    "merchantDisplayName": "Merchant Display Name",
    "merchantId": "ab9435fa-16fe-4aa1-be9c-8128ce7e72de",
    "dateCreated": 1690372347000,
    "expiryDate": 1690545147000,
    "quoteExpiryDate": 1690384240000,
    "acceptanceExpiryDate": 1690373470000,
    "quoteStatus": "ACCEPTED",
    "reference": "REF292970",
    "type": "IN",
    "subType": "merchantPayIn",
    "status": "PENDING",
    "displayCurrency": {
        "currency": "EUR",
        "amount": 200,
        "actual": 0
    },
    "walletCurrency": {
        "currency": "EUR",
        "amount": 200,
        "actual": 0
    },
    "paidCurrency": {
        "currency": "BTC",
        "amount": 0.00758898,
        "actual": 0
    },
    "feeCurrency": {
        "currency": "EUR",
        "amount": 2,
        "actual": 0
    },
    "displayRate": {
        "base": "BTC",
        "counter": "EUR",
        "rate": 26354.002777711892
    },
    "exchangeRate": {
        "base": "BTC",
        "counter": "EUR",
        "rate": 26354
    },
    "address": {
        "address": "mkCVeT3J5oCj7L4opm2rzb2UvEcgkWGRC2",
        "tag": null,
        "protocol": "BTC",
        "uri": "bitcoin:mkCVeT3J5oCj7L4opm2rzb2UvEcgkWGRC2?amount=0.00758898",
        "alternatives": []
    },
    "returnUrl": "",
    "redirectUrl": "https://pay.sandbox.bvnk.com/payin?uuid=fcbacea9-070f-4d69-96ce-db873999c95a",
    "transactions": [],
    "refund": null,
    "refunds": []
}
```
