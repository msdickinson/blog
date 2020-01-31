+++
date = 2020-01-25T07:00:00Z
draft = true
layout = "post"
title = "Roller Coaster Cross Cutting Concerns (Part 3)"

+++
## Cross Cutting Concerns

Roller coaster has mutiple APIs and all of them require testing. Building out a stack will help me follow SOLID by not repeating my self and having a single source of truth. As reviewed in my last post In X Section. 

### Building Early

A common pratice in agile is to build things as you need them. I have seen how this can cause a massive amount of context switching between packages and APIS. One of the reasons it is suggested to follow this pratice is to not over aretche or build somthing you end up not needing. I am accepting that risk going ahead but only after i build our prototpyes and took extensive time to resserach my needs. I suspect my needs wont fit exactly with my resreash but I feel the benfits of handling the stack first outway the concerns.

### Conintues Integaretion - Local

For building local I created a powershell script to drop my packages into a root folder "C:\\Packages" and I add "ci-" with a datetime stamp to ensure quick development.

### Conintues Integaretion - Production

My Production pipeline should ensure the project builds, tests pass, and releaseing to nuget. A major learning point is the power of a release pipeline such as azure devops releases. It gives you addtional check points that you can require mannaul intervention. Although currently it is not nesseary for the pacakges it will be for the APIS to come.

At one point I was checking for code coverage from my unit tests. After days of fiddling with it and finding reported bugs in the applications I was using I decided to move on. This means that the pipeline will only check that the tests pass, and that is on the user making the PR to ensure 100% code coverage (one of my requirements for this project) see (  ... ).

![](https://www.marksdickinson.com/Media/Nuget Pipeline.png)

### DickinsonBros.Test

A wrapper library for DateTime

**Features**

* DI unit tests in a clean fashion
* Easily pull out mocks
* Unit test controllers by injecting headers, claims and taking over the HttpContext for more precise unit tests

[https://github.com/msdickinson/DickinsonBros.Test](https://github.com/msdickinson/DickinsonBros.Test "https://github.com/msdickinson/DickinsonBros.Test")

### DickinsonBros.DateTime

A wrapper Library for DateTime

**_Features_**

* Adds extensibility via abstraction
* Allows for unit testing

[https://github.com/msdickinson/DickinsonBros.DateTime](https://github.com/msdickinson/DickinsonBros.DateTime "https://github.com/msdickinson/DickinsonBros.DateTime")

### DickinsonBros.Encryption

Encrypt and Decrypt strings

**_Features_**

* Certificate based encryption
* Configure certificate location

[https://github.com/msdickinson/Dickinsonbros.Encryption](https://github.com/msdickinson/Dickinsonbros.Encryption "https://github.com/msdickinson/Dickinsonbros.Encryption")

### DickinsonBros.Guid

A wrapper Library for DateTime

**_Features_**

* Adds extensibility via abstraction
* Allows for unit testing
* Sperate abstractions library to reduce coupling of packages.

[https://github.com/msdickinson/DickinsonBros.Guid](https://github.com/msdickinson/DickinsonBros.Guid "https://github.com/msdickinson/DickinsonBros.Guid")

### DickinsonBros.Redactor

A redactor that can take a json string or an object and return a redacted string in json.

**_Features_**

* Configurable properties to redact by name
* Configurable regular expressions to validate against
* Sperate abstractions library to reduce coupling of packages.

[https://github.com/msdickinson/DickinsonBros.Redactor](https://github.com/msdickinson/DickinsonBros.Redactor "https://github.com/msdickinson/DickinsonBros.Redactor")

### DickinsonBros.Logger

A logging service that redacts all logs

**_Features_**

* Redacts all logs
* Allows for dictionary to become first class propertys in the log.
* Ability to add a correlation id that works though async in straight forward fashion
* Allows for improved testability
* Sperate abstractions library to reduce coupling of packages.

[https://github.com/msdickinson/DickinsonBros.Logger](https://github.com/msdickinson/DickinsonBros.Logger "https://github.com/msdickinson/DickinsonBros.Logger")

### DickinsonBros.Middleware

Middleware for ASP.Net that adds redacted logging.

**_Features_**

* Logs requests redacted
* Logs responses redacted and Status Codes
* creates collreation Id
* Catch all uncaught expectations and log them redacted

[https://github.com/msdickinson/DickinsonBros.Middleware](https://github.com/msdickinson/DickinsonBros.Middleware "https://github.com/msdickinson/DickinsonBros.Middleware")

Lets take a closer look

\-> EnsureCorrelationId

### DickinsonBros.SQL

SQL abstraction that adds increased logging on exceptions

**_Features_**

* Improved Logs
* Allows for improved testability

[https://github.com/msdickinson/DickinsonBros.SQL](https://github.com/msdickinson/DickinsonBros.SQL "https://github.com/msdickinson/DickinsonBros.SQL")

### DickinsonBros.DurableRest

SQL abstraction that adds increased logging on exceptions

**_Features_**

* Ability to retry requests
* Timeouts
* Logs all requests redacted with meta data

[https://github.com/msdickinson/DickinsonBros.DurableRest](https://github.com/msdickinson/DickinsonBros.DurableRest "https://github.com/msdickinson/DickinsonBros.DurableRest")

## 