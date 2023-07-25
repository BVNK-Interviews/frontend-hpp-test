# Frontend Hosted Payments Page Test

In this frontend technical test candidates will be presented with an opportunity to demonstrate their proficiency in building modern and user-friendly web applications. The test will assess candidates' skills in designing responsive and interactive user interfaces, implementing efficient state management, and handling data retrieval and rendering with API integration. Emphasis will be placed on writing clean and maintainable code, adhering to best practices, and showcasing a deep understanding of React or Next.js frameworks. Candidates will be expected to leverage their knowledge of component-based architecture, routing, and state management techniques to create a seamless and visually appealing user experience.

## Test 
- Build a Hosted Payment Page (HPP) app using React or NextJS
- Routes
  - "Accept Quote": `<DOMAIN>/payin/<UUID>`
  - "Pay Quote": `<DOMAIN>/payin/<UUID>/pay`
  - "Expiry": `<DOMAIN>/payin/<UUID>/expired`
- Timers 
  -  "Accept Quote" should be refreshed every ±18 seconds, use the `acceptanceExpiryDate` to `PUT` `https://api.sandbox.bvnk.com/api/v1/pay/<UUID>/summary` to refresh the quote and UI.
  - "Pay Quote" has an expiry date set by the api, use the `quoteExpiryDate` to add an expiry count down timer.
- Copy 
  - "Pay Quote" has 2 fields that should be copied to the clip board when individually clicked
    - Amount due
    - Address
- Redirects
  - If the payment status is `ACCEPTED` redirect to "Pay Quote" `<DOMAIN>/payin/<UUID>/pay`
  - If the payment has expired redirect to "Expiry" `<DOMAIN>/payin/<UUID>/expired`
- Typescript
- Test coverage optional


### Resources

<img width="2168" alt="hpp" src="https://github.com/BVNK-Interviews/frontend-hpp-test/assets/132915473/9f896141-6590-4e5b-8438-1c8a77847e40">

Figma: https://www.figma.com/file/5rSrG0uy1ELR1DxxkZPOpK/HPP-Test?type=design&mode=design&t=uNATVIOjHSBlBX2G-1

Postman pack: `Link here`

#### Pay In example

`https://api.sandbox.bvnk.com/api/v1/pay/<UUID>/summary`

```
{
    "uuid": "54574426-c442-4647-8349-e74eb68a6747",
    "merchantDisplayName": "Merchant Display Name",
    "merchantId": "ab9435fa-16fe-4aa1-be9c-8128ce7e72de",
    "dateCreated": 1690291623000,
    "expiryDate": 1690464423000,
    "quoteExpiryDate": 1690302429000,
    "acceptanceExpiryDate": 1690291659000,
    "quoteStatus": "PENDING",
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
        "amount": 0.0075759,
        "actual": 0
    },
    "feeCurrency": {
        "currency": "EUR",
        "amount": 0,
        "actual": 0
    },
    "displayRate": {
        "base": "BTC",
        "counter": "EUR",
        "rate": 26399.503689330642
    },
    "exchangeRate": {
        "base": "BTC",
        "counter": "EUR",
        "rate": 26399.5
    },
    "address": null,
    "returnUrl": "",
    "redirectUrl": "https://pay.sandbox.bvnk.com/payin?uuid=54574426-c442-4647-8349-e74eb68a6747",
    "transactions": [],
    "refund": null,
    "refunds": []
}
```
