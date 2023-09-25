---
author: zihui
pubDatetime: 2023-07-13T15:00:00Z
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
   <img src="https://github.com/OkabeRintarouBeta/okabeBlog/tree/main/public/assets/system_design_youtube/client_server_basic.jpg">
   e.g. streaming platforms like Netflix
2. thick client: logic and data on the client side
   - Many operation happening on the client side(e.g. video game)

### Structure(2-layer v.s. 3-layer)

<img src="https://github.com/OkabeRintarouBeta/okabeBlog/tree/main/public/assets/system_design_youtube/client_server_basic2.jpg">

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

1. Datastores examples
   - Databases, Cache, Queues(send sms request, send email request), Indexes(most searched items in 1 hour)
2. How data is generated?
   - Users
   - Internal
   - Insights(history:payment details)
3. Factors to consider
   1. Type of data
   2. Volume
   3. Consumption/Retrieval
   4. Security
4. Type of System
   1. Authorization: user login,identity management
   2. Streaming: Netflix, Hotstar, Prime Video
   3. Transactional: E-commerce sites, ride booking apps, grocery ordering apps
   4. Heavy Compute Systems: Image recognition systems, video processing using ML

## Databases

### Types of DBs

- Relational
- Non-relational
- File
- Network
- ...

1. Relational DB
   - Schema
     - Represent data in tables and rules
     - Can represent complex data easily, ensures garbage or null value is not populated into the DB, ensure all schema constrained are followed
   - ACID properties
     - Atomicity: a transaction happens completely or doesn't happen at all
     - Consistency: data is consistant on different sides
     - Isolation: Two transactions do not interfere with each other
     - Durability: Guarantees that transactions completed will survive permanently

- Discussion
  - When to choose:
    - Need to support ACID properties
    - Schema doesn't change
  - When not:
    - Schema isn't fixed
    - Tables become huge
    - Can scale vertically(increase the storage of one machine), but couldn't scale horizontally easily(divide into several DBs)

2. Non-relational DBs

- `Key: Value` storage
- Store configuration related data, request:response, etc.

3. Document DBs

- Used when
  - Not sure of the size/schema
  - Support heavy reads and writes
  - Details/properties can change overtime
- Example format
  ```json
  [
    {
      "name": "Raj",
      "age": 29,
      "sex": "Male"
    },
    {
      "name": "Kajal",
      "age": 30,
      "sex": "male"
    }
  ]
  ```
- Discussion
  - Advantage
    - Highly scalable
    - Don't have to seach for multiple tables
    - Support heavy reads and writes
  - Disadvantage
    - Might have to handle null values
    - Don't provide ACID transactions

4. Column DBs

- Store interactions on streaming apps, health data, etc.
- Do not support huge reads
- Structured based on their functionality
- Good support of distributed databases

5. Seach DBs

## Application Programming Interface (API)

- What is API?
  - One entity calls an API to do something to the other entity, the process behind the implementation is unknown
- Why use API?
  - Communication
  - Abstraction: implementation can change, but the entity calling the API don't need to know about it
  - Platform Agnostic: services in different language or platform can communicate through APIs
- API Classification
  - Private APIs
    - hidden, only application running on the device could access it
  - Public APIs
    - Available to public, e.g. Spotify API
  - Web APIs
    - Application on cloud interact with each other
  - SDK/Library APIs
    - e.g. use in code, abstract
- API standards
  - RPC
  - SOAP
  - REST
  - ...
