---
layout: post
title:  "Raindrops exercise in Exercism"
date:   2015-01-16 14:34:25
categories: ruby
image: /assets/images/background_image.jpg
---

So this is how I spent my Friday night before bedtime, solving a problem on [Exercism](http://www.http://exercism.io/daphsta).

#### This is the problem
###### Raindrops

Write a program that converts a number to a string, the contents of which depends on the number's prime factors.

- If the number contains 3 as a prime factor, output 'Pling'.
- If the number contains 5 as a prime factor, output 'Plang'.
- If the number contains 7 as a prime factor, output 'Plong'.
- If the number does not contain 3, 5, or 7 as a prime factor,
  just pass the number's digits straight through.

###### Examples

- 28's prime-factorization is 2, 2, 7.
  - In raindrop-speak, this would be a simple "Plong".
- 1755 prime-factorization is 3, 3, 3, 5, 13.
  - In raindrop-speak, this would be a "PlingPlang".
- The prime factors of 34 are 2 and 17.
  - Raindrop-speak doesn't know what to make of that,
    so it just goes with the straightforward "34".

#### My solution (not the best but will go through another iteration)

  ```ruby  
  class Raindrops
    RAINDROPS = {
        '3' => 'Pling',
        '5' => 'Plang',
        '7' => 'Plong'
    }
    def self.convert(prime_num)
      if prime_num == 1
        return prime_num.to_s
        else
          primes = find_prime_factors(prime_num)
        end
      drops = primes.map { |p| RAINDROPS[p.to_s] }.join
      drops.empty? ? prime_num.to_s : drops
    end
  
    def self.find_prime_factors(num)
      primes = []
      (2..num).each do |n|
        if(num % n == 0)
          primes << n if is_prime?(n)
        end
      end
      primes
    end
  
    def self.is_prime?(num)
      (2..(Math.sqrt(num).ceil)).each do |n|
        return false if (num % n == 0)
      end
    end
  end
    
Yes, I do realise there is a Prime class that I could use, but I didn't want to.
If you haven't checkout [Exercism](http://www.exercism.io) already, you're still not too late to the party.
