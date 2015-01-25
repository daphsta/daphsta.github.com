---
layout: post
title:  "Using Interactors to organise business logic"
date:   2015-01-25 14:34:25
categories: [ruby rails learn]
tags:
- rails
- ruby
- pattern
image: /assets/images/background_image.jpg
published: false
comments: true
---

== What is an Interactor
My team has started using the [Interactor](https://github.com/collectiveidea/interactor) gem to encapsulate complex business logic hence, freeing the controllers. The Interactor gem presents us with a very nice way of representing one thing that the application does and allowing us to adhere to the Single Responsibility Principle. Also, if there is a series of action that are required by the business logic, we are able to organise a sequence of interactors using the *Organizer* method.

I believe that it would be easier to demonstrate the functionalties of the *Interactor* gem through code, of course. 
For a working app, click on this *github repo*. The app is built using the *rails-api* gem which is a lightweight rails app minus all the unnecessary view modules.

In this app, I would want to create a *customer* record and associate it to my *user_id* which is my sales_rep id.
I would create an interactor that would *FindSalesRep* and the user_id would be extracted from the params in the JSON document. 

== Constructing an Interactor
A `context` contains everything that an *Interactor* needs to work with. The params from the controller sets the `context` when an *Interactor* is called. 
Creating an interactor is easy. Just create a class that includes *Interactor* and define an instance method `call`, whereby the `context` can be accessed within it. 

```ruby
class FindSalesRep
  include Interactor

  def call
    begin
      result = SalesRep.find(context.user_id)
      context.sales_rep_id = result.id
    rescue => e
      context.fail!(message: e)
    end
  end
end
```
== Working with a Context

As an *Interactor* is run, you may add information to the context.

```ruby
  context.sales_rep_id = result.id
```
The sales_rep_id can be accessed by the controller that runs the *Interactor*.
```ruby
  result = FindSalesRep.call(params)
  puts "Sales rep id #{result.sales_rep_id}"
```
To set a failing `context` when an operation has failed, a `context.fail!` can be flagged. This can be accessed by the controller to render a flash message.

```ruby
  result = FindSalesRep.call(params)
  unless result.success?
    flash.now[:message] = result.message
    render :error
  end
```

== Organizing multiple Interactors

I find the organizer function, which is a variation of the basic *Interactor*, very helpful in explicitly defining the multiple responsibilities require by the controller when processing an action. The organizer is designed to run multiple *Interactors* in one *Interactor* and contents in the `context` is passed to every *Interactor* that the organizer organizes. Each *Interactor* may change the `context` by adding on more information into it to be passed on to the next *Interactor*. If one *Interactor* fails its `context`, the organizer would not call the subsequent *Interactors*.

The code will provide more clarity on this concept.

*client_controller.rb*
```ruby
class ClientsController < ApplicationController

  def create
    res = AssociateSalesRepToClient.call(params)
    if res.success?
      render json: res.client
    else
      render json: res.message.to_s, status: 404
    end
  end
end
```

*associate_sales_rep_to_client.rb*
```ruby
class AssociateSalesRepToClient
  include Interactor::Organizer

  organize FindSalesRep, CreateClient

end
```

*find_sales_rep.rb*
```ruby
class FindSalesRep
  include Interactor

  def call
    begin
      result = SalesRep.find(context.user_id)
      context.sales_rep_id = result.id
    rescue => e
      context.fail!(message: e)
    end
  end
end
```

*create_client.rb*
```ruby
class CreateClient
  include Interactor

  def call
    begin
      result = Client.create(sales_rep_id: context.sales_rep_id, name: context.name, abn: context.abn)
      context.client = result
    rescue => e
      context.fail!(message: e)
    end
  end
end
```
In the example above, the organizer `AssociateSalesRepToClient` defines the process of creating a Client. The task of creating a Client record requires a sales rep id the be retrieved and saved in a Client record. This example may be contrived, but it would be sufficient demonstrate the concept of organizers.
