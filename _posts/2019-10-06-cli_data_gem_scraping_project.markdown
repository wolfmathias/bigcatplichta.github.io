---
layout: post
title:      "CLI Data Gem Scraping Project"
date:       2019-10-06 22:16:38 -0400
permalink:  cli_data_gem_scraping_project
---


The base concept of this CLI project is the foundation for a larger project I plan on submitting as my final portfolio project, and a web app that will be a new functional fundraising method for The WildHeart Foundation. The concept boilds down to "A website where you can buy toys for zoo animals anywhere in the world."


The first requirement for the project is a data scraper using Nokogiri and Open URI. A scraper will most likely not be utilized in the final product, but was a fun challenge and an opportunity to display what I've learn about it.

While each instance of the 'Animal' class was generated dynamically by scraping a web page, the 'Toy' instances were created manually. In order to keep the program clean and relevant, I essentially hand picked each toy and its information. Websites for zoo animal enrichment are few while having an immense catalog of products, and many of those products have options such as size or color.

This project uses 5 separate objects that work off eachother: The CLI interface with a menu, a 'Donor' object for users, an 'Animal' object, and a 'Toy' object.  The final object being the scraper that pulled the example animal profile data for the program.

Once the index page of animals was scraped to gather names and profile URLs, that hash of information was iterated over to create a local variable `profile`. 

![](https://imgur.com/v5y4tti.png)

For each profile, an array of keys (attribute names) and values (paragraph information) were created by mapping over the CSS selectors `"h3"` and `"h3 = *"`, respectively.. While the paragraph information usually resided in a paragraph class on the profile page, a few profiles used unordered lists and list item for select info. Therefor, it was best to use the 'any class' selector of `*` and grab the sibling classes next to my `h3` classes.

Each item in the arrays were then assigned to each other to create a hash. After this, two keys (`:born` and `:sex`) were manually set as they needed to be extracted from a single string with the structure of "Meet Bart (m), born 2007".

![](https://imgur.com/JsaioRe.png)

This list of hashes is then sent to the 'Animal' object to create new instances of that class and set each attribute.

![](https://imgur.com/6w3SZ1T.png)
![](https://imgur.com/iXC3Nth.png)

A 'Donor' object created on initialization of the CLI interface. When a new ToyBrowser::CLI is instantiated, a user is prompted to enter their name. The name is then sent to the 'Donor' class method `find_or_create_by_name(name)`. This method's purpose is clearly stated, it will create a new instance of 'Donor' unless an instance with that name is already found, then this is returned and set as the CLI's `:user` attribute.

Once the user is set, a list of animal names and species is printed with a number next to each one. The user can select a number to display more information about that animal, or enter "donations" to see a list of items they have donated. "Donations" calls on a 'Donor' class method, `list_donations(donor)`.

When the user enters a number to select an animal, that 'Animal' object is stored in a local variable `current_animal`. The animal information page prints out basic information such as name, sex, birthdate, and classification information. A menu is displayed with more options such as Fun Facts, Habitat, Behavior, Diet, and Ecology. Below this menu are more options, 'donate' and 'display toys'.  

![](https://imgur.com/GEqCRP.png)

'Display toys' calls on a class method of the 'Animal' object, `toys_received(animal)`. Similar to the 'Donor' class method of `list_donations(donor)`. Each of these methods uses the 'Toy' class variable `@@donated_toys`. `@@donated_toys` is an array that has 'Toy' instances pushed into it when that specific instance attribute `:status` is set to "donated" (each instance is initialized with the attribute set to "waiting to be donated".

The 'donate' option in the menu lists all the instances of the 'Toy' class and is calling on a 'Donor' class method while passing it two arguments, the current animal and the toy selection. This method sets three of the 'Toy' instance attributes: `donated_to = animal`(the current animal), `donated_by = self` (the current user, which is an instance of 'Donor', and sets the `:status` attribute to "donated". `donate_again?` is a method which creates a new intance of the CLI interface, restarting the program.

![](https://imgur.com/Z2yM0xa.png)

In the above method, `toy.send("status=", "donated")` is calling on an instance method of the current toy. This method pushes the object into the `@@donated_toys` class variable and deletes it from `@@all`. Then, a new 'Toy' object is instantiated with identical attributes (other than "donated_to" and "donated_by") in order to keep the list of toys populated while still keeping track of toys that have been donated.

![](https://imgur.com/71L0uF9.png)

Thus prompting the user to either exit or restart the program.

This program has been an amazing learing experience, it has also helped expand the concept of the first functional program I plan on building. The program is also educational for anyone using it, since it displays plenty of facts about animals that the user may not have known.




