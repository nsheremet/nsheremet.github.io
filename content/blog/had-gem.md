+++
author = "Nazarii Sheremet"
categories = ["code"]
date = "2017-07-18T13:11:29+03:00"
description = "A couple of days ago, I was searching a library, that will generate API documentation from RSpec specs."
draft = false
tags = ["ruby", "hanami", "api", "documentation", "had"]
title = "Hanami Api Documentation [had]"

+++

### Hi guys.

A couple of days ago, I was searching a library, that will generate API documentation from RSpec specs. But I had one big problem, it's Hanami Framework. We use Hanami for a couple of projects. Because they have good structure, they haven't ActionSupport and many other pros. The result is I couldn't find any library for Hanami, only for Rails/Sinatra.

So. My first step was to find a good and simple library, that already has basic modules for creating HTML pages with documentations or create docs in JSON/pdf files. After this, I should start making changes in code, that's step two.
My coworker sent me the link with this library. This is [reqres_rspec](https://github.com/reqres-api/reqres_rspec) gem. This lib simply generates HTML/JSON/pdf files and even can download generated files to Amazon S3. It seemed to me that this library has more capabilities than I need.
I decided to start working, and if I find some problems with this library, I will find another one.

First of all, I should change collector of data. The collector is parsing files of Rails controllers and finding their actions with comments above.

```ruby
def get_action_comments(controller, action)
  # this line is only for Rails
  lines = File.readlines(File.join(ReqresRspec.root, 'app', 'controllers', "#{controller}_controller.rb"))

  # some code here
rescue Errno::ENOENT
  ['not found']
end
```

This is parsing already collected comment lines with information about actions and controllers that are in testing process.

```ruby
def get_action_description(controller, action)
  comment_lines = get_action_comments(controller, action)

  description = []
  comment_lines.each_with_index do |line, index|
    if line.match(/\s*#\s*@description/) # @description blah blah
      description << line.gsub(/\A\s*#\s*@description/, '').strip
      comment_lines[(index + 1)..-1].each do |multiline|
        if !multiline.match(/\s*#\s*@param/)
          description << "\n"
          description << multiline.gsub(/\A\s*#\s*/, '').strip
        else
          break
        end
      end
    end
  end

  description.join ' '
end
```

All information is getting from parsing action files and testing environment. From tests getting a lot of information about requests and responses from actions what currently in testing. But the way that RSpec works with Hanami actions is much different than we know in Rails. There are no real HTTP requests with headers and much more. There is a lot of problems with session and cookies.

Here is how looks like simple Hanami RSpec spec

```ruby
require_relative '../../../app/controllers/api/books'

RSpec.describe BooksApi::Controllers::Api::Book do
  let(:action)    { described_class.new }
  let(:params)    { Hash[] }
  let(:response)  { action.call(params) }
  let(:data)      { JSON.parse(response.last[0]) }

  describe 'with valid params' do
    it 'should return all books' do
      expect(response[0]).to eql(200)
      expect(data).not_to be_nil
    end
  end
end

```
Did you get it?
The example is if you send a `rack.session` id as Cookie in a real running app, it will get the session from Redis. And if you sent Cookie in your test you just get a Cookie header without the session.

I didn't try to understand why this is happening. But I think it's because of Rack. Hanami using Rack, and `rack.session` id in Cookies using Rack, and for getting a session from Redis, it's also for this magic gem. But Rack in action specs not used. That's why we have this problem with collections data from tests. I can't get information about URL, path, host, query parameters.

For this type of information, I made a special comment above the `call` method. So you can use it like this.

```ruby
module BooksApi::Controllers::Api
  class Book
    include PublicApi::Action

    # @method GET
    # @path /api/books
    # @host booksapi.com
    def call(params)
      # some books stuff
    end
  end
end
```

It's temporary. In future updates, this information will be getting from `routes.rb` file.

All other parts I left from the `reqres_rspec` gem. So you can add sections and titles for all of your spec code.

```ruby
require_relative '../../../app/controllers/api/books'

RSpec.describe BooksApi::Controllers::Api::Book do
  # some `lets` here

  describe 'with valid params', had_section: 'Books' do
    it 'should return all books', had_title: 'All' do
      # some `checks` here
    end
  end
end

```

So, If you want to help me with `had` gem, go to [Github Issues](https://github.com/nsheremet/had/issues).
Also many thanks to the author of `reqres_rspec` gem, and if you want to contribute in this gem, here is one more link on [repo](https://github.com/reqres-api/reqres_rspec).

Kind Regards,
Nazarii Sheremet
