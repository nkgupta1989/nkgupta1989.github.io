---
layout: post
title: Best project practices
---

Listing down some good practises that we have used in our project.
 
 - TEST CASES
 - Automated Deployment
 - Monitoring tool
 - Production server setup script
 - Peer code review

 

__1. TEST CASES :__

Test cases are an integral part of coding, no developer can run from them.
It ensures that the functionality written stays the same as expected.

__Lets understand Why they are important :-__ 
This is a very basic example
Suppose there is a get api :- which returns the below mentioned json output when it was written.

    {
        "userName" :- "nkgupta1989",
        "firstName" :- "Nishant",
        "lastName" :- "Gupta",
        "DOB" :- "30-03-1989",
        "sendMails" :- true,
        "email" :- "nkgupta1989@gmail.com",
        .
        .
        .
    }

And this api is used by some client. Now there comes an additional requirement to add fullName key in the response, Now while adding, the developer can by mistake remove the original firstName and lastName keys from the response thinking that it can be read through the fullName.

But the older versions will still be using firstName and lastName keys, And if they are removed then the app will start crashing.

Now if there were assertion on firstName and lastName. Then the respective TC will fail after the change is done, which will send a message to the developer, informing the importance of these keys in the response.

    Sample assertion
        assert(data.firstName); // These will fail if the firstName key is not present in the api response
        assert(data.lastName);
        assert(data.email);

Some of the packages that can be used for writing TC's are 

    mocha for a nodejs project
    unittest python module for a python project

Please find below a screen shot of test case coverage report.

![alt text](http://nkgupta1989.github.io/images/test-case-coverage.png "Test case coverage report")


__2. Automated Deployment__ 

One way for automated deployment is tag based deployment. There are tools like circle ci, jenkins, which will listen for git tags, and a new build will be deployed when there is tag which satisfies certain conditions.
      
Let me explain how we are currently doing it in our project.


Any tag which is made from the master branch and matches with __deploy.{release-version}__  is considered for deployment by jenkins or circle ci.

Process of deployment is, firstly all the TC's are run on this tag, and if all are passed then the production deploy is started, else the build is stopped and a mail is triggered with the complete log traces of the TC's.
       
This way we are free to deploy 'n' numbers of times to the production enviornment through out the day, without any dependency, this increase the features release and increase the productivity of the team.
The most important thing to consider here are the TC's, as they are ensuring that nothing is broken in the release. 

Ideal projects have a TC's coverage of around __95-97%.__
         
    
__3. Monitoring tool__
 
Its always easy to solve a problem, then to figure out what the problem is, BUT there should be a way of acknowledging or alarming that there is a problem even before figuring out what the problem is.

As per me, NewRelic is one of the best monitoring tool, it not only reports the transactions, rpm, average response time, throughput, But also minutelly reports the time taken in each any every activity in a transaction like, time taken in mongo, node, redis, external service, request queuing etc.  This proves to be a great help in figuring out where the problem is.

![alt text](http://nkgupta1989.github.io/images/newrelic-screen-shot.png "Newrelic screenshot")

New relic screen shot, describing :- average response time of 51.6ms, average rpm 6.3K rpm and an apdex score of 0.93
 
We can set various alert conditions in Newrelic, like an alert can be set on the average response time of the app, like send an alert if the average response time is more than 50 ms. There could be an alert on the apdex score, like through an alert if the apdex score is less than 0.7 seconds.
    
Now NewRelic also supports custom metrices, through this what we can accomplish is put a log in new relic whenever an event happens on your application.

Suppose you want to monitor the transactions done on your website, and want the system to generate an alert whenever the numbers of transactions reduces then a certain number in a specified period of time. To accomplish this, just put this information in newrelic whenever a transaction happens on your system, and set the alert policies for this custom matrix, and get real time alert for any issue in transactions rather knowing it through customer complains through twitter or fb.
    
    
__4. Production server setup script__

Use cases:- Suddenly a production machine crashes, and you need to setup a new machine, with all the installations like nodejs, strongpm, mongo, redis, code deployment, nginx, host enteries etc. Another use case could be, you have a campaign coming up, and it is expected to have more 4X traffic growth and you need to have an instantaneous prod machine ready.
If you are on AWS than it can be directly done by creating an image of the machine and replicating it, or can be accomplish by auto scaling in AWS.

Now let us discuss how we managed to do it our project.

TODO:- comparission of different tools

We preferred ansible over other mentioned tools and we used ansible extensively to setup mongo servers with the required replication set, redis with cluster support, nodejs, deployment through strong-pm, nginx etc. After completing the scripts, what all was required, was to enter the new inventory in the inventory file, and run the ansible-playbook.
  
Writing these scripts will give a deep knowledge about setting up production machines and are very good for an individual knowledge growth.
    
Adding a very simple task below as reference, to create a log directory in the /var/log folder, with its owner and user group.
    
    - name: create directory for logs
      file: path=/var/log/app-log/ state=directory owner=deployer group=deployer
    
  
__5. Peer code review__

This is a very standard practise followed across many companies and is a must required one.

There are some very minor mistakes that can be easily avoided by code review, which further increases the quality of code.

Somethings that can be checked in code review are whether config's are defined properly or not, whether the code is modularized or not, extract those functions that are generic and can be used across the project and add them in the library files, removal of un-necessary if-else statements.






