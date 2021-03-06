# PyPartPicker

#### PyPartPicker is a package that allows you to obtain information from PCPartPicker quickly and easily, with data being returned via objects with numerous attributes.

---
### Features:
- Obtain Part objects, estimated wattage and total cost from PCPartPicker lists.
- Obtain name, product URl, price, product type, image and more from Part objects.
- Obtain everything previously mentioned + specs, reviews and in depth pricing information from PCPartPicker product links

---

# Installation
Installation via pip:
```
>>> pip install pypartpicker
```
Or clone the repo directly:
```
>>> git clone https://github.com/QuaKe8782/pypartpicker.git
```
---
# Basic programs
Here is a program that searches for i7's, prints every result, then gets the first result and prints its specs.
```python
from pypartpicker import part_search, fetch_product

# returns a list of Part objects we can iterate through
parts = part_search("i7")

# iterates through every part object
for part in parts:
    # prints the name of the part
    print(part.name)

# gets the first product and fetches its URL
first_product_url = parts[0].url
# gets the Product object for the item
product = fetch_product(first_product_url)
# prints the product's specs using the specs attribute
print(product.specs)
```
Here is another program that finds i3s that are cheaper than or equal to £110, prints their specs and then prints the first review:
```python
from pypartpicker import part_search, fetch_product
from time import sleep

# returns a list of Part objects we can iterate through
# the region is set to "uk" so that we get prices in GBP
parts = part_search("i3", region="uk")

# iterates through the parts
for part in parts:
    # checks if the price is lower than 110
    if float(part.price.strip("£")) <= 110:
        print(f"I found a valid product: {part.name}")
        print(f"Here is the link: {part.url}")
        # gets the product object for the parts
        product = fetch_product(part.url)
        print(product.specs)
        # makes sure the product has reviews
        if product.reviews != None:
            # gets the first review
            review = product.reviews[0]
            print(f"Posted by {review.author}: {review.content}")
            print(f"They rated this product {review.rating}/5.")
        else:
            print("There are no reviews on this product!")
            
    # slows down the program so as not to spam PCPartPicker and potentially get IP banned
    sleep(3)
```

---
# Methods

###`part_search(search_term, limit=20, region=None)`
#### Returns Part objects using PCPartPicker's search function.
### **Parameters**
- **search_term** ( [str](https://docs.python.org/3/library/stdtypes.html#str) ) - The term you want to search for.
  
    Example: `"i5"`
  

- **limit** (Optional[ [int](https://docs.python.org/3/library/functions.html#int) ]) - The amount of results that you want to be returned.
    
    Example: `limit=5`
  

- **region** (Optional[ [str](https://docs.python.org/3/library/stdtypes.html#str) ]) - The country code of which you want the pricing for the Part objects to be in. 
    Example: `region="uk"`
  
### Returns
A list of Part objects corresponding to the results on PCPartPicker.


---

###`fetch_product(product_url)`
#### Returns a Product object from a PCPartPicker product URL.
### **Parameters**
- **product_url** ( [str](https://docs.python.org/3/library/stdtypes.html#str) ) - The product URL for the product you want to search for.
  
    Example: `"https://pcpartpicker.com/product/9nm323/amd-ryzen-5-3600-36-thz-6-core-processor-100-100000031box"`

### Returns
A Product object for the part

---
###`fetch_list(list_url)`
#### Returns a PCPPLIst object from a PCPartPicker list URL.
### **Parameters**
- **list_url** ( [str](https://docs.python.org/3/library/stdtypes.html#str) ) - The URL for the parts list.
  
    Example: `"https://pcpartpicker.com/list/Tzx22V"`

### Returns
A PCPPList object for the list.

# Objects

---
## Part
### Attributes
- `name` - The name of the part
- `url` - The product URL for the part
- `type` - The type of the part (e.g. CPU, Motherboard, Memory)
- `price` - The cheapest price for the part
- `image` - The image link for the part

---

## Product
### Attributes

- All attributes from Part
- `specs` - The specs for the product, arranged in a dictionary with the values being lists
- `price_list` - A list of Price objects containing all the merchants, prices and stock values
- `rating` - The total user ratings and average score for the product
- `reviews` - A list of Review objects for the product
- `compatible_parts` - A list of tuples containing the compatible parts links for the product

___

## Price
### Attributes

- `value` - The final price value
- `base_value` - The price value before shipping and other discounts
- `seller` - The name of the company selling the part
- `seller_icon` - The icon URL for the company selling the part
- `url` - The URL which links to the seller's product listing
- `in_stock` - A boolean of whether the product is in stock

---

## Review
### Attributes

- `author` - The name of the person who posted the review
- `author_url` - The URL which points to the author's profile
- `author_icon` - The icon URL of the person who posted the review
- `points` - The amount of upvotes on the review
- `created_at` - How long ago the review was sent
- `rating` - The star rating out of 5 of the product that the review gives
- `content` - The text content of the review