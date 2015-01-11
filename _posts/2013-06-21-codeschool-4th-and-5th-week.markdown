---
layout: post
title: CodeSchool 4th and 5th week
date: 2013-06-21 17:36:58.000000000 +10:00
categories: []
tags:
- API
- BrainTree
- codeschool
- coffeescript
- handlebars
image: /assets/images/background_image.jpg
---

### Week 4- BrainTree integration

The whole week was focused on the payment workflow and integration with BrainTree. It was a complex process getting our app to integrate and ensuring all the proper fields are passed on to their servers. I'm not saying that it is complicated to integrate with BrainTree, in fact once you have all the right information and keys set up, it is a straightforward process if the app accepts payments and passes it onto BrainTree's servers.

We have chosen to use the Transparent Redirect transaction whereby all payments will be submitted directly to BrainTree's servers and no credit card information will be stored on the app server. Every merchant who signs up will be given a set of API keys to be included in the app.rb. Developers can sign up for a sandbox account to obtain the keys 

To create transactions, a HTML form_for is set up to retrieve the Customer's Details and Credit Card Details. The form includes a hidden transaction data which contains the transaction type and URL to be directed to.

	tr_data = Braintree::TransparentRedirect.transaction_data(
	  :redirect_url => "http://example.com/url_to_redirect_to",
	  :transaction => {
	    :type => "sale",
	    :amount => "10.00"
	  }
	)
All we needed to do in our controller is to verify if the attendee has received an invitation and to create the transaction details to be passed on to their servers. The app will then await for a URL confirmation from BrainTree's servers and notify the user if they payment has been successful or failed. Obtaining the URL confirmation is as simple as setting the API below:

	result = Braintree::TransparentRedirect.confirm(request.query_string)                                                                   
### Week 5 - API design and Handlebars templates

I was really looking forward towards learning about writing our own APIs and how the APIs would be consumed in the front end of the app. Why did we need APIs in our app? The index page contains a dashboard with a live feed of the status of attendees. Visitors will be able to see the number of people who are Awaiting for Invitation, Received Invitation, Paid, Received Reminder, Confirmed and Declined. Visitors will be able to register to get an invitation and then go through the payment process for confirmation. 

Abiding by API conventions, we use namespace to keep track of our versioning. The Api::V1::BaseController responds to a JSON string and authenticates the HTTP request with a token. The Api::V1::AttendeesController < Api::V1::BaseController responds with a serialized string of the statistics of attendees to be displayed on the dashboard. To serialized, we used Active-Model-Serializer again to construct a JSON string. 

To consume the API, our front end is written in CoffeeScript to render the statistics to be displayed on the dashboard. I must say that it takes a bit of getting used to writing JavaScript without the {},; and var. Just like Ruby, all CoffeeScript syntaxes are terse and it compiles into JavaScript. One of the advantages of using CoffeeScript is that, variables are not global by default hence it would prevent confusion of having variables with the same names from different JS files. I think I'm sticking to CoffeeScript and would not touch JavaScript unless necessary. 

The data from the API is passed into a function that will invoke the HandlebarsTemplates.

	HandlebarsTemplates['attendee_statistics'](data.attendee_statistics);
This will run the Handlebars file stored in the asset pipeline and it will be compiled, compressed and cached like all the JavaScript files in the app.