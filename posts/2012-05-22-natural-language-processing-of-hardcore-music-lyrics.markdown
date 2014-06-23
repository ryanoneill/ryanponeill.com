---
layout: post
status: publish
published: true
title: Natural Language Processing of Hardcore Music Lyrics
author: Ryan O'Neill
author_login: admin
author_email: ryanoneill@gmail.com
author_url: http://www.rymeon.com
wordpress_id: 230
wordpress_url: http://rymeon.com/blog/?p=230
date: 2012-05-22 21:25:21.000000000 -07:00
categories: Hardcore Music, Code
tags: Hardcore Music, Gorilla Biscuits, Bad Brains, Python, NLTK, NLP, Lyrics, Cro-Mags
comments: []
---
The [Natural Language Toolkit (NLTK)](http://nltk.org) is a collection of libraries and programs written in Python to perform Natural Language Processing (NLP) operations on data sets. One of the simplest statistical measurements to perform is a [Frequency Distribution](http://en.wikipedia.org/wiki/Frequency_distribution), which measures how often a given word is used within a particular text. I thought it might be fun to use NLTK to analyze the lyrics of three classic hardcore records from different time periods and see how well the analysis conveys the message of the individual band.

__Album #1: Bad Brains - Bad Brains (1982)__

__Album #2: Cro-Mags - Age of Quarrel (1986)__

__Album #3: Gorilla Biscuits - Start Today (1989)__

Many NLTK functions deal with tokenized texts and because the texts being analyzed here are lyrics, I decided to use a very simple Regular Expression based tokenizer that just split the words based on whitespace. This yielded the following results:

|     | Bad Brains           | Cro-Mags             | Gorilla Biscuits     |
| --- | -------------------- | -------------------- | -------------------- |
|  1. | to - 53              | you - 75             | you - 82             |
|  2. | the - 51             | the - 68             | the - 68             |
|  3. | you - 43             | i - 51               | i - 60               |
|  4. | i - 35               | and - 43             | to - 51              | 
|  5. | and - 23             | to - 43              | and - 48             |
|  6. | we - 22              | a - 25               | a - 37               |
|  7. | me - 20              | it - 25              | it - 34              |
|  8. | my - 20              | just - 25            | not - 30             |
|  9. | i'm - 19             | gotta - 24           | my - 29              |
| 10. | a - 18               | i'm - 24             | it's - 26            |

The results in this case are extremely generic. The second word for all three are all the same. Only at the very end of the Cro-Mags list with the word "gotta" (We Gotta Know) does any semblance of personality start to emerge. Obviously we need a way to filter out more of the very basic, generic English words. These generic words are called [stop words](http://en.wikipedia.org/wiki/Stopwords), and the NLTK provides a collection of them to use for filtering them out of texts. Filtering out the stop words yielded the following results:

|     | Bad Brains           | Cro-Mags             | Gorilla Biscuits     |
| --- | -------------------- | -------------------- | -------------------- |
|  1. | i'm - 19             | gotta - 24           | it's - 26            |
|  2. | got - 18             | i'm - 24             | don't - 22           |
|  3. | don't - 14           | can't - 19           | i'm - 16             |
|  4. | it's - 14            | don't - 19           | time - 16            | 
|  5. | right - 11           | it's - 19            | you're - 15          |
|  6. | that's - 11          | see - 19             | like - 13            |
|  7. | gonna - 9            | world - 18           | know - 11            |
|  8. | guess - 8            | cause - 15           | things - 11          |
|  9. | like - 8             | know - 14            | get - 10             |
| 10. | i've - 7             | things - 14          | that's - 10          |

Filtering out the stop words definitely provides some better results in this case. The word which we pointed out for the Cro-Mags, 'gotta', shoots right to the top of the list. Additionally, other patterns come to light including 'world' (World Peace), 'cause' - John Joseph's preferred choice over the word 'because', and 'know' (We Gotta Know). For the Bad Brains, words like 'right' (Right Brigade) and 'guess' (I) start to shine through as well. And for the Gorilla Biscuits, 'time' (New Direction, Start Today) surfaces as a lyrical theme. While these lists are much better than the first set, there are still too many generic words, and most of those words appear to be contractions that were skipped over in filtering out the stop words. Here are the further processed results with the contractions filtered out:

|     | Bad Brains           | Cro-Mags             | Gorilla Biscuits     |
| --- | -------------------- | -------------------- | -------------------- |
|  1. | got - 18             | gotta - 24           | time - 16            |
|  2. | right - 11           | see - 19             | like - 13            |
|  3. | gonna - 9            | world - 18           | know - 11            |
|  4. | guess - 8            | cause - 15           | things - 11          | 
|  5. | like - 8             | know - 14            | get - 10             |
|  6. | see - 7              | things - 14          | think - 9            |
|  7. | want - 7             | get - 13             | see - 8              |
|  8. | bad - 8              | justice - 13         | want - 8             |
|  9. | pay - 6              | street - 13          | make - 7             |
| 10. | sail - 6             | back - 12            | up - 7               |

Filtering down those lists just a little further produces three entirely different lists.
The Cro-Mags list adds both 'justice' and 'street' (Street Justice), whereas the Bad Brains list adds 'pay' (Pay to Cum) and 'sail' (Sailin' On). The Gorilla Biscuits list is a little different though. None of the words pop out as instantly recognizable, but if the words are instead looked at as a whole set then a pattern becomes clear. All the words have an action quality to them 'like', 'know', 'get', 'think', 'see' which is something very much inline with the title of the record "Start Today". So overall a very good first pass at taking a look at Hardcore Music lyrics using Natural Language Processing and NLTK.

Code is available [on github](https://github.com/ryanoneill/lyrics).
