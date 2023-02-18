---
title: "Webscraping with BeautifulSoup & Robobrowser"
date: 2018-08-01
draft: false
series: "Random"
tags: ["python", "webscraping"]
---

This is a quick example of how to find a get data from a website into a spreadsheet in an automated fashion. This can be useful if you are scraping data such as the price of an item, and just want to set a scraper to check every hour and write the time and price into a spreadsheet for you.

This content also exists in video form:
{{< youtube gqLIhNkRgHQ >}}

## Requirements

- Python 3
- [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [Robobrowser](https://robobrowser.readthedocs.io/en/latest/readme.html)
- [Pathlib](https://pbpython.com/pathlib-intro.html)
- [CSV Module](https://docs.python.org/3/library/csv.html)

### BeautifulSoup

BeautifulSoup is possibly the most popular html parsing package for Python. It provides quick and easy tools for parsing by letting us go through the html code using simple techniques, such as find, siblings, parents, strings, etc...

I will not go into depth, as the documentation is more than enough to get started. I will instead focus more on an actual project.

BeautifulSoup is good when the website is nicely formatted, as it allows you to quickly select HTML tags etc... however, it can be a pain when the website contains chunks of code that look nearly identical except for the data that you want to extract. I commonly find myself having to loop over lists, or use regex to find more complex occurances that BeautifulSoup cannot easily handle.


### Robobrowser

RoboBrowser isn't too popular, and isn't updated very often. However it is, to my knowledge, currently the best implementation for browsing the internet. It is built on top of the requests library, and makes opening sites, parsing HTML, filling out forms (Which allows us to login, sign up, search queries), and pressing buttons etc...

RoboBrowser is quite simple to use, but due to it's simplicity it can be harder to do custom/complex stuff, and for this you might want to use the requests library it is built on. But for day to day use, RoboBrowser speeds up my workflow significantly.

### Pathlib

Pathlib is my favourite library for handling file paths. It allows us to easily create subfolders, check if files exist, get names of files etc...

It is built in a way that lets us add to a path, break down a path into its constituents, and get back the names of files very easily.

I highly recommend this source to learn it as well as the official documentation: [Practical Business Python Pathlib Tutorial](https://pbpython.com/pathlib-intro.html).

### CSV Module

The CSV library allows us to quickly read and write files to CSV. It has a very useful writer called `DictWriter`, which allows writing dictionaries into a CSV, using the keys as headers and the values as values. This allows me to quickly be able to append and write them independently.

A neat trick is to use a conditional statement with Pathlibs `.is_file()` function to check whether the file exists, and if not write headers. This means it will create the file, write the column headers, but next time around it will only write the content. I find this very useful when I am using the csv DictWriter in a loop.

The documentation for it is very good, feel free to simply copy the DictWriter code from it: [csv DictWriter doc](https://docs.python.org/3/library/csv.html#csv.DictWriter).

## What We'll be Parsing

Before beginning, please keep in mind some websites do not like you parsing, especially if you spam their website with requests. Parse responsably, and if a website offers an API they most likely want you to use that.

I will be parsing a website that contains market data for the popular video game EVE Online. Please do not spam this website, you can try parsing it for this tutorial as our requests are simple. [https://market.fuzzwork.co.uk/hub/type/34/](https://market.fuzzwork.co.uk/hub/type/34/)

The sections of data I want have red rectangles around them:
![Market Image](/webscraping_robobrowser/website_market_image.png)

### Preliminary Steps

Before jumping into the code we want to look at the actual data that we want to parse. I know how the data looks on the website, but remember, we are parsing HTML and therefore I need to know what I am looking for.

![Market Image HTML](/webscraping_robobrowser/web_market_html.png)

The first thing I do is look for any obvious ways to get this information. The easiest way to use BeautifulSoup is to use `find_all` command, in which we can enter specific HTML attributes such as the id or in this case the href. For example it would be great if we could simply do this:
```python
find_all('a', href=True)
```
This would return each instance that has an `'a'` attribute, and has an href. Which would easily select our orders.

Another less beautiful, but very effective method is to simply select the td tags we see in the HTML, and then just get what is in between them using the `.string` argument in beautifulsoup.
```python
find_all('td')
```

### Let's get Started!

Now that we have a rough idea of ways we can extract the data, we can start writing code. First we will simply import our libraries we mentioned at the start.

```python
import re
from robobrowser import RoboBrowser
from bs4 import BeautifulSoup
import csv
from pathlib import Path
```

We will simply follow the documentation, and start by initializing our browser, and opening our webpage. We will then use the default RoboBrowswer parser. If that doesn't work we can use the BeautifulSoup parser.

```python
browser = RoboBrowser()

browser.open('https://market.fuzzwork.co.uk/hub/type/34/')

soup = browser.parsed
```

To check that this step works, you are going to want to print your soup. If you see a bunch of HTML which looks correct, you are good to go. If not, you might want to check whether the site could be blocking parsing attempts, or if you need to use a different parser. BeatifulSoup allows for using different methods including my favourite, lxml.

### Getting the data!

As we saw earlier in the HTML, we can see that the data we want is contained inside of individual td brackets. We will be using those to select the data. But in order to select the right data, we can also use the unique order ID values, which are easier to obtain since they are inside a 'a' and an href.

#### Obtaining the Order IDs

The cool thing about find_all is that we can treat it as a list. This means we can loop over it and extract information we want using conditional statements.

First we use `find_all` to get every piece of code that has an `a` and an `href` attribute.

We can then create an empty list, loop over our `find_all`, and if the value contains a specific string, in this case `'history'`, we add it to our new list.

One thing to watch out for, when selecting data inside of an HTML attribute such as href we have to use `[]` in BeautifulSoup. So for example here we have to use `el``['href']` to get the value of the href. We can then use the `.string` attribute to return the `order_id` which is contained inside the string.

```python
order_tag = soup.find_all('a', href=True)

order_id_list = []

for el in order_tag:
		if 'history' in el['href']:
				order_id_list.append(el.string)

print(order_id_list)
```
We should then obtain a nice list of order IDs:
```text
['5230717002', '5247053150', '5247122183', '5247106299', '5186490649', '5179015542',
	 '5244891224', '5238048141', '5246111732', '5246929196', '5227640542',
	  '5216357963', '5244643640','5202752240', '5240810598', '5239602499', ... 

```

#### Obtaining the rest of the data

Now we want to use these order IDs, to isolate the data we want to deal with.

First of all we can simply do what we did above, but instead of using find_all on `'a'` and `href`, we will use it on `td`. This is obviously going to return a ton of data we don't want, but utilizing our order IDs we can now isolate what we want.
```python
tag = soup.find_all('td')
print(tag)
```

![HTML printed out](/webscraping_robobrowser/html_printed.png)

So we can see that we have the data that we want, let's display only the strings.
```python
for i in tag:
    print(i.string)
```
We can see in pink are the order IDs, in red is the volume left, in blue is the price and in green is the station.
![Values we want from HTML](/webscraping_robobrowser/values_we_want.png)

So what we can simply do is make a loop where we skip the values we don't want. So what we could do is, loop over every value, check if the value is an order ID. (Since we have a list of the order IDs), and if it is, we can then check 2 values down for the volume, 4 values down for the price, and 6 values down for the station.
```python
for idx, el in enumerate(tag):
    if el.string in order_id_list:
        print(f"order ID is: {tag[idx].string}")
        print(f"Volume is: {tag[idx+2].string}")
        print(f"Price is: {tag[idx+4].string}")
        print(f"Station is: {tag[idx+6].string}")
```
![pretty print names](/webscraping_robobrowser/pretty_print_names.png)

### Writing the data to CSV
It's great that we've gotten this far, now we want to take this data and write it to csv. My favourite way of doing this is using dictionaries + pythons csv module that includes a `DictWriter`.

So firstly we want to create an empty dictionary before our loop, and instead of printing our values we want to assign them to our dictionary. We also want to fix them up, so for example for volume we just want the first half, and we want them as integers. For this I use simple Python string methods `.replace()`, `.split()` and type conversion using `int()`.

```python
export_dict = {}

for idx, el in enumerate(tag):
    if el.string in order_id_list:
        order_id = int(tag[idx].string)
        volume = int(tag[idx+2].string.split('/')[0].replace(',', ''))
        price = int(tag[idx+4].string.replace(',', '').replace('.', ''))/100
        region = tag[idx+6].string

        export_dict['Order ID'] = order_id
        export_dict['Volume'] = volume
        export_dict['Price'] = price
        export_dict['Location'] = region

print(export_dict)
```

Our dictionaries then will look like this:
```python
{'Order ID': 5205541456, 'Volume': 1,
 'Price': 1000000.0,
 'Location': 'Jita IV - Moon 4 - Caldari Navy Assembly Plant'}
```

So now our goal is to write to a CSV inside of our loop. You can copy my code or the code from the csv DictWriter documentation if you like, it's relatively simple but easy to forget.

Here is the code for this, I will explain it below:
```python
export_dict = {}

file_location = Path('C:/Users/username/Desktop/tritanium_data.csv')

for idx, el in enumerate(tag):
    if el.string in order_id_list:
        order_id = int(tag[idx].string)
        volume = int(tag[idx+2].string.split('/')[0].replace(',', ''))
        price = int(tag[idx+4].string.replace(',', '').replace('.', ''))/100
        region = tag[idx+6].string

        export_dict['Order ID'] = order_id
        export_dict['Volume'] = volume
        export_dict['Price'] = price
        export_dict['Location'] = region

        fieldnames = [*export_dict]

        if file_location.is_file() is True:
            write_head = False
        else:
            write_head = True

        with open('tritanium_data.csv', 'a', newline='') as csvfile:
            writer = csv.DictWriter(csvfile, fieldnames=fieldnames)

            if write_head is True:
                writer.writeheader()

            writer.writerow(export_dict)
```
There are a few moving pieces here in this loop. Firstly, we want a way of checking whether the file exists, and for this we use the `Pathlib` module, we tell it the path of the file including the CSV name, and then later on we run an if statement that checks whether the file exists. If it does not exist, we then put write_header = `True`, and write the header (column names.). This stops us from repeatedly writing the columns.

Our final result:
![Final Resulting CSV](/webscraping_robobrowser/our_csv.png)

I hope this "guide" was not too convoluted, and I might come back and fix it up a little bit later. Good luck with your parsing!
