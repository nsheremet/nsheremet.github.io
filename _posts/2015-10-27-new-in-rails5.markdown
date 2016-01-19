---
layout: post
title:  "Що нового в Rails 5"
date:   2015-10-27 12:30:00
categories: ukr
tags: [ruby, rails, ruby on rails, rails 5]
---
![Keep Calm and code in Ruby ](https://cdn-images-1.medium.com/max/2000/1*5_YdyN2Ee-xIqM3JHZeIpQ.png)

Rails 5 були аннонсовані [dhh](https://twitter.com/dhh)’шом цього квітня на RailsConf 2015 в Атланті.

## Ну Окей, так що ж нового в Rails 5?

Команда Rails Core зажди робила мажорні релізи кожні два року. Тож вихід Rails 5 вписуєтся в вихід до кінця цього року. Ніжче перелік нових можливостей Rails 5.

* Пяті Рельси працюють тількі на Ruby 2.2.1 або вище
* Гем Rails Api змержились в Rails, лінк на [PR#19832](https://github.com/rails/rails/pull/19832)
* Тепер рендерити темплейти можна поза контроллером [PR#18546](https://github.com/rails/rails/pull/18546)
* Turbolinks 3
* Action Cable. тепер в вашу аплікуху інтегруються WebSockets. Одна з накращих фіч в новій версії.
* Action Record. Дадали трохи змін до логіки роботи #or, можна подивитись [тут](https://github.com/rails/rails/pull/16052). Також додали has_secure_token, тобто тепер можлива авто генерація токенів.

{% highlight ruby %}
class User < ActiveRecord::Base
  has_secure_token :token1, :token2, key_length: 30
end

user = User.new
user.save
user.token1 # => "973acd04bc627d6a0e31200b74e2236"
user.token2 # => "e2426a93718d1817a43abbaa8508223"
user.regenerate_token1! # => true
user.regenerate_token2! # => true
{% endhighlight %}

* До ActiveSupport додали нових методів для роботи з часом #weekend?, #next_weekday і #prev_weekday
* ActiveModel, допрацювали повернення деталізованих помилок при валідації.

{% highlight ruby %}
class User < ActiveRecord::Base
  validates :name, presence: true
end
user = User.new; user.valid?; user.errors.details => {name: [{error: :blank}]}
{% endhighlight %}

* CaseSensitive валідація
* ActiveJob. Дадали пріоритети для кожного Job’a

## Зміни в Rails 5

* Всі rake команди, переїхали до rails. Це означає те що тепер всі команди по роботі з міграціями, сідами та іншими штуками для яких треба було писати rake … буде працювати через команду rails. Ну таке собі покращення, але новачкам стане краще орієнтуватись в самих рельсах. [PR #18878](https://github.com/rails/rails/issues/18878)
* Покарщено Minitest Runner, можна подивитися тут [PR # 19216](https://github.com/rails/rails/pull/19216)
* в belongs_to тепер є валідація яка перевіряє чи була створена дана ассоціація. Дуже доречі круто! [PR #18233](https://github.com/rails/rails/issues/18233)
* Зміни в контроллері HTTP, покращена логіка [PR #18323](https://github.com/rails/rails/pull/18323)
* ActionView. Деякі метод-хелпери такі як content_tag_for , div_for та інші, перенесли з ядра Рельсів в окремий гем [record-tag-helper](https://github.com/rails/record_tag_helper).
* ActiveJob. Деякі зміни в локалі [PR #20800](https://github.com/rails/rails/pull/20800)

Даний пост є трохи зміненим перекладом на українську мову, [original post](http://kryptonlabs.com/blog/2015/10/25/whats-new-in-rails-5/).
