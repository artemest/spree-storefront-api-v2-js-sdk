# Spree Storefront API SDK

Node module for integration with Spree API V2
---



Сontents:

- [Quick Start](#quick-start)
- [Example](#example)
- [Response schema](#response-schema)
  - [Order schema](#order-schema)
  - [Error schema](#error-schema)
- [Endpoints](#endpoints)
  - [Authentication](#authentication)
    - [getToken](#getToken)
    - [refreshToken](#refreshToken)
  - [Account](#account)
    - [accountInfo](#accountInfo)
    - [creditCardsList](#creditCardsList)
    - [defaultCreditCard](#defaultCreditCard)
    - [completedOrdersList](#completedOrdersList)
    - [completedOrder](#completedOrder)
  - [Order](#order)
    - [status](#status)
  - [Cart](#cart)
    - [create](#create)
    - [show](#show)
    - [addItem](#addItem)
    - [setQuantity](#setQuantity)
    - [removeItem](#removeItem)
    - [emptyCart](#emptyCart)
    - [applyCouponCode](#applyCouponCode)
    - [removeCouponCode](#removeCouponCode)
  - [Checkout](#checkout)
    - [orderUpdate](#orderUpdate)
    - [orderNext](#orderNext)
    - [advance](#advance)
    - [complete](#complete)
    - [addStoreCredits](#addStoreCredits)
    - [removeStoreCredits](#removeStoreCredits)
    - [paymentMethods](#paymentMethods)
    - [shippingMethods](#shippingMethods)
    - [Checkout Examples](#checkout-examples)


<br/>

## Quick Start

1. Install SDK

  `npm install spree-storefront-api-v2-js-sdk --save`

<br/>

2. Initialize `Client`

  ```ts
    import { spreeClient } from 'spree-storefront-api-v2-js-sdk'

    const client = spreeClient({
      host: 'http://localhost:3000'
    })
  ```

<br/>

## Response schema

### Order schema

```ts
  data: {
    id: number
    type: string
    attributes: {
      numbe: string
      item_total: string
      total: string
      ship_total: string
      adjustment_total: string
      created_at: Date
      updated_at: Date
      completed_at: Date
      included_tax_total: string
      additional_tax_total: string
      display_additional_tax_total: string
      display_included_tax_total: string
      tax_total: string
      currency: string
      state: string
      token: string
      email: string
      display_item_total: string
      display_ship_total: string
      display_adjustment_total: string
      display_tax_total: string
      promo_total: string
      display_promo_total: string
      item_count: number
      special_instructions: string
      display_total: string
    }
    relationships: {
      line_items: {
        data: [
          {
            id: string
            type: string
          }
        ]
      }
      variants: {
        data: [
          {
            id: string
            type: string
          }
        ]
      }
      promotions: {
        data: [
          {
            id: string
            type: string
          }
        ]
      }
      payment: {
        data: [
          {
            id: string
            type: string
          }
        ]
      },
      shipments: {
        data: [
          {
            id: string
            type: string
          }
        ]
      }
      user: {
        data: {
          id: string
          type: string
        }
      }
      billing_address: {
        data: {
          id: string
          type: string
        }
      }
      shipping_address: {
        data: {
          id: string
          type: string
        }
      }
    }
  }
```

### Error schema

```ts
  error: string
```


## Endpoints

Spree Storefront API SDK contains each endpoint according to [Spree Guides](https://guides2.spreecommerce.org/api/v2)

## Authentication

### `getToken`

Method `getToken` used for creates a Bearer token required to authorize calls API calls. [Read more](https://guides2.spreecommerce.org/api/v2/authentication)

__parameters schema:__

```ts
  username: string
  password: string
  grant_type: string = 'password'
```

__success response schema:__

```ts
  access_token: string
  token_type: string = 'Bearer'
  expires_in: number
  refresh_token: string
  created_at: number
```

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `refreshToken`

Method `refreshToken` used for refreshes a Bearer token required to authorize calls API calls. [Read more](https://guides2.spreecommerce.org/api/v2/authentication)

__parameters schema:__

```ts
  refresh_token: string
```

__success response schema:__

```ts
  access_token: string
  token_type: string = 'Bearer'
  expires_in: number
  refresh_token: string
  created_at: number
```

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.refreshToken({
      refresh_token: 'aebe2886d7dbba6f769e20043e40cfa3447e23ad9d8e82c632f60ed63a2f0df1'
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

## Account

### `accountInfo`

Returns current user information. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Account%20Information)

__parameters schema:__

```ts
  token: {
    bearerToken: string
  }
```

__success response schema:__

```ts
  data: {
    id: number
    type: string
    attributes: {
      email: string
      store_credits: number
      completed_orders: number
    }
    relationships: {
      default_billing_address: {
        data: {
          id: number
          type: string
        }
      }
      default_shipping_address: {
        data: {
          id: number
          type: string
        }
      }
    }
  }
```

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })
  
    const account = await client.account.accountInfo({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `creditCardsList`
Returns a list of Credit Cards for the signed in User. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Credit%20Cards%20list)

__parameters schema:__

```ts
  token: {
    bearerToken: string
  }
```

__success response schema:__

```ts
  data: [
    {
      id: number
      type: string
      attributes: {
        cc_type: string
        last_digits: string
        month: number
        year: number
        name: string 
        default: boolean
      }
      relationships: {
        payment_method: {
          data: {
            id: string
            type: string
          }
        }
      }
    }
  ]
```

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })
  
    const cardsList = await client.account.creditCardsList({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `defaultCreditCard`
Return the User's default Credit Card. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Default%20Credit%20Card)

__parameters schema:__

```ts
  token: {
    bearerToken: string
  }
```

__success response schema:__

```ts
  data: {
    id: number
    type: string
    attributes: {
      cc_type: string
      last_digits: string
      month: number
      year: number
      name: string 
      default: boolean
    }
    relationships: {
      payment_method: {
        data: {
          id: string
          type: string
        }
      }
    }
  }
```

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })
  
    const defaultCard = await client.account.defaultCreditCard({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `completedOrdersList`
Returns Orders placed by the User. Only completed ones. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Completed%20Orders)


__parameters schema:__

```ts
  token: {
    bearerToken: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })
  
    const ordersList = await client.account.completedOrdersList({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `completedOrder`
Return the User's completed Order. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Completed%20User%20Order)


__parameters schema:__

```ts
  token: {
    bearerToken: string
  }
  id: string
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })
  
    const ordersList = await client.account.completedOrder({
      bearerToken: token.access_token
    }, 'order_id')
  } catch (err) {
    console.error(err)
  }
```

<br/>

## Order

### `status`
Returns placed Order. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Order%20Status)

__parameters schema:__

```ts
  number: string
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {  
    const order = await client.order.status('order_number')
  } catch (err) {
    console.error(err)
  }
```

<br/>

## Cart

### `create`
Creates new Cart and returns it attributes. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Create%20Cart)

__optional parameters schema:__

```ts
  {
    bearerToken?: string 
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const cart = await client.cart.create({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {  
    const cart = await client.cart.create()
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `show`
Returns contents of the cart. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Get%20Cart)

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const cart = await client.cart.show({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    cart = await client.cart.show({
      orderToken: cart.data.attributes.token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `addItem`
Adds a Product Variant to the Cart. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Add%20Item)

__parameters schema:__

```ts
  {
    variant_id: number
    quantity: number
  }
```

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const cart = await client.cart.addItem({
      bearerToken: token.access_token
    }, {
      variant_id: 1,
      quantity: 1

    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    cart = await client.cart.addItem({
      orderToken: cart.data.attributes.token
    }, {
      variant_id: 1,
      quantity: 1

    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `setQuantity`
Sets the quantity of a given line item. It has to be a positive integer greater than 0. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Set%20Quantity)

__parameters schema:__

```ts
  {
    line_item_id: number
    quantity: number
  }
```

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const cart = await client.cart.addItem({
      bearerToken: token.access_token
    }, {
      line_item_id: 9,
      quantity: 100

    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    cart = await client.cart.addItem({
      orderToken: cart.data.attributes.token
    }, {
      line_item_id: 9,
      quantity: 100

    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `removeItem`
Removes Line Item from Cart. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Remove%20Line%20Item)

__parameters schema:__

```ts
  line_item_id: string
```

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const cart = await client.cart.removeItem({
      bearerToken: token.access_token
    }, '1')
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    cart = await client.cart.removeItem({
      orderToken: cart.data.attributes.token
    }, '1')
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `emptyCart`
Empties the Cart. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Empty%20Cart)

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    cart = await client.cart.emptyCart({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    cart = await client.cart.emptyCart({
      orderToken: cart.data.attributes.token
    }, '1')
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `applyCouponCode`
Applies a coupon code to the Cart. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Apply%20Coupon%20Code)

__parameters schema:__

```ts
  {
    coupon_code: string
  }
```

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    cart = await client.cart.applyCouponCode({
      bearerToken: token.access_token
    }, {
      coupon_code: 'promo_test'
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    cart = await client.cart.applyCouponCode({
      orderToken: cart.data.attributes.token
    }, {
      coupon_code: 'promo_test'
    })
  } catch (err) {
    console.error(err)
  }
```

### `removeCouponCode`
Removes a coupon code from the Cart. [Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Remove%20Coupon%20Code)

__parameters schema:__

```ts
  coupon_code: string
```

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const cart = await client.cart.removeCouponCode({
      bearerToken: token.access_token
    }, 'promo_test')
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    cart = await client.cart.removeCouponCode({
      orderToken: cart.data.attributes.token
    }, 'promo_test')
  } catch (err) {
    console.error(err)
  }
```
<br/>

## Checkout

### `orderUpdate`
Updates the Checkout 
You can run multiple Checkout updates with different data types.
[Read more](https://guides2.spreecommerce.org/api/v2/storefront#operation/Update%20Checkout)

__parameters schema:__

```ts
  order: {
    email: string
    bill_address_attributes?: {
      firstname: string
      lastname: string
      address1: string
      city: string
      phone: string
      zipcode: string
      state_name: string
      country_iso: string
    }
    ship_address_attributes?: {
      firstname: string
      lastname: string
      address1: string
      city: string
      phone: string
      zipcode: string
      state_name: string
      country_iso: string
    }
    shipments_attributes?: [
      {
        selected_shipping_rate_id: number
        id: number
      }
    ]
    payments_attributes?: [
      {
        payment_method_id: number
      }
    ]
  }
  payment_source?: {
    [payment_method_id: number]: {
      number: string
      month: string
      year: string
      verification_value: string
      name: string
    }
  }
```

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const order = await client.checkout.orderUpdate({
      bearerToken: token.access_token
    }, {
      order: {
        email: 'john@snow.org',
        bill_address_attributes: {
          firstname: 'John',
          lastname: 'Snow',
          address1: '7735 Old Georgetown Road',
          city: 'Bethesda',
          phone: '3014445002',
          zipcode: '20814',
          state_name: 'MD',
          country_iso: 'US'
        },
        ship_address_attributes: {
          firstname: 'John',
          lastname: 'Snow',
          address1: '7735 Old Georgetown Road',
          city: 'Bethesda',
          phone: '3014445002',
          zipcode: '20814',
          state_name: 'MD',
          country_iso: 'US'
        },
        shipments_attributes: [{
          id: 1,
          selected_shipping_rate_id: 1
        }],
        payments_attributes: [{
          payment_method_id: 1
        }]
      },
      payment_source: {
        1: {
          number: '4111111111111111',
          month: '1',
          year: '2022',
          verification_value: '123',
          name: 'John Doe'
        }
      }
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    cart = await client.checkout.orderUpdate({
      orderToken: cart.data.attributes.token
    }, {
      order: {
        email: 'john@snow.org',
        bill_address_attributes: {
          firstname: 'John',
          lastname: 'Snow',
          address1: '7735 Old Georgetown Road',
          city: 'Bethesda',
          phone: '3014445002',
          zipcode: '20814',
          state_name: 'MD',
          country_iso: 'US'
        },
        ship_address_attributes: {
          firstname: 'John',
          lastname: 'Snow',
          address1: '7735 Old Georgetown Road',
          city: 'Bethesda',
          phone: '3014445002',
          zipcode: '20814',
          state_name: 'MD',
          country_iso: 'US'
        },
        shipments_attributes: [{
          id: 1,
          selected_shipping_rate_id: 1
        }],
        payments_attributes: [{
          payment_method_id: 1
        }]
      },
      payment_source: {
        1: {
          number: '4111111111111111',
          month: '1',
          year: '2022',
          verification_value: '123',
          name: 'John Doe'
        }
      }
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `orderNext`

Goes to the next Checkout step
[Read more](https://guides2.spreecommerce.org/api/v2/storefront/#operation/Checkout%20Next)

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const order = await client.checkout.orderNext({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    const order = await client.checkout.orderNext({
      orderToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `advance`

Advances Checkout to the furthest Checkout step validation allows, until the Complete step. [Read more](https://guides2.spreecommerce.org/api/v2/storefront/#operation/Advance%20Checkout)

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const order = await client.checkout.advance({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    const order = await client.checkout.advance({
      orderToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `complete`
Completes the Checkout. [Read more](https://guides2.spreecommerce.org/api/v2/storefront/#operation/Complete%20Checkout)

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const order = await client.checkout.complete({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    const order = await client.checkout.complete({
      orderToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `addStoreCredits`
Adds Store Credit payments if a user has any. [Read more](https://guides2.spreecommerce.org/api/v2/storefront/#operation/Add%20Store%20Credit)


__parameters schema:__
```ts
  {
    amount: number
  }
```

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const order = await client.checkout.addStoreCredits({
      bearerToken: token.access_token
    }, {
      amount: 100
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    const order = await client.checkout.addStoreCredits({
      orderToken: token.access_token
    }, {
      amount: 100
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `removeStoreCredits`
Remove Store Credit payments if any applied. [Read more](https://guides2.spreecommerce.org/api/v2/storefront/#operation/Remove%20Store%20Credit)

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ [Order schema](#order-schema)
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const order = await client.checkout.removeStoreCredits({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    const order = await client.checkout.removeStoreCredits({
      orderToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `paymentMethods`
Returns a list of available Payment Methods. [Read more](https://guides2.spreecommerce.org/api/v2/storefront/#operation/Payment%20Methods)

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ 
```ts
  {
    data: [
      {
        id: atring
        type: string
        attributes: {
          type: string
          name: string
          description: string
        }
      }
    ]
  }
```
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const order = await client.checkout.paymentMethods({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    const order = await client.checkout.paymentMethods({
      orderToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### `shippingMethods`
Returns a list of available Shipping Rates for Checkout. Shipping Rates are grouped against Shipments. Each checkout cna have multiple Shipments eg. some products are available in stock and will be send out instantly and some needs to be backordered. [Read more](https://guides2.spreecommerce.org/api/v2/storefront/#operation/Shipping%20Rates)

__optional parameters schema:__

```ts
  {
    bearerToken?: string
    orderToken?: string
  }
```

__success response schema:__ 
```ts
  {
    data: [
      {
        id: atring
        type: string
        attributes: {
          type: string
          name: string
          description: string
        }
      }
    ]
  }
```
<br/>

__failed response schema:__ [Error schema](#error-schema)
<br/>

__Example:__

```ts
  try {
    const token = await client.authentication.getToken({
      username: 'spree@example.com',
      password: 'spree123'
    })

    const order = await client.checkout.shippingMethods({
      bearerToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }

  // or

  try {
    let cart = await client.cart.create()

    const order = await client.checkout.shippingMethods({
      orderToken: token.access_token
    })
  } catch (err) {
    console.error(err)
  }
```

<br/>

### Checkout Examples

1. One-step checkout

```ts
try {
  const { access_token: accessToken } = await client.authentication.getToken({
    username: 'spree@example.com',
    password: 'spree123'
  })

  await client.cart.create({
    bearerToken: accessToken
  })

  await client.cart.addItem({
    bearerToken: accessToken
  }, {
    variant_id: 1,
    variant_id: 10
  })

  const shipping = await client.checkout.shippingMethods({
    bearerToken: accessToken
  })

  const payment = await client.checkout.paymentMethods({
    bearerToken: accessToken
  })

  let order = await client.checkout.orderUpdate({
    bearerToken: token.access_token
  }, {
    order: {
      email: 'john@snow.org',
      bill_address_attributes: {
        firstname: 'John',
        lastname: 'Snow',
        address1: '7735 Old Georgetown Road',
        city: 'Bethesda',
        phone: '3014445002',
        zipcode: '20814',
        state_name: 'MD',
        country_iso: 'US'
      },
      ship_address_attributes: {
        firstname: 'John',
        lastname: 'Snow',
        address1: '7735 Old Georgetown Road',
        city: 'Bethesda',
        phone: '3014445002',
        zipcode: '20814',
        state_name: 'MD',
        country_iso: 'US'
      },
      shipments_attributes: [{
        id: shipping.data[0].id,
        selected_shipping_rate_id: shipping.data[0].relationships.shipping_rates.data[0].id
      }],
      payments_attributes: [{
        payment_method_id: payment.data[0].id
      }]
    },
    payment_source: {
      [payment.data[0].id]: {
        number: '4111111111111111',
        month: '1',
        year: '2022',
        verification_value: '123',
        name: 'John Doe'
      }
    }
  })

  order = await client.checkout.complete({
    bearerToken: token.access_token
  })
} catch (err) {
  console.error(err) 
}
```

1. Three-step checkout

```ts
try {
  const { access_token: accessToken } = await client.authentication.getToken({
    username: 'spree@example.com',
    password: 'spree123'
  })

  await client.cart.create({
    bearerToken: accessToken
  })

  await client.cart.addItem({
    bearerToken: accessToken
  }, {
    variant_id: 1,
    variant_id: 10
  })

  // Step one 

  let order = await client.checkout.orderUpdate({
    bearerToken: token.access_token
  }, {
    order: {
      email: 'john@snow.org',
      bill_address_attributes: {
        firstname: 'John',
        lastname: 'Snow',
        address1: '7735 Old Georgetown Road',
        city: 'Bethesda',
        phone: '3014445002',
        zipcode: '20814',
        state_name: 'MD',
        country_iso: 'US'
      },
      ship_address_attributes: {
        firstname: 'John',
        lastname: 'Snow',
        address1: '7735 Old Georgetown Road',
        city: 'Bethesda',
        phone: '3014445002',
        zipcode: '20814',
        state_name: 'MD',
        country_iso: 'US'
      }
    },
  })

  await client.checkout.orderNext({
    bearerToken: accessToken
  })

  // Step two

  const shipping = await client.checkout.shippingMethods({
    bearerToken: accessToken
  })

  let order = await client.checkout.orderUpdate({
    bearerToken: token.access_token
  }, {
    order: {
      shipments_attributes: [{
        id: shipping.data[0].id,
        selected_shipping_rate_id: shipping.data[0].relationships.shipping_rates.data[0].id
      }]
    }
  })

  await client.checkout.orderNext({
    bearerToken: accessToken
  })

  // Step three

  const payment = await client.checkout.paymentMethods({
    bearerToken: accessToken
  })

  let order = await client.checkout.orderUpdate({
    bearerToken: token.access_token
  }, {
    order: {
      payments_attributes: [{
        payment_method_id: payment.data[0].id
      }]
    },
    payment_source: {
      [payment.data[0].id]: {
        number: '4111111111111111',
        month: '1',
        year: '2022',
        verification_value: '123',
        name: 'John Doe'
      }
    }
  })

  // Order complete

  await client.checkout.orderNext({
    bearerToken: accessToken
  })

  order = await client.checkout.complete({
    bearerToken: token.access_token
  })
} catch (err) {
  console.error(err) 
}
```
