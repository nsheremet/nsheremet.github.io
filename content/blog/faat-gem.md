+++
categories = ["code"]
date = "2015-11-18T14:42:26+03:00"
description = "I want to introduce you with my first open-source project. It's Faat! Gem that will kill your fat model."
tags = ["ruby", "open-source", "faat", "rubygems"]
title = "This is my FAAT!"

+++

I want to introduce you with my first open-source project. It's Faat! Gem that will kill your fat model.

Link at my gem on [RubyGems](https://rubygems.org/gems/faat) and [GitHub](https://github.com/xo8bit/faat)

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'faat'
```

And then execute:
```bash
$ bundle
```

Or install it yourself as:
```bash
$ gem install faat
```

## Usage

Run `rails generate faat:resources {model_name}`,
generator will create folder `resource` in `app` directory, and file `{model_name}_resource.rb`

Run `rails generate faat:forms {form_name} {attribute_name}:{attribute_type}`,
generator will create folder `forms` in `app` directory, and file `{form_name}_form.rb`

### Initialize:
```ruby
@post = Post.new
@post_resource = PostResource.new(@post)

@post_form = PostForm.new(post_form_params)
```

### Usage:
```ruby
@post_resource.destroy  => destroy @post
@post_resource.update   => update @post

PostResource.last     => Post.last
PostResource.all      => Post.all
PostResource.where(title: "First Test Title") => Post.where(...)
```

### Examples:


In `post_resource.rb`
```ruby
class PostResource < Faat::Resources::Base
    ...

    def initialize(post_form)
        @author = Author.create!(name: post_form.author_name, email: post_form.author_email)
        @post = Post.create!(text: post_form.text, title: post_form.title)
        send_confirmation_email(@author)
    end

    ...
end
```

In `post_controller.rb`
```ruby
...
def create
    @post_form = PostForm.new(post_form_params)
    if @post_form.valid?
        @post_resource = PostResource.new(@post_form) => create @author and @post
    end
end
```

Some other usage:
```ruby
@post_form = PostForm.new(post_form_params)
@post_form.valid? # or invalid?
@post_form.text => some value
@post_form.author_email => john@example.com
@ost_form.author_name => John Smith
```
## TODO

Add `spec/test` auto generator


## Contributing

Bug reports and pull requests are welcome on GitHub at [Faat!](https://github.com/xo8bit/faat). This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
