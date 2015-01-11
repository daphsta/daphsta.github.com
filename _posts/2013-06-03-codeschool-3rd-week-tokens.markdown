---
layout: post
title: CodeSchool 3rd week - Tokens
date: 2013-06-03 18:10:47.000000000 +10:00
categories: []
tags:
- codeschool
image: /assets/images/background_image.jpg
---

That was all we did in week 3, creating email tokens and testing it. It doesn't sound like a lot for 6 hours of lesson but we sure did a lot of discovering when dealing with BCrypt in creating tokens and the output of encoding the hash using urlsafe_encode64. 

At the moment, every attendee will have a 3 unique tokens generated for the actions confirm, pay and decline.  These tokens are generated using the BCrypt::Password create function with a hash.

BCrypt is the most recommended hash function compared to MD5,SHA1,SHA256 etc. From my understanding, BCrypt uses a variant of the Blowfish encryption algorithm that introduces a work factor which determines how expensive the hash function will be. BCrypt is really slow as the work factor increases compared to other hash functions. This would only mean it would take much longer to crack passwords with massive amounts of resources.

With every email sent out, there will be a link for the attendee to click on to notify of his/her decision be it confirm, pay or decline in one step. Dependent on their decision, the attendee object will be moved into a different state and forwarded to the appropriate link. 

This is where BrainTree payment gateway comes into play in the app. All credit card transactions and information storage is handled by BrainTree. The integration to their APIs will be in the next few lessons. 

Of course we took a lot of time writing the RSpec tests on the tokens and the links and refactoring our code to ensure there are no unnecessary repetition. At the moment we have not done anything to the view hence I can't provide any screenshots of our progress. However, this is the project link https://github.com/net-engine/tedx-brisbane in case you want to have a look around.
