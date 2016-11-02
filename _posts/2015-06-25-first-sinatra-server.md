---
layout: post
title: Ruby 101: Gemfile and your first Sinatra web server
---

This tutorial is designed to get you up and running with a Sinatra server with as little work as possible. This tutorial assumes that you have a basic knowledge of Ruby



#### Introducing Sinatra

Sinatra is a web framework. It is designed to listen for user requests and to provide responses. It is lightweight and designed to get to the point. As frameworks go, it is highly modular and built to scale. It is also (arguably) the easiest way to write a server in Ruby. Let's get started by building a web server in Ruby!

#### 1. Prepwork with Gemfile

You'll need Sinatra. Time to install it! `gem install sinatra`. Next, you need the JSON gem to send responses back via JSON. This also allows us to call `.to_json` on hashes (and other data types such as Arrays). You should `gem install json`. Finally, you want Bundler - `gem install bundler`! This gem allows us to bundle all of your gems together for your application to use.

You need somewhere to store all of the Gems that your application needs - this will be a plaintext file called `Gemfile` (no extension). Gemfiles are where you store information about our application's libraries (aka, gems). Essentially, the Gemfile allows us to specify what gems we want our application to use.

*Gemfile*

```ruby
source 'https://rubygems.org'

gem 'sinatra'
gem 'json'
```

Each time the Gemfile had new Gems added to it, you need to `bundle`. Yes, `bundle` from terminal in the folder where your app lives. Bundler will then grab all the gems and bundle them together for your app. If you add additional gems, don't forget to `bundle`!


---

#### 2. Configuraiton with Config.ru

You now need a file to tell our application how it should be configured. This tells you what we need to do and what settings your app should use. Since you're using Sinatra, this will be pretty simple! Create a `config.ru` in the same directory as your `Gemfile`.

*config.ru*

```ruby
require './app'
run Sinatra::Application
```

What was going on in that file? You required the `app.rb` file to be used by using Ruby's `require` statement. This forces a file to be loaded once into our application and run. You then `run Sinatra::Application` - or tell Sinatra to start. That's it!


#### 3. Communicating with the Web

Now we're going to create one more final - our `app.rb`. Our final app.rb will look like this.. but we're going to build it and explain as we go.

```ruby
require 'bundler'
Bundler.require()

get '/' do
  {:name => 'test'}.to_json
end
```

First, In your `app.rb` file, you now need to to require Bundler. It is what bundles your gems. 

*app.rb*

```ruby
require 'bundler'
Bundler.require()
```

Now, you need to let users access resources on your server. But how will you tell them where to go? Sinatra provides **Routers**. These route the user to http://somedomain.com/route/. Or maybe http://somewhere.com/landing/. You'll define a root route first to access http://localhost/. If this were on a live server, it could be http://somedomain.com/

*app.rb*

```ruby
require 'bundler'
Bundler.require()

get '/' do
  # some code goes here
end
```

Now that you can have a user access a resource via a route. It is time to expose a resource to them! You'll use the JSON gem here to return a Hash back to a user as JSON.

*app.rb*

```ruby
require 'bundler'
Bundler.require()

get '/' do
  {:message => 'hello, world!'}.to_json
end
```

#### 4. Did this work? Let's run your server!

In terminal, run your app. Did you add any new gems? If so, `bundle`! Now, let's start our server

> *Note* This tells the **Rackup** middleware to run our server.

```bash
bundle exec rackup
```

If everything went to plan, you can view your resource. Sinatra listens for requests on port **9292**. Browse to your resource at http://localhost:9292/. Hopefully everything worked out and you'll see `{"message":"hello, world!"}`.


#### 5. Conclusion

* You just build a webserver! Congrats! Hopefully this feels awesome and you want to learn more.

> **Next Steps**: Take a look at the official documentation - http://www.sinatrarb.com/