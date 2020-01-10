---
layout: post
title: Roller Coaster Design Decisions (Part 2)
date: 2020-01-09T07:00:00.000+00:00

---
# Design Decisions

This section will deal with system designs, SOLID, unit testing, API considerations, API flows and cross cutting concerns.

## System Designs

I’ll work though different designs and find the design that fits my principles best.

### (1) Monolith

A single project and database.

![](https://d3efwhw5kd1q0b.cloudfront.net/Media/Design1.png)

_Pros_

* Simple

_Cons_

* API coupling
* Database coupling (Reporting queries are built on it)
* No partial deploys
* Cannot scale APIs and databases independently

### (2) Monolith with Isolated Schemas

A single project and database with isolated data access using schemas.

![](https://d3efwhw5kd1q0b.cloudfront.net/Media/Design2.png)

_Pros_

* Data is protected by domain logic and limited access 

_Cons_

* API coupling
* Database coupling (Reporting queries are built on it)
* No partial deploys
* Cannot scale APIs and databases independently

### (3) Monolith with Isolated Database

A single project with multiple databases and a reporting API that contains non sensitive data.

![](https://d3efwhw5kd1q0b.cloudfront.net/Media/Design3.png)

_Pros_

* Data is protected by domain logic and limited access
* Scale databases independently
* Reporting API cannot query sensitive data and removes load from production databases
* APIs run independently and push out results to other APIS

_Cons_

* API coupling
* No partial deploys
* Cannot scale APIs independently
* API Failures can cause databases to become out of sync. Solving this with transactional requests would increase complexity
* Increased hosting cost (multiple databases)

### (4) Microservices with Isolated Databases

Multiple projects with their own database and a reporting API that contains non sensitive data.

![](https://d3efwhw5kd1q0b.cloudfront.net/Media/Design4.png)

_Pros_

* Data is protected by domain logic and limited access
* Scale APIS and databases independently
* Reporting API cannot query sensitive data and removes load from production databases
* APIs run independently and push out results to other APIS
* Ability to deploy single API r

_Cons_

* API coupling
* Cross cutting concerns require NuGet Packages
* Increased Response Times (Between APIS)
* API Failures can cause databases to become out of sync. Solving this with transactional requests would increase complexity
* Increased hosting cost (multiple APIs and databases)


### (5) Microservices with Isolated Databases, Bus (Pub/Sub), and SignalR

Multiple projects with their own database and a reporting API that contains non sensitive data. Communication between APIS is done though a bus via a pub/sub modal using SignalR for signaling. APIS can push to the user using SignalR. 

![](https://d3efwhw5kd1q0b.cloudfront.net/Media/Design5.png)


_Pros_

* Data is protected by domain logic and limited access
* Scale APIS and databases independently
* Reporting API cannot query sensitive data and removes load from production databases
* APIS are durable, in the case of in outage they can recover with retries from the Bus
* Increased hosting cost (multiple APIs and databases)
* Ability to deploy single API
* APIs can push a message to a user

_Cons_

* Bus coupling
* Cross cutting concerns require NuGet Packages
* Increased Response times (Bus)
* Increased hosting cost (multiple APIs and databases)



### Additional System Design Considerations

#### SQL / NoSQL

I have limited experince using CosmosDB (noSQL) professionally. But I wanted to consider the pros and cons and see if noSQL was a good fit.

_Pros_

* Increased Up time (Redundancy)
* Ability to scale out instead of Up (Avoiding potential bottle necks)
* Database designed for fast reads (with data duplication as needed
* Reduced Costs

_Cons_

* Limited ability to query
* Complex Records

The pros look fantastic. I have concerns being able to query for reporting purposes but design (5) solves for that. The inability to query and complexity of the records for the coasters API do not seem worth the effort. I hope one day to improve my noSQL skills further but It does not appear to be a good fit for this project.


#### Redis

All designs except for (1) have databases that require you to go through the API to access it. Addtionly I intend to keep the database and there API on the same machine. With these requirements the benefits of redis became greatly diminished. If I find I have queries taking a long time or have load concerns on my database I will reconsider Redis in the future.

### Design Conclusion

After reviewing designs with there pros and cons and then comparing them to my principles a clear winner stands out. I am going to go ahead with microservices with isolated databases, bus (Pub/Sub), and SignalR. The durableness of the design, ability to maintain, and flexibility it brings are worth increased responses times, complexity, additional work and hosting costs.

## SOLID

SOLID helps keep applications maintainable and testable. These traits fit very well with my project principles. 

In implementation detail that needs consideration is dependency inversion. This dictates that dependencies should depend upon abstractions instead of concrete classes. This brings the benefits of classes being extensible and testable. To solve for this generally factories or dependency injection are used.

Factories generate in instance of another class. Dependency injection (DI) uses configuration, and injects the dependency where you need them. In reality DI injects them in more places then where you ask for your dependency but only as needed.

I have chosen to use dependency injection because it reduces code, reduces tests, reduces scope and has additional extensibility options.

## Unit Testing

I have discussed with many developers the pros of cons of unit testing, and have made my own conclusions.

_Pros_

* Find a class of concerns early
* Tests can be reused as regression tests
* Requires fine grain look at code. Often solves for problems that unit testing does not directly test for.
* Saves time in the long run (From my experience)

_Cons_

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


## API Considerations

Each API will include the following Projects

* Abstractions – Shares models between View API and Proxy
* View API (ASP.net)
* Logic API (Library)
* Infrastructure (Library)
* Database (SQL Scripts)
* Proxy (Library) and Proxy Runner (Console)

The two interesting points here are database and proxy. Keeping all of my queries inside of source control has served me well in the past. The proxy for most APIS won’t be used in production, but will give me another option to test my API when running local, and when running integration tests.

Each Project (excluding Database and Proxy runner) will have a Unit test project with them.

## Cross Cutting Concerns (NuGet Packages)

Now that I have a design, I reviewed my coding principles and practices and created this flow here. In this example, I can see that APIS will be using REST, SQL, and redacted logging.

**High level Flow**

![](https://d3efwhw5kd1q0b.cloudfront.net/Media/Flows+1.png)

Before taking my theory too far, I decided it was time to create a quick prototype of Account API and flush out unit tests. Doing so turned up concerns about testability and repeating patterns of code. Here are 2 of my main prototype flows.

![](https://d3efwhw5kd1q0b.cloudfront.net/Media/Flows+2.png)

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

After careful thought on multiple system designs a plan emerged that fits well for the user stories and coding principles. Next walking though implementation decisions with SOILD using dependency injection. Considerations with unit and integration testing were reviewed. Then creating a general guideline for APIS using N-Tier, proxies, and SQL Scripts. Finally looking thought request flows and prototyping to find cross cutting concerns.

These considerations have help set the table to hit the ground running with a clear high-level plan. I have heard that designing to early can cause over architecture instead of growing it as you need it. I have found that by the time it’s a major problem it can be a massive effort and level of risk to change it. Even if it’s a moderate effort explaining to your boss that you need take a few days, weeks, months to rewrite code for maintenance is an uphill battle.