# Csob payment gateway

## BEWARE!
You should probably use the original library https://github.com/sedrickcz/csob_payment_gateway
This is a quick hack, where you have to insert actual keys right into the config variables (config.private_key and config.public_key).


## Installation

Add this line to your application's Gemfile:

```ruby
gem 'csob_payment_gateway'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install csob_payment_gateway

## Usage

### Initialize

config/initializers/csob.rb

```ruby
CsobPaymentGateway.configure do |config|
  config.close_payment      = true
  config.currency           = 'CZK'
  config.environment        = Rails.env.production? ? :production : :test
  config.gateway_url        = "https://iapi.iplatebnibrana.csob.cz/api/v1.7"
  config.merchant_id        = "M1MIPS0459"
  config.private_key        = "-----BEGIN RSA PRIVATE KEY----- ...."
  config.public_key         = "-----BEGIN PUBLIC KEY----- ..."
  config.return_method_post = true
  config.return_url         = "http://localhost:3000/orders/process_order"
end
```
### Payment

```ruby
payment = CsobPaymentGateway::BasePayment.new(
  total_price: 60_000,
  cart_items:  [
    { name: 'Item 1',   quantity: 1, amount: 50_000, description: 'Item 1' },
    { name: 'Shipping', quantity: 1, amount: 10_000, description: 'Shipping' }
  ],
  order_id:    1234,
  description: 'Order from your-eshop.com - Total price 600 CZK',
  customer_id: 1234
)

init   = payment.payment_init
pay_id = init['payId']

redirect_to payment.payment_process_url
```
