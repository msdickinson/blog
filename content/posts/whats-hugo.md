---
layout: post
title: Roller Coaster Design Decisions
date: 2020-01-09T07:00:00+00:00

---
Roller Coaster 2020

# Roller Coaster

## Project Summary

Roller coaster is a game on a website that will allow you to build, ride, and share coasters. To accomplish this, I built versions of this game before with web assembly (Blazor C#) and WebGL (Three.Js). This time around I intend to build the game and surrounding services in the same way I would solve problems in my place of work. I will first focus on the surrounding services before working on the game itself.

Roller coasters stand outs in the past have been its flexibility and simplicity. Mainly the game is designed that you should be able to mash buttons and still complete a coaster. My latest version was built in 2018 at [http://rollercoaster.dickinsonbros.com/](http://rollercoaster.dickinsonbros.com/ "http://rollercoaster.dickinsonbros.com/"). This version only allows you to build roller coasters and the builder is not fully feature complete. In past versions you could ride the coaster as well.

![](/uploads/Tracks.PNG)

## User Stories

These user stories are desired features for the application from the users perspective.

_Account_

* Create Account (Email Optional)
* Login
* Reset Password (Email)
* Update Password
* Update Email Settings directly from all emails
* Update Email Settings when logged in

_Coaster_

* Create
* Save
* Load
* Share (Private Link)
* Publish (Public)
* Find Rate Published coasters
* Get notification when someone a published coaster

_Achievement_

* Gain Achievements
* View Achievements
* Get notification when I receive an achievement

## Coding Principles

I have decided on 4 principles to drive the project. After and having many discussions with work colleagues I determined coding principles highly depend on our project. For example, I have a brother who writes mobile app games for a living. Many don’t store any user data like a username or a password, so security is less important. Additionally, putting in the extra effort to help ensure correctness, and that the game is maintainable may not be worth the effort until he knows the game is popular enough. This is totally acceptable as they are in a sense a prototype. I am instead aiming for a production ready system and these 4 principles fit with my user stories.

_Secure_

* Modern Password encryption
* Login Attempts Guarded
* Communication Encrypted
* No PII In Logs, or Reporting database

_Correct_

* Testing - Unit, Integration, Manual 
* Multiple environments 
* Blue / Green Deployment
* High Fidelity logging

_Service Level Agreement_

* Load Testing
* Monitoring 

_Maintenance_

* High Fidelity logging (Yes this in here twice)
* Reporting - High level reports (Accounts, Coasters, Achievements, System Health)
* Source Control
* Continuous Integration
* Documentation and health checks

I have seen differing opinions on the degree of correctness and ability to maintain a project. My experience has shown me even for short lived projects It is generally worth the extra effort to ensure its correct and maintainable. Having to patch a project for a 1 liner can take meetings, coding, code reviews, releases and post release monitoring. (summary sen)

## Conclusion

I have laid out the project overview and user stories and decided to build a production ready system over a prototype. I have created coding principles and items that to help measure and ensure they are meet. Finally, I added coding practices that I feel fit well with the coding principals. These high-level decisions will guide design decisions, and creation of the project.

# Design Decisions

This section will deal with SOLID, system designs, API flows, cross cutting concerns, dependency injection, unit testing, Integration testing, and proxies

## System Designs

I’ll walk though different designs and find the design that fits my coding principles best.

### (1) Monolith

A single project and database.

![](https://d3efwhw5kd1q0b.cloudfront.net/Design1.png)
| Pros · Simple | Cons · Direct API Coupling · Database Coupling · No Partial deploys · Cannot scale parts Independently |

| Pros | Cons |
| --- | --- |
| Simple | Direct API Coupling |
|  | Database Coupling |
|  | No Partial deploys |
|  | Cannot scale parts Independently |

### (2) Monolith with Isolated Schemas

A single project and a single database with isolated data access using schemas.

![](https://d3efwhw5kd1q0b.cloudfront.net/Design2.png)

| Pros | Cons |
| --- | --- |
| Data is protected by domain logic and limited access reducing coupling | Direct API Coupling |
|  | Database Coupling |
|  | No Partial deploys |
|  | Cannot scale parts Independently |

### (3) Monolith with Isolated Database

A single project with multiple databases and a reporting API that contains any relevant non sensitive information.

![](https://d3efwhw5kd1q0b.cloudfront.net/Design3.png)

| Pros | Cons |
| --- | --- |
| Data is protected by domain logic and limited access reducing coupling | Direct API coupling |
| Ability to change databases at in API Level without affecting other APIS and reporting | No partial deploys |
| Reporting API reduces access for reporting purposes, and removes calls from production databases | Cannot scale parts independently |
| APIs are fully independently functional and pass forward information to others instead of requesting data | Increased hosting cost (multiple databases) |

### (4) Microservices with Isolated Databases

Multiple project with their own database and a reporting API that contains any relevant non sensitive information.

![](https://d3efwhw5kd1q0b.cloudfront.net/Design4.png)

| Pros | Cons |
| --- | --- |
| Data is protected by domain logic and limited access reducing coupling | Direct API coupling |
| Ability to change databases at in API Level without affecting other APIS and reporting | Increased Response Times (Between APIS) |
| Reporting API reduces access for reporting purposes, and removes calls from production databases | Cross cutting concerns require NuGet Packages to stay DRY |
| APIs are fully independently functional and pass forward information to others instead of requesting data | Increased hosting cost (multiple APIs and databases) |
| Ability to deploy Single APIS at a time |  |
| Ability to scale APIS independently |  |
| APIS |  |

### (5) Microservices with Isolated Databases, Bus (Pub/Sub), and SignalR

Multiple project with their own database and a reporting API that contains any relevant non sensitive information. Communication between APIS is done though a bus, and APIS can push responses to the user using SignalR. The Bus communicates with APIs via a pub/sub modal using SignalR to alert then when there is data available.

![](https://d3efwhw5kd1q0b.cloudfront.net/Design5.png)

| Pros | Cons |
| --- | --- |
| Data is protected by domain logic and limited access reducing coupling | Bus Coupling |
| Ability to change databases at in API Level without affecting other APIS and reporting | Increased Response Times (Bus) |
| Reporting API reduces access for reporting purposes, and removes calls from production databases | Cross cutting concerns require NuGet Packages to stay DRY |
| APIs are fully independently functional and pass forward information to others instead of requesting data | Increased hosting cost (multiple APIs and databases) |
| Ability to deploy Single APIS at a time |  |
| Ability to scale APIS independently |  |
| Ability for Any API to respound to message user |  |

### Additional System Design Considerations

#### SQL / NoSQL

I have used SQL professionally often with very limited experience in noSQL. I decided to do some reading and talked to multiple teams at three companies who are using CosmosDB or MonogDB in production.

| Pros | Cons |
| --- | --- |
| Increased Up time (Redundancy) | Limited ability to query |
| Ability to scale out instead of Up (Avoiding potential bottle necks) | Potential Expensive Writes (As may be in records) |
| Database designed for fast reads (with data duplication as needed) | Complex Records |
| Cheaper |  |

Reviewing the cons, the inability to query won’t be a problem as ill have duplicate data in the reporting database using SQL. Expensive writes are not really a concern for me. But complex records are where I have a lot of pause. I worked out what the coasters API may look like using it and it brought a lot of additional complexity and concerns to the table. The additional up time is the only pro that I feel I would strongly desire, but It is not worth the additional complexity for this project. To be fair my noSQL experience is limited but I am going to stick with SQL in all APIS for the MVP.

#### Redis

After concluding that I am going to put each database on the same machine as there API, the benefits of Redis became greatly diminished. If I find I have queries taking a long time or have load concerns on my database I will reconsider Redis in the future.

### Conclusion

After reviewing designs, reviewing their pros and con and then comparing them to my coding principles a clear winner stands out. I am going to go ahead with microservices with isolated databases, bus (Pub/Sub), and SignalR. The durableness of the design, ability to maintain, and flexibility it brings are worth increased responses times, complexity, additional work and hosting costs.

## SOLID

SOLID helps keep applications maintainable and this fits well with my coding principles.

One principle that needs in implementation detail is dependency inversion. This dictates that dependencies should depend upon abstractions instead of concrete classes. This brings the benefits of classes being extensible, and testable. To solve for this generally factories or dependency injection are used.

Factories generate in instance of another class. Dependency injection (DI) uses configuration, and injects the dependency where you need them. In reality DI injects them in more places then where you ask for your dependency but only as needed.

I have chosen to use dependency injection because It reduces my code, reduces tests, scope, has additional extensibility options.

## Unit Testing

I have discussed with many developers the pros of cons of unit testing, and have made my own conclusions.

_Pros_

* Find a class of concerns early
* Tests can be reused as regression tests
* Requires fine grain look at code. Often solves for problems that unit testing does not directly test for.
* Saves time in the long run (From my experience)

Cons

* Code needs to be designed to be unit tested (If you already follow SOLID, its rarely in issue)
* Takes additional time, that should be done when the code is written

There are a lot of different styles and opinions on unit testing. These are mine.

* Test for return value, exceptions, state change, and interactions.
* Test names follow UnitUndertest_Sencrio_Expected convention.
* Each unit test I target one line of code and use as many asserts as needed for that line.
* 100% Unit Test Coverage, every method independently tested (even if indirectly tested) and change my private methods to internal.
* For dependencies that have statics, and very challenging to test code, I choose to wrap them in another class and then add exclude from coverage.
* For plan data objects that have no logic I exclude form code coverage.

I have written unit tests on the daily for 12 months. I have found the process of writing units to be even more valuable than the unit tests. It forces me to slow down and take heavy considerations of my code by walk through all the of code paths without glossing over anything. I also enjoy the increased confidence I have when modifying in existing solution as breaks existing tests pointing me to places that I caused a change.

## Integration Testing

Integration testing is a great way to test before deployments and after. I have a single project that runs integration tests for all projects. I write an integration test for all expected return flows from each end point in the service, with the exception of 500s. If in API is supposed to update the user in the database and It returns a 200. I accept the 200 and unless needed do not inspect the database for the expected change. All tests are designed to be able to run repeatedly and in parallel.

## API Solutions

Each API Will include the following Projects

* Abstractions – Shares models between View API and Proxy
* View API (ASP.net)
* Logic API (Library)
* Infrastructure (Library)
* Database (SQL Scripts)
* Proxy (Library) and Proxy Runner (Console)

The two interesting points here are database, and proxy. Keeping all of my queries inside of source control with some intellisense has served me well in the past. The proxy for most APIS won’t be used in production, but will give me another option to test my API when running local, and when running integration tests.

Each Project (excluding Database and Proxy runner) will have a Unit test project with them.

## Cross Cutting Concerns (NuGet Packages)

Now that I have a design, I reviewed my coding principles and practices and created this flow here. In this example, I can see that APIS will be using REST, SQL, and redacted logging.

**High level Flow**

![](https://d3efwhw5kd1q0b.cloudfront.net/Flows 1.png)

Before taking my theory too far, I decided it was time to create a quick prototype of Account API and flush out unit tests. Doing so turned up concerns about testability and repeating patterns of code. Here are 2 of my main prototype flows.

![](https://d3efwhw5kd1q0b.cloudfront.net/Flows 2.png)

After reviewing my prototype, I came up with these cross-cutting concerns.

* SQL – High Fidelity logging, Ability to unit test
* Middleware - High Fidelity logging, correlation Ids, and handles exceptions
* Durable Rest - High Fidelity logging, adds ability to retry requests
* Guid - Ability to unit test
* DateTime - Ability to unit test
* Encryption (Certificate) – Ability to encode and decode strings with certs
* Logger – Adds correlation ids and redaction to all logging
* Redactor – redacts objects and json strings with regular expression and property names

_Versioning_

After reading [https://devblogs.microsoft.com/devops/versioning-nuget-packages-cd-1/](https://devblogs.microsoft.com/devops/versioning-nuget-packages-cd-1/ "https://devblogs.microsoft.com/devops/versioning-nuget-packages-cd-1/") from Microsoft I decided to follow their lead. All of the packages follow semantic versioning. For development I add CI + Datetime to the version.

_Abstractions_

At my place of work, we have a much larger stack for our use cases, and there is strong coupling between packages. I reviewed solutions to this and found Microsoft solves this by creating abstraction packages. I will add them only when another package depends on one.

## Conclusion

After careful thought on multiple system designs a plan emerged that fits well for the user stories and coding principles. Next walking though implementation decisions with SOILD using dependency injection. Following though considerations using unit testing, and Integration testing. Then creating a general guideline for APIS to follow to follow N-Tier, proxies, and SQL Scripts. Finally looking at a request and creating a quick prototype to find cross cutting concerns and implementation decisions when making them.

These decisions have help set the table to hit the ground running. To be clear this is still going to be in agile effort but the planning help give insight, and vision.