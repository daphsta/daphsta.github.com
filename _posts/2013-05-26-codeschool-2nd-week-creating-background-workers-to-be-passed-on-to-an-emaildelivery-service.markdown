---
layout: post
title: CodeSchool 2nd week - Creating background workers to be passed on to an EmailDelivery
  service
date: 2013-05-26 22:14:31.000000000 +10:00
categories: []
tags:
- Capybara
- codeschool
- Mandrill
- RSpec
image: /assets/images/background_image.jpg

---
So, we have finally arrived at the heart of the app which is sending invitations and confirmations through email using Mandrill. Mandrill is an email service with SMTP setup and API integration. There is even a Mandrill course on Codecademy (yipee!) if going through the documentation is not your thing at the moment.

We actually spent a lot of time constructing the email background worker model using Sidekiq,  and an email deliverer model using HTTParty gem to serialize the content of the email to be sent to Madrill's API. The body of the email is serialized using the ActiveModel::Serializer class which basically allows the user to customize the JSON output; beats passing hashes around.

With a big part of the backend constructed, we needed a front end for the administrator to be able to control the invitations and with that, we were introduced to the ActiveAdmin framework which eliminates the need to build an interface and the basic admin processes.


ActiveAdmin interface making it easy for developers to customize

Here's the most interesting part of the course by far, Capybara testing on the interface! What Capybara does is, it allows the developer to test the application by simulating how a user would interact with it. Capybara comes with a bunch of DSLs that emulates the language a user would use ie: page.should have_content("New User"), Attendee.count.should == 1. Language that is so similar to the English language that I struggled a bit understanding how it should work at a low level (this stems from my C++ background). I soon got a hang of it and stopped converting it to C++ constantly in order to understand the DSLs used.

Finally, our app has a front end and in the next few lessons, we will be integrating all the parts together. And now ,back to my revision and preparation for the upcoming lesson.

p/s: My opinions are based on a beginner's understanding of RoR. Please do correct me if I've made any mistake in terms of terminology and concept.