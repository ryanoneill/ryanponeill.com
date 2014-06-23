---
layout: post
status: publish
published: true
title: Minimum Coins Change Problem
author: Ryan O'Neill
author_login: admin
author_email: ryanoneill@gmail.com
author_url: http://www.rymeon.com
wordpress_id: 224
wordpress_url: http://rymeon.com/blog/?p=224
date: 2012-05-21 20:58:38.000000000 -07:00
categories: Code
tags: Python, Algorithms
comments: []
---
A question came up recently on how to write an algorithm where given a list of denominations of coins, what is a way to return the exact amount of change using the smallest number of coins possible.

Here is my solution which has not been fully optimized.

~~~python
#!/usr/bin/python
 
import sys
 
def minimum_coins_change(denominations, value):
  # Find the minimum number of coins necessary to 
  # provide the exact change. If exact change is
  # not possible, return None
  result = None
 
  if value == 0:
    # if the value left to compute is 0, we're done
    # and so the value of all other denominations left
    # is zero (if there are any)
 
    result = { }
    for denom in denominations:
      result[denom] = 0
  elif len(denominations) == 0:
    # if the value is not zero and we have no denominations
    # left to use, then we cannot make change using this
    # set of denomination counts chosen
 
    result = None
  else:
    # both the value and the number of denominations left
    # to use are non zero
    denom = denominations[0]
    rest = denominations[1:]
    rest_result = { }
 
    max_count = value / denom
    min_coin_count = sys.maxint
 
    # loop through this denomination and test the different
    # combinations available for the range of values 
    # for this denomination
    for i in range(max_count + 1):
      current_value = value - (denom * i)
      rest_result = minimum_coins_change(rest, current_value)
      # if we can make change given the remainder
      if rest_result != None:
        new_count = i + sum(rest_result.values())
        # test to see if this new set is the minimum coin count
        if new_count < min_coin_count:
          # if so, set it to our result
          min_coin_count = new_count
          result = rest_result
          result[denom] = i
  return result
 
def print_result(denominations, value, result):
  print "Smallest change for", value, "using denominations:", denominations
  if result == None:
    print "  No change possible"
  else:
    print "  %d coins needed" % sum(result.values())
    print "  distributed via", sorted(result.items())
  print 
 
denominations = [1]
value = 67
result = minimum_coins_change(denominations, value)
print_result(denominations, value, result)
 
denominations = [5]
value = 69
result = minimum_coins_change(denominations, value)
print_result(denominations, value, result)
 
denominations = [5, 1]
value = 43
result = minimum_coins_change(denominations, value)
print_result(denominations, value, result)
 
denominations = [100, 50, 25, 10, 5, 1]
value = 72
result = minimum_coins_change(denominations, value)
print_result(denominations, value, result)
 
denominations = [67, 42, 34, 15, 8]
value = 124
result = minimum_coins_change(denominations, value)
print_result(denominations, value, result)
~~~

Here is a running of the program.

    rhino:python_code roneill$ python minimum_coins_change.py
     Smallest change for 67 using denominations: [1]
     67 coins needed
     distributed via [(1, 67)]
    Smallest change for 69 using denominations: [5]
     No change possible
    Smallest change for 43 using denominations: [5, 1]
     11 coins needed
     distributed via [(1, 3), (5, 8)]
    Smallest change for 72 using denominations: [100, 50, 25, 10, 5, 1]
     5 coins needed
     distributed via [(1, 2), (5, 0), (10, 2), (25, 0), (50, 1), (100, 0)]
    Smallest change for 124 using denominations: [67, 42, 34, 15, 8]
     3 coins needed
     distributed via [(8, 0), (15, 1), (34, 0), (42, 1), (67, 1)]
