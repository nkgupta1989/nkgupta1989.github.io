---
layout: post
title:  Reviews and Ratings backend using Loopback.
---

__Objective__ :- To have a working backend with required api's for a stand alone authenticated reviews and ratings system.

 - Programming language (nodejs)
 - Framework strongloop ( [strongloop](https://strongloop.com/) )
 - Database (in-memory)
 
  
__Pre-requisites are__ :- 

  1. Basic knowledge of REST api's only.
  2. System with nodejs installed on it.

__Github Repo__
"Github-repo-for-this-app" :-
[Github-repo-for-this-app](https://github.com/nkgupta1989/ratingsAndReviews)


__Model's list__
 
  1. HotelReview
  2. Images
  3. User 
  
__Models Relation ship__ :-

![alt text](http://nkgupta1989.github.io/images/reviewsAndRatings.png "reviews and ratings model diagram")

 
__Steps 1. Install strongloop node module globally using npm:__

    npm install -g strongloop
    
__Steps 2. Create a new loopback app using slc loopback:__

    slc loopback

__Steps 3. Create a new loopback models app using slc loopback:model :__

You have to create the 3 models mentioned above, hotel reviews (persisted model), images (persisted models) and reviewer (User) and have them as public, and add the required properties in each model as per the requirements, like title in hotelreview, url in image model.

    slc loopback:model

__Steps 4. Setup relations between the loopback models:__

Now you have to setup the relations between each models as per the Models relation ship diagram.

    slc loopback:relation

__Steps 5. Run the node app using :__

    node .
     
__Steps 6. check the api's documentation at http://0.0.0.0:3000/explorer/ :__

It will look something like this.
![alt text](http://nkgupta1989.github.io/images/image1.png "main pic")
![alt text](http://nkgupta1989.github.io/images/image2.png "hotelreview image")
![alt text](http://nkgupta1989.github.io/images/image3.png "image api's image'")

"Github-repo-for-this-app" :-
[Github-repo-for-this-app](https://github.com/nkgupta1989/ratingsAndReviews)
