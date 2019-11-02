---
layout: post
title:      "If you want to build something, you need to commit"
date:       2019-11-02 02:25:50 +0000
permalink:  if_you_want_to_build_something_you_need_to_commit
---


A big idea is really just a bunch of small ideas. You can have a goal in mind, but in order to make it happen you must first discover how.

To do that, I've found it's best to break everything down as much as I need to. Pick one of those things and see what it looks like as you build it out. As you learn, it will get easier and faster, but if you want to build you need to *commit*.

For the web app being built for the [WildHeart Foundation](https://www.thewildheartfoundation.org), we'll need a way for zookeepers to create an account and add simple profiles of animals under their care. Each animal will also have a collection of toys and enrichment that can be donated to them. These items are selected by the keeper when making the animal's profile. This ensures that animals receive appropriate toys when we randomly select the item from a list. After all, you don't want a meerkat getting something like a 36-inch Boomer Ball. There is also the fact that many zoos have restrictions on what their animals can have. Sometimes these restrictions are due to safety issues, or the zoo might only allow items that are made from natural materials. 

So, that's three separate models right there: Zookeepers, Animals, and Toys. Let's start with the database, each of these objects will need a table (we'll skip doing anything with Toys until the app is meeting all the requirements). The migration for each will look something like:

```
class CreateZookeepers < ActiveRecord::Migration
  def change
	   create_table :zookeepers do |t|
       t.string :first_name
       t.string :last_name
       t.string :email
       t.string :password_digest
     end
		end
end
```

Let's think about our associations. A zookeeper has many animals, and animals have many toys (or at least they will if this app is successful!). A zookeeper will then have many toys through their animals. An animal belongs to a zookeeper, at least for the purposes of this app.

*In the future, it might be a good idea to add another class, Zoo, which has many animals and many keepers. But then do the keepers have many animals, or are they simply associated with them? And technically, an animal has many keepers. A volunteer keeper may also have many zoos. Let's just work on what we have, remember this is really just a proof of concept.*


Here we have some finished controllers. I don't think even think this is half of them. Right now, they are all located in ZookeeperController, but we should separate some of them into their own controller since we have both `/account` controllers and `/account/animal` controllers. I'll separate the latter into their own controller that also inherits from ApplicationController. This will help keeping everything neat. 


![](![](http://https://imgur.com/a/n5Ln8wQ.jpg)

*A note on naming conventions here: The database and models were created with the idea of also having another type of user (a donor). This user would also have an account but not be able to create animals, but they will become related to existing animals as they fund toys. The routes were written with that in mind, but wasn't implemented in this build*



![](https://imgur.com/a/AoxipFX.png)

It's a little easier to read with two separate controllers. But there is a lot of repeated code here. Helper methods would make a lot of this shorter and cleaner.

Along with helpers to clean up repetitive code, I also have things like `if session[:id] == params[:id].to_i`. This checks that a user is logged in. It's stupid. Let's create a method called something like `logged_in?` to do that.

I also want to get rid of some of this repetive code. I'll still have a lot of the same things, but this could be more precise.

![](https://imgur.com/a/rP5fYuF)





