---
layout: post
title:  Ways to reduce the api response time.
---

Listing down some of the ways, through which we can cache the api response and make the api faster, and offload the load from the server.
 
 - CDN server caching (Akamai)
 - Varnish (using redis or memcache)
 - Using Redis or memcache or in-memory database to store main db result
 
 
 
Pre-requisites are :- 

  1. Basic knowledge of REST api's
  2. Basic knowledge of CDN servers
  3. Knowledge of any In-memory database

Lets take them bottom to top, and first discuss the approach to use an in-memory database. 

__1. Use in-memory database to cache the time taking static activities :__

Caching the DB queries response or of some external api's

Lets take an example to understand this, Suppose you have an api which takes the category id (cid) as the input parameter and returns the list of all the games along with there descriptions in the response.

We were quering the MYSQL table on the cid to get the list of the games for a particular category and send the response, As an improvement what can be done is to cache the response of this db query in some in-memory database like memcache, or Redis because this is a static data and the chances for it to change frequently are very less, we may have addition of the games in a particular category but that would be 1 in 6 hours or so.

BEFORE:- 

    dbResponse = Query the MySQL 
    Process the response 
    Send the response 


Now the steps will be first check the in-memory database for the result, if found then return it, else query the main db, store the result in the in-memory db, corresponding the key with this category-id, and then send the data.

AFTER:- 

    dbResponse = GET(from the inMemoryDB(cat_id) ) 
    if(!dbResponse){
        dbResponse = Query the MySQL
        if(dbResponse){
            Store the response in inMemoryDB
        }
    }
    Process the response 
    Send the response
    

Some Important points

  1. It is Faster to access an in-memory database then Query persistant dbs like MySQL or mongodb, because to access RAM is faster then accessing the main memory.
  2. Cache time is very important, it should be such that we will get the benefit from the caching and also save ourself from showing stale data to users.
  3. Properly monitor the hit to the miss ratio.
  4. Always prefix the cache key with some constant string, it will help you to flush out the cache without any problem.
  5. Use proper data type provided by redis to cache the data.
    
    
__2. Varnish :__

Varnish works on exactly the same principals with few points, it works at the web server level, means it will offload the request from landing at the app server level.
varnish cache the exact URL along with its params and cache its response for the specified time for the first request, and will serve the same request on its own for the rest of the times till it is cached in its cache.
Here is the link to configure the varnish on your server.
 https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-varnish-with-apache-on-ubuntu-12-04--3

     
__3. CDN servers :__

One of the most important, easy and reliable source to offload and speedup the static web requests.
Advantages

  1. CDN servers are geography distributed through out earth, and the content is served from the web server that is closest to the requester to give a speed boast by reducing the network latency.
  2. One very basic advantage of using CDN server is, it reduces the network hopes, between the user and webservers, as the CDN companies have direct tie-ups with the network operators, this gives boast of 30%, even without caching anything on CDN.
  3. Server all the static html, js, css files through CDN, why to waste app-server CPU cycles for serving static content.
  4. Cache the static api's like the one mentioned above at the CDN level to further give a boast in the api response time and user exp.
  5. Companies like Akamai, which is a giant in providing CDN services, provides horizontal caching on all the servers for a web request, and tries to get the response from the nearest CDN server if not found one the first CDN server, instead of directly hitting the web server. 
     
     

At the end I would like to say is, We should use any caching mechanism only after first understanding the capability of our web servers, and should develop with a super optimized backend data base modeling, and then use cache only to scale up.
