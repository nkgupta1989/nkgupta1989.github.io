---
layout: post
title: Why Nodejs
---

Listing down reasons for anyday choosing Nodejs over any other programming language.
 
 - Its lightening Fast
 - Single Event loop
 - Package Manager (NPM)
 - Async behavior (Additional speed boast)
 - At last its javascript (Will help in Mongo shell too)



__1. Its lightening Fast :__

Nodejs use google chrome v8 engine at the server level, which directly converts the nodejs code to assembly language making it super fast.


__2. Single Event loop__

This is an amazing feature of nodejs, nodejs runs in a single thread per core, and does not waits for any external read or write, or any child routine.

It is event loop is a single thread, that performs all the external operations asynchronously, not like the traditional languages like PHP or python, which does external operations either blocking way, or by forking a new process consuming unnecessary memory.


__3. Package Manager (NPM)__

Its npm package manager is very fast and consistant, and its very easy at describing and installing dependencies.

__4. Async behavior (Additional speed boast)__

Due to the async behavior of nodejs, we can execute non related tasks asynchronously, 
Like suppose, we have to make 2 independent select queries from the database, and first one takes 20ms, and second one takes 30ms, then in traditional systems we dont have a choose but to execute them synchronously one by one, and giving the api output in 20+30ms which is 50ms, but what you can do in nodejs, is fire both the queries parallely, and combined the response when you gets the output of both the queries, in this case you can return the output in 30ms, flat saving of 20ms in the api response. 


__5. At last its javascript (Will help in Mongo shell too)__

Its javascript only, every developer has once worked on it, more over nodejs, and mongo combination is irresitable for many projects, and knowledge of javascript will be a super boon in the mongo shell too.



