# txdeathrow_check - A quickie count of Texas Death Row using HTML parsing

This assignment is a quick review of how to use BeautifulSoup, the HTML parsing library, to parse data from a government website and do a simple count. 

This is an light intro to what is usually just one big convoluted assignment mixing web-scraping techniques with trying to understand the justice system. And also, learning to deal with data-entry errors.



<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [Requirements](#requirements)
  - [Example usage](#example-usage)
  - [Functional requirements](#functional-requirements)
- [Setup and getting started](#setup-and-getting-started)
  - [From the command-line](#from-the-command-line)
  - [Running the tests](#running-the-tests)
- [Walkthrough/Guide](#walkthroughguide)
- [Background](#background)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->




## Overview

The website is the [Death Row Information Homepage of the Texas Department of Criminal Justice](https://www.tdcj.state.tx.us/death_row/), specifically, the [Offenders on Death Row page](https://www.tdcj.state.tx.us/death_row/dr_offenders_on_dr.html).

For the purposes of this exercise, we'll be using a mirror of the Texas site (so that our programmatic results are predictable)

https://wgetsnaps.github.io/tdcj-state-tx-us-2018/death_row/dr_offenders_on_dr.html

This assignment comes with a file already written for you: [data_helper.py](data_helper.py)

You have to fill out 2 functions that are in a file named [checker.py](skeleton.checker.py). But [data_helper.py](data_helper.py) does the boring work, you just have to know how to use BeautifulSoup to turn HTML on the Texas death penalty page into something a Python analysis program can use.



## Requirements

### Example usage

For `checker.py`, if you've completed the `get_and_parse_inmate_rows()` function, then the `count_inmates()` function can be used like this:


```py
from checker import count_inmates
n = count_inmates()
print("There are currently", n, "inmates listed on Texas's death row.")
```

### Functional requirements

You have to fill out the 2 function definitions in [checker.py](skeleton.checker.py). 

The [data_helper.py](data_helper.py) file is provided to you -- it basically just takes care of downloading from the webpage if needed. The only function you need to use from `data_helper` is `get_html()`, which returns the raw text of the death penalty webpage.

The `checker.get_and_parse_inmate_rows()` function should call `get_html()`, and then use BeautifulSoup to process the raw HTML. Visit the webpage via your web browser (use View Source too), as it's the quickest way to identify the HTML structure to use BeautifulSoup's `.select()` method:

https://wgetsnaps.github.io/tdcj-state-tx-us-2018/death_row/dr_offenders_on_dr.html

If you use `.select()` properly, you get a list of data (table rows) for the inmates. That makes the `checker.count_inmates()` function basically a one-liner (remember what Python function you can use to count things in a `list`?)



## Setup and getting started

<a name="cli-setup" id="cli-setup"></a>

### From the command-line

Assuming that you are in your system shell (Terminal/PowerShell), here's how to use the command-line to download and copy the skeleton and test files quickly to your current working directory (i.e. make sure you've used `cd` to get to where you want):


**Mac OS/Linux:**

```sh
$ curl -O \
      https://compciv.github.io/homeworkhome/txdeathrow_check/data_helper.py

$ curl -O \
      https://compciv.github.io/homeworkhome/txdeathrow_check/test_checker.py

$ curl -o checker.py \
    https://compciv.github.io/homeworkhome/txdeathrow_check/skeleton.checker.py

```

**Windows:**

```
$ curl.exe -O `
      https://compciv.github.io/homeworkhome/txdeathrow_check/data_helper.py

$ curl.exe -o checker.py `
    https://compciv.github.io/homeworkhome/txdeathrow_check/skeleton.checker.py

$ curl.exe -O `
      https://compciv.github.io/homeworkhome/txdeathrow_check/test_checker.py
```


### Running the tests

The tests are in a file named [test_checker.py](test_checker.py). Use `pytest` at the command line to run them:

```sh
$ pytest
```


## Walkthrough/Guide

Think of this assignment as a refresher/practice on HTML parsing with BeautifulSoup. 

I've written out [data_helper.py](data_helper.py) so that experimenting with the HTML for this assignment is as easy as going into iPython and doing this:

```py
>>>> from data_helper import get_html
>>>> txt = get_html()
```

You may see some messages about data/files being downloaded -- that's what `data_helper` does: the annoying work of getting and organizing the relevant data. All you need to worry about is using BeautifulSoup to parse that raw text:


```py
>>>> from bs4 import BeautifulSoup
>>>> soup = BeautifulSoup(txt, 'lxml')
```

Use your browser's [Web inspector and View Source tools](http://www.compjour.org/tutorials/elements-of-a-webpage/) to see the raw HTML text that represents the table of inmates:

![image screenshot-web-panel-death-row-table-row-html.png](images/screenshot-web-panel-death-row-table-row-html.png)

### What to `select()`?

If viewing the HTML doesn't give enough hints, nothing wrong with trying out some tags that you see. 

For example, in the above screenshot, there are a lot of `<td>` elements. Maybe those represent inmate data? Trying doesn't hurt:

```py
>>>> from bs4 import BeautifulSoup
>>>> from data_helper import get_html
>>>> soup = BeautifulSoup(get_html(), 'lxml')
>>>> tags = soup.select('td')
```

Easiest thing to try is to count the results:

```py
>>>> len(tags)
2320
```

**2,320** death row inmates seems excessive. Iterate through a few of the things in `tags` (remember, it's a list), just to see what you have:

```py
>>>> tags[0]
<td>999608</td>
>>>> tags[1]
<td align="center"><a href="dr_info/hudsonwilliam.html" title="Offender Information for William Hudson">Offender Information</a></td>
>>>> tags[2]
<td>Hudson</td>
>>>> tags[3]
<td>William</td>
```

It's certainly not nothing. Take a look at the HTML again and see the next HTML element that *encloses* the groups of `<td>` tags.



## Background

Past assignments have tried to do a lot of things at once:

[Web-scraping the Texas Executed Offenders List](http://2017.compciv.org/syllabus/assignments/homework/texas-executed-scrape.html)

This one you just have to think about using BeautifulSoup on the HTML.








