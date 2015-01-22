---
layout: post
title:  "Inject,each_with_object,reduce - fun Ruby enumerables"
date:   2015-01-23 14:34:25
categories: ruby
image: /assets/images/background_image.jpg
published: true
---

I have learnt to love Enumerables and it is imperative to understand how it works because nobody would want to write 
nested `for` loops all the time.
Solving problems in [Exercism](http://www.exercism.io) has taught me the joy of using enumerables and how it can effeciently solve problems in a succint manner.

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
    
    
The first iteration goes through the given collection, `(1..num)` and returns an array, which is the object given to `each_with_object([])`. You can pass in an empty hash object, `each_with_object({})` to return a hash result.
In the block, `sq` becomes the accumulator value(memo value) that will accumulate an array of squares by iterating through every element, `n`.
And because an array is returned, I could method chain the result with an `inject` method to sum every element in the array.

The #inject method is really lovely in the sense that it allows you to apply an operation on a collection by specifying a block or symbol.

#####inject(initial, sym) → obj definition
> Combines all elements of enum by applying a binary operation, specified by a block or a symbol that names a method or operator.

> If you specify a block, then for each element in enum the block is passed an accumulator value (memo) and the element. If you specify a symbol instead, then each element in the collection will be passed to the named method of memo. In either case, the result becomes the new value for memo. At the end of the iteration, the final value of memo is the return value for the method.

According to the definition on [Ruby Doc](http://ruby-doc.org/core-2.2.0/Enumerable.html#method-i-each_with_object), `inject` expects an initial value and a symbol whereby each element in the collection will be passed to the name of the method, in this case a `:+` which would sum every element in the collection.

In the above example I would have 
```ruby
[1,4,9,16,25,36,49,64,81,100].inject(0,:+)
```
This is just a looong way to arrive at the desired result and I think I can do better. With just a bit of refactoring of the first iteration of my solution and it should look good. So, I have decided to use the enumerator `reduce`, which essentially is an alias for `inject`. I think `reduce` gives a better meaning to this example as it is collapsing all the elements into one.

```ruby
def sum_of_squares
  (1..num).reduce { |sum,n| sum + (n ** 2) }
end
```
As per the definition for `inject`, I am able to specify a block in `reduce`, and an accumulator value, in this case `sum`.
Because there isn't an initial value specified in this block, `sum` is going to assume the value of the first element as the initial value(in this case `1`) and `n` will be set to the 2nd element.

If `num` was set to 2, we would have the code with values as such:

```ruby
def sum_of_squares
  (1..2).reduce { |sum,n| sum + (n ** 2) }
end
```
`sum` initial value in this case would be 1 and `n` would be set to 2, hence the returning the value 5.

####Conclusion
If you are coming from a language like C++, you would find these methods delightful to use (hence creating happy Rubyists). I would use it to build hash or an array by applying a method onto every element in the collection. `each_with_object` may be an overkill for the problem above but it is definitely useful when transforming values that we have queried from the database. 

