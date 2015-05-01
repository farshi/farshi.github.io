---
layout: post
title: "Amazon SQS Notes"
date: 2015-04-27 22:09:04 +1000
comments: true
categories: [Amazon SQS , AWS Architecture Certificate]
---
 
Some Notes about the Amazon SQS service. You will find it useful if you want to prepare for AWS architecture certificate exam.

Amazon Simple queue service (SQS):

* is A web service that gives you access to a message queues.
* Queues can be used to store messages while waiting for a computer to process them.
* is distributed Queue System.
* is fast and reliable 
* A queue is a temporary repository for messages hat are waiting processing.
*By using SQS you can decouple the components of an application so they run independently.
* Messages can contain up to 256KB of text in any format.
* Messages can be retrieved by any distributed component by using Amazon SQS API.
* Amazon SQS ensures delivery of each message at least once. so one message can be read by many distributed application components simultaneously. And They are remained until you remove them from the queue yourself.
* SQS does not support FIFO ( First In Firs Out).
* SQS is a pull system , and messages are not pushed out like SNS.
* Visibility time out period for a message starts when a component retrieves the message. The default visibility time out is minimum 30 seconds and maximum 12 hours.
* SQS Billed at 64kb Chunks.
* Firs 1 million Amazon SQS request per month are free.
