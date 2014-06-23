---
layout: post
status: publish
published: true
title: Simple Anagrams Lookup
author: Ryan O'Neill
author_login: admin
author_email: ryanoneill@gmail.com
author_url: http://www.rymeon.com
wordpress_id: 216
wordpress_url: http://rymeon.com/blog/?p=216
date: 2012-05-16 12:38:07.000000000 -07:00
categories: Code
tags: Python
comments: []
---
A question came up recently on how to build a very simple program that has the ability to lookup anagrams for a given word. An anagram of a given word is a word spelled using the same letters as the given word but in a different order. Here is my take on the solution using Python.

Here is the output from running the program:

    rhino:python_code roneill$ ./anagrams.py
    ['opt', 'pot', 'top']
    ['asterin', 'eranist', 'restain', 'stainer', 'starnie', 'stearin']
    ['something']

Here is the source code for the program:

~~~python
#!/usr/bin/python
 
# Converts a word to a hash key
# by taking the word, converting it
# to lowercase, sorting the letters
# and combining the word back as a
# string
def word_to_key(word):
  return ''.join(sorted(word.lower()))
 
# Takes a filename a builds a word
# list hash with the value being a 
# list of anagrams for the given 
# key 
def build_word_list(filename):
  word_list = { }
  input_file = open(filename, 'rU')
  for line in input_file:
    word = line.strip()
    key = word_to_key(word)
    if key in word_list:
      word_list[key].append(word)
    else:
      word_list[key] = [word]
  input_file.close()
  return word_list
 
# Retrieves the anagrams for the
# word given the word_list and 
# the word. Results include
# the original word.
def get_anagrams(word_list, word):
  key = word_to_key(word)
  if key in word_list:
    anagrams = word_list[key]
  else:
    anagrams = [word]
  return anagrams 
  
# Build the word_list and retrieve
# anagrams for some example words
def main(): 
  word_list = build_word_list('/usr/share/dict/words')
  anagrams = get_anagrams(word_list, 'top')
  print(anagrams)
  anagrams = get_anagrams(word_list, 'retains')
  print(anagrams)
  anagrams = get_anagrams(word_list, 'something')
  print(anagrams)
  
if __name__ == '__main__':
  main()
~~~
