# The Moron, Numbskull or Abject Nincompoop's Guide to Web Scraping

Apologies, I can never resist a "for dummies" reference. 
So what's web scraping? Using a script, you request a web page and pull interesting
information out of it.

How is it done? Personally I use the Python programming language 
because I know it and it has tools which support scraping but any language will do. 

Usually I scrape one of two ways: for a simple page without
Javascript, which are virtually non-existent nowadays, you could just 
use the built-in `requests` module to make a network request for the web page content and then `BeautifulSoup` or `lxml` to 
load the page as a bunch of node objects so it's easier to grab the data. 
If anyone is interested in knowing more about that, hit me up!

Below is my preferred method for pulling information from complex websites.
I do this on a Mac with [PyCharm](https://www.jetbrains.com/pycharm/) as the IDE (app that helps me write code) but it
would work on any operating system (Windows, Linux). 

## Setup
- Install the [Chrome browser](https://www.google.com/chrome/)
- Install [chromedriver](https://chromedriver.chromium.org/getting-started) (which lets you control Chrome using a helper library) through the binary on that site.

### Setup Python env and install Selenium  (macOS)
- Go to the `Terminal` app and make sure you have Python installed by typing `python` at the prompt. `Ctrl+D` will exit the Python interpreter.
- Make a virtual environment so all the libraries which will be installed are put in one nice spot: `python3 -m venv venv` ([more on virtual environments](https://docs.python.org/3/library/venv.html))
- Activate the environment with `source ./venv/bin/activate`
- Type `pip install selenium` and hit Enter

### Use the headless browser

Okay, if you did all that we're basically done! Now I generally pop into PyCharm, create a project 
and then make a new Python file, but you can just use vim from the command line, Sublime, even TextEdit. 
We just need any text editor.

- Create a file with whatever name you want that ends in `.py`. Mine is called `scraper.py`. Boring!
- Import libraries and set up browser options:

```
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

options = Options()
options.add_argument("--headless")
options.add_argument(f"window-size=4000x4000")
```

Now in Python, you can put all these commands in classes, or functions,
and you can make sure your code only runs when the script is called directly
by using `if __name__ == "__main__"` but we've got lives to live here so just 
paste it all in at the top and the Python interpreter will read it line by line. This is test code; it is morally obligated to look like crap.

Notice I made a really huge window; this is because I don't want responsive
sites to load the mobile version of the site.

BTW selenium has hella good [docs](https://www.selenium.dev/documentation/en/). Python specific ones [here](https://selenium-python.readthedocs.io),
but I like the main ones and barely ever use the Python ones.

## Let's scrape!

So let's scrape something. Scraping always starts with two questions: where is
the webpage with the data that I want, and what HTML nodes have it in the page?

I'm kind of head-over-heels in love with [Paiko](https://www.paikohawaii.com/), this little Kaka'ako flower
shop in SALT. Let's find the prices of their insanely expensive arrangements. First I went
to Safari (could be Chrome, doesn't matter) and loaded their site and clicked on BOTANICAL SHOP. 
Then I right-clicked one of the pretty flower pics and chose `Inspect Element`. 

Now we're looking at the HTML source of the page. Enough < and > to make your damn head spin. 
We can hover over elements in the lower pane and they'll be highlighted in the page. 

So we can see that every piece of text below the image is in a `div` with a class of `product-meta`. Well yippee,
that just what we wanted. We also want the price and the name, which are below the image in 
div tags with classes `product-title` and `product-price`. This could not be easier. Good job Paiko dev!

So now onto the code...

```
def cls_els(el, cls_name):
    return el.find_elements_by_css_selector(f"*[class='{cls_name}']")
    
def txt(el, selector=None, cls=None):
    sel = selector if cls is None else f"*[class='{cls}']"
    return el.find_element_by_css_selector(sel).text
    
with webdriver.Chrome("/usr/local/bin/chromedriver", options=options) as driver:
    driver.get("https://www.paikohawaii.com/botanical-shop")
    prices = []
    products = WebDriverWait(driver, timeout=5).until(lambda d: cls_els(d, 'product-meta'))
    for product in products:
        name = txt(product, cls='product-title')
        price = txt(product, cls='product-price')
        prices.append({"name" : name, "price" : price })
```

What is all this mumbo jumbo? The first two functions are helpers because I find 
Selenium's naming soooooooo verbose and I loathe typing `css_selector`. These functions
help me take one or more Selenium `WebElement` objects and pull text out of them. As an example, the `txt` function will pull "from 85.00" out of the element `<div class="product-price">
from <span class="sqs-money-native">85.00</span></div>`. 

The meat of the work is done below the `with` statement. Using `with` means that 
chromedriver will take care of cleaning up the headless browser in the event of an 
error or when the script completes, so that ports and processes aren't accidentally left
running. This statement is often used to open and read from files in Python.

Once we've created the web driver, We'll load the page with the products that the we looked up earlier.
Web loading is asynchronous and lots of elements might be rendering at different intervals (images, ads, text) 
so it can be hard to know when a webpage is truly loaded. Instead, we ask the web driver to
wait until the elements we're interested in exist on the page. That's the whole `WebDriverWait` bit. 
Basically I'm saying: wait until you are able to find some elements with a class of product-meta and then 
return them to me, but don't wait longer than 5 seconds; throw an error if it goes beyond that.

Once we have a list of product WebElements, it's a simple thing to fetch the text values from the child elements.
The text property will return a string with all the text in the child elements. I take that and build a little dictionary
of information per product, and then add that dictionary to a Python list. This is my usual way of doing things when scraping;
you could make classes for each thing you're scraping, but this way is simple.

## CSS class syntax

Note that when I want to pull data from an HTML element, I'm using special punctuation to identify bits and bobs of those elements. 

For example, if the element is `<p id="main_flower" class="flower">Rose</p>`, I could grab the element by its id with `p#main_flower` 
or use the class via `p.flower`. This is CSS locator syntax. A dot means class, a pound sign means id, etc, etc. Asterik means "an element with any name", 
so `*[class="flower"]` would return all elements with a class of flower: p tags, div tags, a tags, anything. 
The full spec is [rather extensive](http://www.w3.org/TR/css3-selectors/) but in practice you need to know basically just those two, 90% of the time. [Here](https://www.seleniumeasy.com/selenium-tutorials/css-selectors-tutorial-for-selenium-with-examples) are 
a bunch of good examples.

## Figuring out wtf is wrong

Since you can't actually see what the headless browser is seeing, it can be obnoxious to debug problems. 

Here are my best tips:
- `driver.save_screenshot('filename.png')` is your best friend. It does just what it says; takes a screenshot of the currently
displayed web page and saves it locally so you can see what selenium is seeing.
- There are sort of two ways to use a headless browser: you can navigate through the site by clicking buttons 
and using `element.send_keys("some text")` on input fields to act like a real user,
or you can find the deepest page possible and look at the URL to see how to get to 
other pages you might want. For instance, when scraping Target, I could have clicked the 
"Next" button to get more search results, or I could have noticed, for instance, that there was a `page_index` query parameter
in the URL and just incremented that and fetched the next page until I got a 404.
- Use the debugger: breakpoint right after you've fetched the elements and print out their values or HTML.

## So we have the data, now what?
 
The easy thing to do is write it to a CSV like this (a more organized person would write a header before writing the values lol):

```
import csv
with open('prices.csv','w') as f:
    writer = csv.writer(f)
    for price in prices:
        writer.writerow(list(price.values()))
```

The [csv](https://docs.python.org/3/library/csv.html) library is built into Python, no installs needed. It just takes a list of strings 
and writes them to a file in a comma-separated format, with newlines between each row. 

## Non sequitur

Scraping can be massively confusing and frustrating. Timeouts, hidden elements,
iframes, the list is endless. But the code is tiny and easy. My approach to programming is not 
"this should work why isn't it" but "holy shit the duct tape and twine I put this together with is actually briefly functional!!" 
So when a script runs successfully, grab yourself a beer
and reflect on your triumph against the chaos of our times :)