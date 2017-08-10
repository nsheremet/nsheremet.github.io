+++
categories = ["code"]
date = "2016-01-26T14:36:32+03:00"
description = "As I wrote earlier, I am developing a gem-generator for the separation of the base rails logic to resources, services and forms. Today I released a new version of my gem [Faat!-v0.1.5](https://github.com/xo8bit/faat). And added the module `services` to separate the logic of your rails projects."
tags = ["ruby", "open-source", "faat", "rubygems"]
title = "How to use services in Rails with Faat!"

+++

### Faat! v0.1.5

As I wrote earlier, I am developing a gem-generator for the separation of the base rails logic to resources, services and forms. Today I released a new version of my gem [Faat!-v0.1.5](https://github.com/xo8bit/faat). And added the module `services` to separate the logic of your rails projects.

Now, you will see examples of working with service.

### Example:

Ok, we just generated new `payment_service.rb`:
```ruby
class PaymentService < Faat::Services::Base

  def year_payment
    # ...
    if charge(year_amount)
      # ...
      send_subscribing_email
    end
  end

  # ...

  private

  def send_subscribing_email
    # sending email
  end

  def charge(amount)
    # some payment logic
  end

end
```

This is `user_controller.rb`:
```ruby
class SubscribesController < ActionController::Base
  def create
    @subscribe = Subscribe.create(subscribe_params)

    if @subscribe.premium
      PaymentService(@subscribe).year_payment
      # ...
    else
      # ...
    end
  end
  # ...
end
```

Also we need to have `subscribe.rb` model:
```ruby
class Subscribe < ActiveRecord::Base
# ...
end
```

As you can see, all the logic related to subscription and payment, now separated and moved to a separate service. So the code organization looks much better. We have completely separated the business logic and the controller logic.

It's all to do not necessary, but it all looks better after this refactoring your code well organized. And it gives the opportunity for other developers to understand better your code.

Of course, you can do everything yourself, without using any additional gems, or something like that. But `Faat!` gives you the opportunity to quickly generate you the necessary files and immediately begin to write the logic in the services or resources.

P.S. Fork `Faat!` on [github](https://github.com/xo8bit/faat)! or share this post in Twitter, Facebook!
