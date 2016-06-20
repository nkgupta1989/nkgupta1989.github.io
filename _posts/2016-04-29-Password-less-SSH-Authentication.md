---
layout: post
title: Password less SSH Authentication
---

In this blog I will be stepwise explaining the flow of password less SSH authentication using the pem file. Pre-requisites are a basic knowledge of servers and SSH.

When ever we create a SSH key using ssh-keygen, we get public-private key pair. 
Public key is the one that should be placed in the remote server and private key is the one that should be privately shared with the people with whom you want to give the access to the remote server.

The public key should be in the .ssh folder of the remote server.

Sample command to SSH using the private key.

        ssh -i /home/{key-name}.pem root@{instance-IP}

![alt text](http://nkgupta1989.github.io/images/sshauthentication.png "Password less SSH Authentication")


Now listing down the steps that will happen in the background :-
Remote Server :- R1
Machine doing SSH :- M1

 __Step 1:__
  
 Machine M1 interacts with R1 mentioning public key name which is to be used for SSH.
 
 
 __Step 2:__
  
 Remote Machine R1, creates a random string suppose RS1 and encrypts using the public key, to create an encrypted string ES1, this string can only be decrypted using the respective private key.
 
 Now Remote Machine R1, send the ES1 + sessionId to the remote machine R1
 
 
 __Step 3:__    
 
 M1 decrypt's the ES1 using the private key that it has and generates the RS1, and respond with md5(RS1+sessionId) to the Remote server RS1.
 
 
 __Step 4:__    
 
 Remote machine R1 will have the mapping of the RS1 with the sessionId, and generates a md5 of its own to match with the md5 value send by the M1, if matched than SSH is successful and further connection is established. 
 
 
This how a password less SSH authentication is done using the Public-Private keys.