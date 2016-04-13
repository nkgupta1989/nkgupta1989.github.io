---
layout: post
title: My First Experience in Mongo Tunning
---

Let me first explain the conditions in which we were, using mongodb, with around 60GB data, around 14 million records in a collections with an average cpm of 20K and a target to achieve average response time of 10ms.

In current world it is tough to find out the problem as compared to its fix, same was the case with us, We first needed to know the root cause for slowness and then fix it up.

Now mongo itself provides a hell lot of things, to find and figure out the problems and fix them.
These are the steps that we followed to achieve our target.

 ##Step1: 
 Check the value of "slowms" {mongo will log the quesries taking time more than this} in mongo.conf file and restart mongo :- ideally set it at 100ms
 
 
 ##Step2: 
 Now your slow query logs should have started, check the log path provided in the mongo.conf file.
 
 
 ##Step3: 
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
   
 ##Step4: 
 Manually kill those operations that are stuck.
 
   db.killOp({operation-id})
 
   {operation-id} can be obtained from the response of the currentOp query. 
 
 ## Step5: 
 
 Now lets analyse the slow query logs:-
 We used "fluent-plugin-mongo-slow-query" to analyse the slow query mongo logs, Attached is the format of the output file.
 https://github.com/caosiyang/fluent-plugin-mongo-slow-query
 Sample generated File:- TODO:- attach a sample output excel
    
 ## Step6: 
 
 Now we know the problem, killing our mongodb (slow queries), Used the required Indexes to fix them up.
 https://docs.mongodb.org/manual/indexes/