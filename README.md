
# E-volve Retail

Climate change is now the most pressing and urgent problem facing the world today.  It is imperative that we move to a sustainable way of life before the consequences become irreversible.

The E-volve Retail store aims to be part of the solution by providing fully sustainable and beautiful clothing, beauty and home products for an increasingly ecologically aware customer base.



A live website can be found [here](https://e-volve-retail.herokuapp.com/).


 **September 1, 2021**

## Table of Contents

`Highlights in yellow`

_Make's the text italic_

1. Text 
2. *Also makes the text italic*
3. Text
4. Text
5. Text

------

```
Example of a
black background
```

---


1. [User Experience (UX)](#user-experience-UX)
    1. [Project Goals](#project-goals)
    2. [User Stories](#user-stories)
    3. [Color Scheme](#color-scheme)
    4. [Data Model](#data-model)
2. [Features](#features)
    1. [User Information Input](#user-information-input)
   
   
3. [Technologies Used](#technologies-used)
    1. [Language Used](#language-used)
    2. [Frameworks, Libraries and Programs Used](#frameworks-libraries-and-programs-used)
4. [Testing](#testing)
    1. [Testing User Stories](#testing-user-stories)
    2. [Code Validation](#code-validation)
    3. [Manual Testing](#manual-testing)
5. [Deployment](#deployment)
6. [Credits](#credits)
7. [Acknowledgements](#acknowledgements)

***

## User Experience (UX)

### Project Goals

* Provide an online retail website which is instantly recognisible as an e-commerce store selling products to the general public. 

* The site must be modern, pleasant to view, easy to navigate and provide all the ususal features customers expect to see on a retail website.


* Customers should be able to make secure credit card payments and be able to create an account in order to save details of orders and delivery addresses for use and viewing at a later date. 

* The site should be easily navigable and provide intuitive features to help the user find a specific product/s they may be interersted in.

* The site should be secure with only approved Admin Superusers able to make changes to products and orders.  Superusers should also have the ability to change another user's account details where required/authorised.

### User Stories

* As a user, I want to immediately understand the site's main function.

* As a shopper, I want to easily understand how to begin viewing products.

* As a shopper, I want to easily understand the navigation options available  and the types of products available.

* As a shopper, I want to be able to easily identify specific products I may be looking for (via a 'search' box for example). 

* As a shopper, I want to be able to list products on screen to better suit my search requirements (eg. sort by price or by category).

* As a shopper I want to be able to click into individual products to see more details and a larger picure of a product I'm interested in.

* As a shopper I want to be able to store my selections in a shopping trolley and be able to alter my selections as necessary.

* As a shopper I want clear notifications each time I add, remove or alter items in my shopping trolley.

* As a shopper I want to be able to make secure credit card payments for the items I've chosen including relavant delivery address details.

* As a returning shopper I want to be able to create my own account in order to save my personal and delivery details. 

* As a returning shopper, I want to be able to see details of previous orders I've placed.

* As a returning shopper with an account, I want my shopping bag to remain saved for as long as I stay logged in to my account.

* As an Admin Superuser I want to be able to view and make changes to products, orders or user account details as and when necessary by logging in via my Superuser account.

### Color Scheme

Here are the colors being used :


* Site-wide text, product prices and Navbar icons:

    Charcoal Grey   -  `rgb(55, 56, 56)`

* Default Background :

    White  -  `#ffffff`

* E-volve Logo :

    Terracotta Orange - `#c06e2c`

* 'Add to Basket' and 'Checkout' buttons :

    Green  -  `rgb(126, 170, 126);`


### Data Model

* Category Model

    - name = models.CharField(max_length=254) 
    - friendly_name = models.CharField(max_length=254, null=True, blank=True)

* Product Model
    - category = models.ForeignKey('Category', null=True, blank=True,
                                 on_delete=models.SET_NULL)
    - sku = models.CharField(max_length=254, null=True, blank=True)
    - name = models.CharField(max_length=254)
    - description = models.TextField()
    - has_sizes = models.BooleanField(default=False, null=True, blank=True)
    - price = models.DecimalField(max_digits=6, decimal_places=2)
    - image_url = models.URLField(max_length=1024, null=True, blank=True)
    - image = models.ImageField(null=True, blank=True)

* Order Model
    - order_number = models.CharField(max_length=32, null=False, editable=False)
    - user_profiles = models.ForeignKey(UserProfile, on_delete=models.SET_NULL,
                                      null=True, blank=True, related_name='orders')
    - full_name = models.CharField(max_length=50, null=False, blank=False)
    - email = models.EmailField(max_length=254, null=False, blank=False)
    - phone_number = models.CharField(max_length=20, null=False, blank=False)
    - country = CountryField(blank_label='Country *', null=False, blank=False)
    - postcode = models.CharField(max_length=20, null=True, blank=True)
    - town_or_city = models.CharField(max_length=40, null=False, blank=False)
    - street_address1 = models.CharField(max_length=80, null=False, blank=False)
    - street_address2 = models.CharField(max_length=80, null=True, blank=True)
    - county = models.CharField(max_length=80, null=True, blank=True)
    - date = models.DateTimeField(auto_now_add=True)
    - delivery_cost = models.DecimalField(max_digits=6, decimal_places=2,
                                        null=False, default=0)
    - order_total = models.DecimalField(max_digits=10, decimal_places=2,
                                      null=False, default=0)
    - grand_total = models.DecimalField(max_digits=10, decimal_places=2,
                                      null=False, default=0)
    - original_shopping_bag = models.TextField(null=False, blank=False, default='')
    - stripe_pid = models.CharField(max_length=254, null=False, blank=False, default='')
* Order Line-Item Model (used to create an order line for each item in a given order)

    - order = models.ForeignKey(Order, null=False, blank=False,
                              on_delete=models.CASCADE,
                              related_name='lineitems')
    - product = models.ForeignKey(Product, null=False, blank=False,
                                on_delete=models.CASCADE)
    - product_size = models.CharField(max_length=2,
                                    choices=SIZE_CHOICES, null=True,
                                    blank=True)  `'SIZE_CHOICES'` variable derived from `'SIZE_CHOICES'` selection listed below.
    - quantity = models.IntegerField(null=False, blank=False, default=0)
    - lineitem_total = models.DecimalField(max_digits=6, decimal_places=2,
                                         null=False, blank=False,
                                         editable=False)

    - `SIZE_CHOICES`= [
        (SIZE_6, 'Size 6'),
        (SIZE_8, 'Size 8'),
        (SIZE_10, 'Size 10'),
        (SIZE_12, 'Size 12'),
        (SIZE_14, 'Size 14'),
        (SIZE_16, 'Size 16'),
        (SIZE_18, 'Size 18'),
        (SIZE_20, 'Size 20'),
    ]   `'SIZE'` variable derived from the `'SIZE'` selection listed below.

    - SIZE_6 = "6"
    SIZE_8 = "8"
    SIZE_10 = "10"
    SIZE_12 = "12"
    SIZE_14 = "14"
    SIZE_16 = "16"
    SIZE_18 = "18"
    SIZE_20 = "20"

* User Profile Model
    - user = models.OneToOneField(User, on_delete=models.CASCADE)
    - default_phone_number = models.CharField(max_length=20, null=True, blank=True)
    - default_street_address1 = models.CharField(max_length=80, null=True, blank=True)
    - default_street_address2 = models.CharField(max_length=80, null=True, blank=True)
    - default_town_or_city = models.CharField(max_length=40, null=True, blank=True)
    - default_county = models.CharField(max_length=80, null=True, blank=True)
    - default_postcode = models.CharField(max_length=20, null=True, blank=True)
    - default_country = CountryField(blank_label='Country', null=True, blank=True)


## Features

### Home Page 'Carousel' imagery

 - Carousel of three eye-catching product images, each with a simple headline message to educate the user as to the websites main function and unique selling point (USP).
 - Each image has a 'Shop Now' button to prompt the user to begin browsing.  The buttons take the user to the Department relevant to the image on screen.
- The Carousel also has left and right arrows to enable the user to manually scroll left or right should they so choose.


### Navbar

- A Navbar is fix to the top of the page in order to provide the user with browsing options during the visit.
- On smaller screens the Navbar features:
    - At top right : 
        - Search Icon, 
        - User/Account icon (visible on tablet screens but not on mobile phones)
        - Shopping Bag icon
    -  At top left : 
        - a drop-down 'burger' toggle which includes links to : 
            * The main Product Departments pages
            * User Login / Register / Account Page options
            * Product Review page
    - Top Centre :
        - A Logo / Home link to take the User back to the Carousel home page.

    
- On larger screens the central Navbar area comprises 
    - At top right :
        - Search Icon, 
        - User/Account icon
        - Shopping Bag icon

    - At top left : 
        - The 'Home / Logo' link to return the User to the Carousel

    - At top centre :

        - Links to the three main store departments. plus an additional link to display 'All Products'.  
 
            Each of the four links include dropdowns when hovering. These allow the user to pre-sort the results, should the user choose to. The sort selection includes :
            - By Price
            - By Name
            - By Category (option only displayed for the 'All' products link) 

    
### 'Search' function

- A 'Search Box' (large screens) or 'Search Icon' (small screens) is provided to enable users to search for a keyword located in either in the Product Name or Product Details.  If the keyword is found - the relevant products are displayed/listed on screen.

### Products Page

- After clicking a Department / Category link (Fashion, Home, Beauty or All), or after successully searching for a product using the 'Search' function, the relevant products are listed on the 'Products' page.

-  Items are listed in columns dependent upon screen size.  Each listed product includes the :
    - Product Image 
    - Product Name
    - Product Price 
    - Product Category/Department link

- Clicking a product directs the user to the 'Product Detail' page for the item in question.
- Clicking the product's Category/Deprtment link prompts the site to display all products for the Department concerned.


### Products Page 'Sort' Function

- Items listed on the 'Products' page can be sorted using a number of pre-set options within the 'Sort by' box, located at top middle on small screens and top right on larger screens.

    Options for sorting include:

    - Price (high to low)
    - Price (low to high)
    - Name (A-Z)
    - Name (Z-A)
    - Category (A-Z)
    - Category (Z-A)

### Product Detail Page

Clicking on a product image, name or price takes the user to the 'Product Detail' page. A larger image of the selected product is displayed along with a product description and the product's price.  

Here the user can select to add the product to their shopping bag, or write a review if they so wish.  The following options are provided:

- 'Choose size' box (user selects from a pre-defined dropdown list of sizes).  Note: appears only when the product in question has a range of sizes to choose from.
- 'Enter Quantity' box (user types in the quantity of the item they wish to purchase).
- 'Add To Bag'
- 'Write a Review'


### Shopping Bag

Accessed by clicking the 'Shopping Bag' icon at top right of the screen, this page lists all items currently selected for purchase by the user.

The top of the page details:
- the page title including the total quanity of items currently selected.
- total value of the items selected.
- delivery charges, if any (currently, orders over £50.00 have free delivery)
- total value of the order including delivery costs
- value still required to be spent to gain free delivery.
- a green 'Checkout' button enabling the user to pay for the items selected.

Beneath this section the page lists each of the selected items in turn, detailing:
- a product image
- the product name
- price of the item
- quantity chosen
    - Can be amended by the user should the original quantity selected be incorrect.
- 'Update Quantity' button  (highlighted in green)
    - Altering the quantity and then clicking this button enables the user to amend the quantity required. 
    - Once amended the shopping bag total is recalculated accordingly.
- 'Remove Item' button (highlighted in red)
    - Clicking the button removes the item permanently from the shopping bag.


### Shopping Bag 'Pop Up Notification'

Following an addition or change to the shopping bag, a pop-up notification appears at top right of the screen beneath the Shopping Bag item.

The notification : 
- displays a green header and a 'Success!' message.
- confirms the change in writing for the user.  
- displays the number of items currently in the shopping bag.
- lists the quantity and size (if relevant) of each item.
- allows the user scroll down to view the list of items.
- confirms the total value of the shopping bag including delivery.
- confirms the amount still required to be spent before free delivery is acquired (if appropriate).
- displays a link inviting the user to 'Go to Secure Checkout'. Clicking this link takes the user to the Shopping Page'.  Here the green 'Checkout' button takes the user to the 'Checkout Page'.


### Checkout Page

The 'Checkout Page' allows the user to securely pay for the chosen items.

The top of the page confirms the items to be purchased, including:
- Confirmation of the number of items within the current order.
- A list of the items in the order including for each item:
    - an image
    - the size of the item (if relevant)
    - the quantity ordered
- Order total
- Delivery cost
- Grand Total including delivery

The bottom section of the page consists of a 'Payment Form' within which the user must add their :
- name 
- delivery address 
- credit card details.  

Plus :

- a button to 'Adjust the Shopping Bag' if required.
- a button to 'Complete Order' once the user is ready to make payment.
- confirmation of the value of the order including delivery.


###  Order Confirmation Page

The Order Confirmation Page confirms that:
- payment was successful via a 'pop-up confirmation' and, 
- confirms that an email will be sent to the users email address. 

Also listed are details of the order,  including:
- Order Number.
- Order date and time.
- Items purchased, including quantity and value each.
- Delivery address details
- Order and delivery totals

The bottom of the confirmation includes a link to enable the user to continue shopping should they choose to.

## Technologies Used

### Languages Used

* [Python3](https://en.wikipedia.org/wiki/Python_(programming_language))
* [HTML5](https://developer.mozilla.org/en-US/docs/Glossary/HTML5)
* [CSS3](https://en.wikipedia.org/wiki/CSS)
* [JavaScript](https://en.wikipedia.org/wiki/JavaScript)


### Frameworks, Libraries and Programs Used

* [GitPod](https://gitpod.io/) was used for writing code, committing, and then pushing to GitHub.

* [GitHub](https://github.com/) was used to store the project after pushing.

* [Heroku](https://id.heroku.com/) was used to deploy the application.

* [PEP8 online check](http://pep8online.com/) was used to validate the Python code.
* [Bootstrap.com](https://getbootstrap.com/docs/4.6/getting-started/introduction/) helped provide structure, styling and responsiveness to various viewports. 
* [jQuery](https://en.wikipedia.org/wiki/JQuery) helped simplify the JavaScript coding.
* [Django](https://en.wikipedia.org/wiki/Django_(web_framework)) web framework was used to provide pre-built structure and functionality to the Python code.
* [pip3 / Python Package Index](https://pypi.org/) provided open-source Python software packages.
* [Google Fonts](https://fonts.google.com/) provided the 'Exo' and 'Lato' fonts utilised within the site.

* [Font Awesome](https://fontawesome.com/) provided the various icons used within the site.










   


