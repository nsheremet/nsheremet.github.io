+++
categories = ["code"]
date = "2016-02-18T14:33:44+03:00"
description = "Hi, guys! I decided to remove the ability to create resources.Because for ethical reasons it is better to use the concept of services. Since it is more suitable for the removal of business logic in services. Also the concept of services for wider use. And also since the implementation of resources and services was the same, it would be correct to leave only services."
tags = ["ruby", "open-source", "faat", "rubygems"]
title = "New update in Faat!"

+++

Hi, guys! I decided to remove the ability to create resources.
Because for ethical reasons it is better to use the concept of services. Since it is more suitable for the removal of business logic in services. Also the concept of services for wider use. And also since the implementation of resources and services was the same, it would be correct to leave only services.

Link on [Faat!-v0.1.6](https://github.com/xo8bit/faat)

## Updated usage

Run `rails generate faat:services {service_name}`,
generator will create folder `services` in `app` directory, and file `{service_name}_service.rb`

Run `rails generate faat:forms {form_name} {attribute_name}:{attribute_type}`,
generator will create folder `forms` in `app` directory, and file `{form_name}_form.rb`

### Initialize:

```ruby
@post = Post.new
@post_service = PostService.new(@post)

@post_form = PostForm.new(post_form_params)
```

### Usage:

```ruby
@post_service.destroy  => destroy @post
@post_service.update   => update @post

PostService.last     => Post.last
PostService.all      => Post.all
PostService.where(title: "First Test Title") => Post.where(...)
```

### Examples:

In `post_services.rb`
```ruby

class PostService < Faat::Services::Base
    def initialize(post_form)
        @author = Author.create!(name: post_form.author_name, email: post_form.author_email)
        @post = Post.create!(text: post_form.text, title: post_form.title)
        send_confirmation_email(@author)
    end
end
```

In `post_controller.rb`
```ruby
def create
    @post_form = PostForm.new(post_form_params)
    if @post_form.valid?
        @post_service = PostService.new(@post_form) => create @author and @post
    end
end
```
