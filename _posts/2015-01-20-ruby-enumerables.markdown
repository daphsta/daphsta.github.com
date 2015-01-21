---
layout: post
title:  "Inject,each_with_object,reduce - fun Ruby enumerables"
date:   2015-01-20 14:34:25
categories: ruby
image: /assets/images/background_image.jpg
published: false
---

I have learnt to love Enumerables and it is imperative to understand how it works because nobody would want to do 
nested `for` loop all the time.
Solving problems in [Exercism](http://www.exercism.io) has taught me the joy of using enumerables and how it can effeciently solve problems.

Take for example this problem from the **Difference of Squares** exercise from Exercism.

  The sum of the squares of the first ten natural numbers is,
  
    1**2 + 2**2 + ... + 10**2 = 385
  
  The square of the sum of the first ten natural numbers is,
  
    (1 + 2 + ... + 10)**2 = 55**2 = 3025

To calculate the sum of squares, I initially chose to use the `each_with_object` and `inject` methods.

```ruby
  def sum_of_squares
    (1..num).each_with_object([]) { |n,sq| sq << n*n }.inject(0,:+)
  end
```
#####each_with_object(obj) { |(*args), memo_obj| ... } definition
> Iterates the given block for each element with an arbitrary object given, and returns the initially given object.
    
    
The first iteration goes through the given block, `(1..num)` and returns an array, which is the object given to `each_with_object([])`. You can pass in an empty hash object, `each_with_object({})` to return a hash result.
In the block, `sq` becomes the memo value that will accumulate an array of squares by iterating through every element, `n`.
And because an array is returned, I could method chain the result with an inject method to sum every element in the array.


