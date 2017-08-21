+++
date = "2017-08-01T00:00:00-04:00"
draft = false
title = "Real World Application Developer - Part 1"
tags = ["c#"]
categories = [ "Languages", "Software Design" ]
featureimage = ""
menu = ""
+++

Here is the problem statement used for the first segment of the _max column sum by key_ workshop.

<!--more-->

Write a library that can read a file separated by a delimiter (i.e., comma, tab, etc.). 

Write a library that can read in a tab separated data file, generate a summation of the values for each key, and return the key and sum for the key with the greatest sum. The exposed method to perform the operation will take the file path, position of the key, and position of the value as parameters.

For testing we will be using a file of [n-grams](https://storage.googleapis.com/books/ngrams/books/googlebooks-eng-all-1gram-20120701-0.gz) from the Google Books dataset. Once uncompressed the file will contain over 10 million records.

For the given arguments, `ngrams.tsv`, 1, 2 we would expect the following response:

```
{
    Key: 2006,
    Sum: 22569013
}
```

## Additional Resources

If you would like to learn more about n-grams there is a [pretty good write up on Wikipedia](https://en.wikipedia.org/wiki/N-gram).