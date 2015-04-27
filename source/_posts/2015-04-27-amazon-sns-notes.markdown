---
layout: post
title: "Amazon SNS Notes"
date: 2015-04-27 22:10:57 +1000
comments: true
categories: [Amazon SNS , AWS Architecture Certificate]
---
Some Notes about Amazon SMS service , you will find it useful if you want to prepare for AWS architecture certificate.

Amazon Simple Notification service  (SNS) :

* Is a web service that makes it easy to set up, operate, and send notifications from the cloud.
* Amazon SNS allows the "publish-subscribe" (pub-sub) messaging paradigm.
* SNS eliminates the need to periodically check or pull for new information and updates.
* it is inexpensive, pay-as-you-go model with no up-front costs.
* Amazon SNS can be deliver notifications by SMS text messages or email, to Amazon Simple Queue Service (SQS) queues, or any HTTP endpoint.
* To prevent messages from being lost,all messages published to Amazon SNS are stored redundantly across multiple availability zones.
* SNS Topic allows you to group multiple recipients. It acts like a access point for allowing recipients to dynamically subscribe for identical copies of the same notification.
