---
layout: post
title:  "Operate - service objects for Ruby on Rails"
date:   2019-02-27 06:24:00 -0800
author: Justin Tomich
categories: blog
---

Service objects contain business logic. We use service objects to remove business logic from the controller and model, and provide a separate object for it. Clean up code duplication, fat models and bloated controllers. Let models and controllers focus on their primary responsibilities.

In Rails, service objects are usually invoked from a controller action. The service object should orchestrate all domain logic necessary to perform the application's function. The service object is usually meant to contain everything that happens in a controller action without all of the HTTP-related functions: parsing params, rendering views, or redirecting to another action. And even parsing params is arguably not appropriate for a controller.

### The Operate Gem

The [operate] gem provides a framework for writing service objects. Operate focuses on making interactions with a service object easy and readable, eliminating conditionals where possible when interacting with a service object. An operate-based service class broadcasts events, and the client (your contorller) supplies code blocks that subscribe to those events.

Here's a very simple example showing operate in action:

```ruby
class UserAddition
  include Operate::Command
  
  def initialize(form)
    @form = form
  end
  
  def call
    return broadcast(:invalid) unless @form.valid?    
    transaction do
      create_user
      audit_trail
      welcome_user
    end
    broadcast(:ok)
  end
  
  def create_user
    # ...
  end
  
  def audit_trail
    # ...
  end
  
  def welcome_user
    # ...
  end
end

class UserController < ApplicationController
  def create
    @form = UserForm.new(params)
    UserAddition.call(@form) do
      on(:ok)      { redirect_to dashboard_path }
      on(:invalid) { render :new }
    end
  end
end
```

The UserAddition service object encapsulates all business logic. The UserController knows nothing about the operation other than the where to direct the user. All logic, and all conditionals, are removed from the controller.

### An example refactoring

We'll start with an example controller from `Fearless Refactoring` (by Arkency). This controller contains business logic, along with parameter handling, ip filters, and redirects:

```ruby
class PaymentGatewayController < ApplicationController
  ALLOWED_IPS = ["127.0.0.1"]
  before_filter :whitelist_ip

  def callback
    order = Order.find(params[:order_id])
    transaction = order.order_transactions.create(callback:
      params.slice(:status, :error_message, :merchant_error_message, :shop_orderid, :transaction_id, :type, :payment_status, :masked_credit_card, :nature, :require_capture, :amount, :currency))
    if transaction.successful?
      order.paid! 
      OrderMailer.order_paid(order.id).deliver 
      redirect_to successful_order_path(order.id)
    else
      redirect_to retry_order_path(order.id)
    end
  rescue ActiveRecord::RecordNotFound => e 
    redirect_to missing_order_path(params[:order_id])
  rescue => e
    Honeybadger.notify(e) 
    AdminOrderMailer.order_problem(order.id).deliver
    redirect_to failed_order_path(order.id), alert: t("order.problems")
  end

  private
      
  def whitelist_ip
    raise UnauthorizedIpAccess unless 
      ALLOWED_IPS.include?(request.remote_ip) 
  end
end
```

The controller is readable but complex. The business logic is tangled up in the controller and difficult to test. And as the app evolves, the controller will become more complicated. 

Let's extract the business logic into an service object. Leave the HTTP-specific tasks (IP whitelisting, params, redirects) in the controller.

Basic steps:
- create service class, `include Operation::Command`
- move business logic from controller to `call` method of service object
- identify parameters needed by service, add to `initialize` method of service
- service object makes `broadcast` calls to signal in the service object
- call the service object in the controller, with `on` blocks for the correct redirects

```ruby
class PaymentGatewayCallbackService
  include Operation::Command

  def initialize(order_id, gateway_transaction_attributes)
    @order_id = order_id
    @gateway_transaction_attributes = gateway_transaction_attributes
  end

  def call
    order = Order.find(@order_id)
    transaction = order.order_transactions.create(callback: @gateway_transaction_attributes)
    transaction.successful? or return broadcast(:transaction_failed)
    order.paid!
    OrderMailer.order_paid(@order.id).deliver
    broadcast :ok
  rescue ActiveRecord::RecordNotFound => e
    broadcast :record_not_found
  rescue TransactionFailed => e
    broadcast :transaction_failed
  rescue => e
    Honeybadger.notify(e)
    AdminOrderMailer.order_problem(order.id).deliver
    broadcast :order_problem
  end
end

class PaymentGatewayController < ApplicationController
  ALLOWED_IPS = ["127.0.0.1"]
  before_filter :whitelist_ip
  
  def callback
    PaymentGatewayCallbackService.call(params[:order_id], gateway_transaction_attributes) do
      on(:ok) { redirect_to successful_order_path(params[:order_id]) }
      on(:record_not_found) { redirect_to missing_order_path(params[:order_id]) }
      on(:transaction_failed) { redirect_to retry_order_path(params[:order_id]) }
      on(:order_problem) { failed_order_path(params[:order_id]), alert: t("order.problems") }
    end
  end
    
  # Extracted to small helper method
  def gateway_transaction_attributes
    params.slice(:status, :error_message, :merchant_error_message, :shop_orderid, :transaction_id, :type, :payment_status, :masked_credit_card, :nature, :require_capture, :amount, :currency)
  end
end
```

In this case, we pull out the business logic to the service class, and opted to leave the IP whitelisting and params handling in the controller. The next step would be to extract the parameter handling from the controller, possibly by adding them into a form object. But that's for another day.

[Operate]: https://github.com/tomichj/operate
