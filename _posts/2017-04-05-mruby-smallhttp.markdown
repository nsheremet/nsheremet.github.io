---
layout: post
title:  How I wrote the http-client for mruby
date:   2017-04-04 9:29:20
comments: true
categories: en
tags: [ruby, open-source, mrbgems, mruby, http, http-client]
---
Hi guys.
While I was working on the [library](https://github.com/nsheremet/mruby-tbot) for Telegram API.
I ran into the problem of finding a good http-client library. Some libraries did not allow `multipart/form-data` requests. And in others it was necessary to form the body itself. I thought that this is a problem that I can solve. I wrote the library a small library, which is a small `http-client` (less than 200 lines of code). There are still small problems in it. So I will be glad to any help.

So here is link on [mruby-smallhttp](https://github.com/nsheremet/mruby-smallhttp).

## How to Install by mrbgems
- add conf.gem line to `build_config.rb`

```ruby
MRuby::Build.new do |conf|
  conf.gem :mgem => 'mruby-smallhttp'
end
```

## Usage

### Requests
```ruby
# GET Request
HTTP.new("https://example.com/api/v1/users").get
#=> response

# POST Request
data = { name: 'value' }
headers = {'Content-Type' => 'application/json'}

HTTP.new("https://example.com/api/v1/users").post(data, headers)
#=> response

# PUT Request
http = HTTP.new("https://example.com/api/v1/users/1")
http.put(data, headers)

# DELETE Request
http = HTTP.new("https://example.com/api/v1/users/1")
http.delete(data, headers)
#=> response

# HEAD & OPTIONS Request
http = HTTP.new("https://example.com/api/v1/users/1")
http.request("HEAD", body, header)
#=> response
```

### How to send file
```ruby
# How to send file in post request
body = { name: 'value', file: File.read('filename') }
header = { 'Content-Type' => 'multipart/form-data' }
http = HTTP.new("https://example.com/api/v1/users/1")
http.post(body, header)
#=> response
```
`Content-Type` supported: `application/json`, `application/x-www-form-urlencoded`, `multipart/form-data`
