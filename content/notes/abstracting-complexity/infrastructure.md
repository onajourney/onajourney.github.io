+++
title = 'Abstracting Complexity - Infrastructure'
date = 2024-01-20T10:03:26-05:00
tags = ["Infrastructure", "aws"]
draft = true
+++

The most direct way of of making infrastructure more maintanable is through automation. This removes the need to manually setup infrastructure. Great! But how do we achieve this? We could of course script away, however there are better tools out there. Tools that let us handle infrastructure in a more declarative manner. Tools that handle the state of our infrastructure. Tools that let us treat **infrastructure as code** which we can check into our projects repository. 

One such tool is terraform. However, as great as terraform is, it is in itself is a source of complexity as it introduces HCL (HashiCorp Configuration Language), however we can levarage HLC in order to reduce the need for it all together.

In order to have more maintainable infrastructure, lets find the sources of complexity in the context of infrastructure, and to be more specific, in the context of setting up infrastructure in aws.

- A pretty obvious one is the sheer number of products available to us (as of the time of writing aws has over 200!), each with their own configuration. 

- Another source of complexiy is security. Who needs access to the products we deploy? This not only includes developers working on a project but also products that interact with eachother. 

- Delivering products to consumers often requires the use of multiple environments. So minizing friction when setting up environments is also important. Specially so for a developer setting up their own.
