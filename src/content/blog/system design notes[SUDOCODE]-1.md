---
author: zihui
pubDatetime: 2023-07-13T15:50:00Z
title: SUDOCODE--System Design Notes1
postSlug: system-design-notes
featured: true
draft: false
tags:
  - system design
ogImage: ""
description: Notes for system design course on Youtube by SUDOCODE
---

## Table of contents

## Components Overview

- Logical Entities
  - data,database, applications,cache, message queues, interface...
- Tangible Entities
  - text,image,video..
  - mongodb,MySQL,...
  - Java,Python...
  - Redis,MemeCache...

### Quiz

What are the components that are absolutely necessary to build any kind of system no matter the requirement or the scale or the users. Comment your answers below along with any queries that you may have.

- For a very basic system, we need the following components for sure.

  - Database - Persistence is something that is required for every backend application. So, A Database is utmost necessary
  - Server - For any system that needs to scale, we need to have a server, so that we can have an API driven architecture so that if the frontend changes, the new frontend can easily consume the existing API's.
  - Network - We need a Network as a component for the communication between the client and the server and between the server and the database.

- Not necessary

  - Components like a frontend are not necessary because even a console will work or even a curl command will work.
  - Stuff like Load Balancers, Caches are there for efficient and highly scalable systems, they are not required for basic applications.

## Client Server Architecture

### Client Type

1. thin client: logic and data on the server(backend) side
   <img src="../images/system_design_youtube/client_server_basic.jpg">
   e.g. streaming platforms like Netflix
2. thick client: logic and data on the client side
   - Many operation happening on the client side(e.g. video game)

### Structure(2-layer v.s. 3-layer)

<img src="../images/system_design_youtube/client_server_basic2.jpg">

### Quiz

List down the real world use cases when you would choose a thick client architecture over thin client architecture and vice versa. List pros and cons in all scenarios. Can you think of a system which has thick client and thin client and multiple tiers? Also, can servers behave as clients?

1. When I choose thick client over thin client architecture : When a system want to takes care of more computation in the client side and want to relay less on the server side in this scenario we can go for thick client. <br>Example: Ms-Excel, Ms-Paint.
2. When I choose thin client over think client architecture: When a system want to takes care of more computation in the server side and making client lite weight in this scenario we can go for thin client.
   <br>Example : Web-browser.
3. Pros and Cons of thick and thin client:
   - Thick client:
   - Pros :
     - System can perform seamlessly without considering network latency.
     - More cost to maintain.
     - High responsive to the user
   - Cons:
     - No assurance of data availability as it has possibility of system crash and other difficulties.
     - High cost to maintain the app.
     - Effective usage of system resource is less.
   - Thin client:
     - Pros:
       - Easy to manage/maintain system infrastructure.
       - Highly scalable and easy maintainable app
       - Cost effective
     - Cons:
       - Highly dependent on network connectivity/availability.
       - Each components are managed by component professionals.
4. Thick client example : Ms-Excel
   Thin client example : webserver
   Multi tier example : youtube
5. Yes, Server can be a client also, because in large enterprise applications not all the apps takes care of computations some may get the other processed/computation result and take care of responsibility to get back to presentation layer.

## Proxy

proxy: on one's behalf

### Proxy Type

1. Forward Proxy
   - Client(s) <-> Proxy <-> Server
   - Server doesn't know the IP of the client, just know the proxy's IP
   - Can cache the response on proxy side
   - Can be used for blocking access to some sites
2. Reverse Proxy
   - Client <-> Proxy <-> Server(s)
   - Client doesn't know the identity of the server, only know the proxy's identity
   - Can be used for load balancing

### Quiz

## Data & Data Flow
