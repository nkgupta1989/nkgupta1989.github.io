---
layout: post
title: Tips for On Premises to AWS Migration
date: 2016-06-29 00:00:00
---

In this blog, I will be sharing some things which we learnt while migrating our app from on-Premises to AWS. These are not in any specific order, each point is a unique learning in itself.

  
 - Automate the IT setup (Most important, this way the automated setup script can be shared across the organisation and a lot of manual work can be avoided)
 - Always start with smaller instances, and create new servers with automated script on the fly whenever required.
 - Use elastic cache for Redis services instead of self manged Redis servers (as it is easy to scale, operate and deploy).
 - Drop the https at ELB. (as https is a layer 7 security, and implemented to transfer the data securely over the unsecured networks, so it can be dropped at ELB.)
 - Distribute the servers equally between AZ's
 - Create security groups based on the infra usage, like app-security-group, database-security-group etc.
 - Use cloud watch for server monitoring.
 - Use S3 for regular database backs.(cheaper and faster way for DB retrieval in case of emergency)
 - Keep your system distributed, 10 smaller instances will cost far less than 3 larger instances, and here the automated scripts will help.
 - Always have EBS attached to your machine, and do not store any data in the empherial storage, as it may get lost with machine restart.
 - Use specific instance types for specific work, like c instances for compute intensive tasks, i instances for mongo database related services.