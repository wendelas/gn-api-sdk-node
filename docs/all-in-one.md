## Create charge and payment

The most common case scenarios will consist of the two steps mentioned in the title. The other examples show each part separately. Here goes the most used endpoints together in one example.

At this point you should've noticed that this sdk works with promises, so that you can concat calls one after another using the `then` method.

Create the inputs for the three endpoints:

```js
var chargeBody = {
  items: [{
    name: 'Product 1',
    value: 1000,
    amount: 2
  }],
  shippings: [{
    name: 'Default Shipping Cost',
    value: 100
  }, {
    name: 'Adicional Shipping Cost',
    value: 150
  }]
}

var paymentBody = {
  payment: {
    credit_card: {
      installments: 1,
      payment_token: '8c888fe1e7d96112020cf9fcf5e4db5b9dba5cf6',
      billing_address: {
        street: 'Av. JK',
        number: 909,
        neighborhood: 'Bauxita',
        zipcode: '35400000',
        city: 'Ouro Preto',
        state: 'MG'
      },
      customer: {
        name: 'Gorbadoc Oldbuck',
        email: 'oldbuck@gerencianet.com.br',
        cpf: '04267484171',
        birth: '1977-01-15',
        phone_number: '5144916523'
      }
    }
  }
}
```

Create the callback function:

```js
var payCharge = function (response) {
  var params = {
    id: response.data.charge_id
  }

  return gerencianet.payCharge(params, paymentBody);
}
```

Call the endpoints:

```js
var gerencianet = new Gerencianet(options);

gerencianet
  .createCharge({}, chargeBody)
  .then(payCharge)
  .then(console.log)
  .catch(console.log)
  .done();
```

Response:

```js
{ "code": 200,
  "data": {
     "charge_id": 260,
     "total": 2250,
     "status": "new",
     "custom_id": null,
     "created_at": "2015-05-18"
   }
} //charge created

{ "code": 200,
  "data": {
     "charge_id": 260,
     "total": 2400,
     "payment": "credit_card",
     "installments": 1,
     "installment_value": 2400
  }
} //payment created
```
