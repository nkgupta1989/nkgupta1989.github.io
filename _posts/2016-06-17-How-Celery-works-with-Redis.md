---
layout: post
title: How Celery interacts with Redis
---

As we know that there are 2 python modules {celery} and {celery-beat}, which we can be used to execute the asynchronous tasks, and to run the schedule tasks.


In this blog I will be sharing few learning which I learnt while working on celery workers.

Pre-requisite:-
A very basic knowledge of 

    python
    celery
    celery-beat
    redis

Celery is the worker, which actually executes the tasks, and celery-beat is the scheduler which actually triggers the tasks. Now in order to communicate with each other they can use Redis or Rabbit-MQ, a simple key-value pair databases.

We have used celery with redis as the task database store.

__IMPORTANT :- Now whenever celery beat has to trigger a task, it creates a linked list data type if not exist with a name "celery" by default, and push the new task at the end of this linked list.__

So at any point of time this list will contains all the pending celery tasks, these tasks are the tasks that are triggered by beat, but none of the workers have picked them till yet.

    Redis Terminal
    127.0.0.1:6379[0]> llen celery
    (integer) 3

llen will give the length of the linked lists.

__IMPORTANT :- Now as soon as a worker is ideal, it picks the tasks from the starting which is oldest task and removes it from the linked list, and generates a unique id for this task, and create a simple key value mapping in the redis with some {default names+this unique id } and starts executing this tasks.__

    Redis Terminal
    Sample key for a task that is currently executing is like:-
    127.0.0.1:6379[0]> "celery-task-meta-ae9bea75-4ee4-4dee-9e32-62fc5dacbd0f"

Once the task is over this key is removed from the redis by the worker, now if somehow celery worker got killed in between the tasks, then the same task will be executed again from the starting as its redis key will still be there in redis.

__IMPORTANT :- Now for monitoring :- what we have done is we are checking the length of the linked list mentioned above, it should never be more than a specific number.__


Hope it will help in future :)