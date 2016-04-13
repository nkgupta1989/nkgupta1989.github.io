---
layout: post
title: My First Experience in Mongo Tunning
---

Let me first explain the conditions in which we were, using mongodb, with around 60GB data, around 14 million records in a collections with an average cpm of 20K and a target to achieve average response time of 10ms.

In current world it is tough to find out the problem as compared to its fix, same was the case with us, We first needed to know the root cause for slowness and then fix it up.

Now mongo itself provides a hell lot of things, to find and figure out the problems and fix them.
These are the steps that we followed to achieve our target.

 __Step 1:__
  
 Check the value of "slowms" {mongo will log the quesries taking time more than this} in mongo.conf file and restart mongo :- ideally set it at 100ms
 
 
 __Step 2:__
  
 Now your slow query logs should have started, tail the log path provided in the mongo.conf file.
 
 
 __Step 3:__    
 
 You can run the below mentioned query on your database, It will provide you all the active queries taking time more than the specified time.
 
       db.currentOp(
          {
            "active" : true,
            "secs_running" : { "$gt" : 1 }
          }
       )
 
   Check a [sample response](../samples/sample1_current_op_response.json) json.
   
   This response will have a key running_time, which signifies the running time for that query. 
   Considering the fact that mongo entertains the read lock, even a single stuck query can exponentially slow the mongo performance. 
   
 __Step 4:__
    
  Manually kill those operations that are stuck.
 
    db.killOp({operation-id})
 
   {operation-id} can be obtained from the response of the currentOp query. 
 
 __Step 5:__  
 
 Now lets analyse the slow query logs:-
 We used "fluent-plugin-mongo-slow-query" to analyse the slow query mongo logs, Attached is the format of the output file.
 [fluent-plugin-mongo-slow-query](https://github.com/caosiyang/fluent-plugin-mongo-slow-query)
 Sample generated File:- TODO:- attach a sample output excel
    
 __Step 6:__
 
 Now we know the slow queries killing our mongodb, Use the required Indexes to fix them up.
 [Mongo Indexes docs](https://docs.mongodb.org/manual/indexes/)