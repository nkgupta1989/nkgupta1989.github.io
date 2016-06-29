---
layout: post
title: Running a web app on production boxes
---

In this blog, I will be explaining the architecture for a production level app, including the app servers (loopback node application), web servers (nginx), mongo (replica set), redis (M-S replica), ELB (if on AWS) along with the security groups, and data communications among them.

![alt text](http://nkgupta1989.github.io/images/aws-architecture-blog.jpg "Server level diagram")

Though the above diagram is very much self explaining. But listing down some important points here :-
 
 - https certificate is dropped while communication from the ELB to web servers (nginx) to avoid the decryption again and again.
 - nginx is used over app servers, as the connection handling for the nginx server is much better than the app servers.
 - All the app servers, are in one security group and only the required access is open on these machines. These servers can only be accessed on port 80 from outside world.
 - All the mongo servers are in one security group and only the servers in the app security group can connect to mongo servers on 27017 port.
 - All the redis servers are in one security group and only the servers in the app security group can connect to these redis servers on 6379 port.
 - Health check of a server is based upon a sample get url, like a simple count api.
 
         
         
 
 