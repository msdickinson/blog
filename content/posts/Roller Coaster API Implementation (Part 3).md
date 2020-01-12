+++
date = 2020-01-25T07:00:00Z
draft = true
layout = "post"
title = "Roller Coaster Implementation (Part 3)"

+++
## Packages

The stack is highly used across all of the projects and is great place for me to start.

Builds exist in .github/workflows you can also view past builds by adding /actions/ to the Urls below. the production and test both live on my server.

Branchs - Dev - No Deployments for local use, Test - Deploys to Test, Master - Deploys to prod 

_Stack (Nuget Packages)_

![](https://d3efwhw5kd1q0b.cloudfront.net/Media/Stack.png)

Although not shown in the image above, But all of these packages for testing use the test package even if they are not a depdecny when pulling in the package it self. For the time being ill only build local versions of the packages.

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
* Catch all uncaught expectations and log them redacted

[https://github.com/msdickinson/DickinsonBros.Middleware](https://github.com/msdickinson/DickinsonBros.Middleware "https://github.com/msdickinson/DickinsonBros.Middleware")

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

## APIS

![](https://d3efwhw5kd1q0b.cloudfront.net/Media/Projects.png)

Working in order the Bus, and Web have zero depecnys. After them ill follow though with the others.

### DickinsonBros.RollerCoaster.API.Bus

...

**_Features_**

* ...
* ...
* ....

[https://github.com/msdickinson/DickinsonBros.DurableRest](https://github.com/msdickinson/DickinsonBros.DurableRest "https://github.com/msdickinson/DickinsonBros.DurableRest")

### DickinsonBros.RollerCoaster.API.Web

...

**_Features_**

* ...
* ...
* ....

[https://github.com/msdickinson/DickinsonBros.DurableRest](https://github.com/msdickinson/DickinsonBros.DurableRest "https://github.com/msdickinson/DickinsonBros.DurableRest")

### DickinsonBros.RollerCoaster.API.Account

...

**_Features_**

* ...
* ...
* ....

[https://github.com/msdickinson/DickinsonBros.DurableRest](https://github.com/msdickinson/DickinsonBros.DurableRest "https://github.com/msdickinson/DickinsonBros.DurableRest")

### DickinsonBros.RollerCoaster.API.Achievement

...

**_Features_**

* ...
* ...
* ....

[https://github.com/msdickinson/DickinsonBros.DurableRest](https://github.com/msdickinson/DickinsonBros.DurableRest "https://github.com/msdickinson/DickinsonBros.DurableRest")

### DickinsonBros.RollerCoaster.API.Report

...

**_Features_**

* ...
* ...
* ....

[https://github.com/msdickinson/DickinsonBros.DurableRest](https://github.com/msdickinson/DickinsonBros.DurableRest "https://github.com/msdickinson/DickinsonBros.DurableRest")

## Integreation Tests

### DickinsonBros.RollerCoaster.IntegrationTests

...

**_Features_**

* ...
* ...
* ....

[https://github.com/msdickinson/DickinsonBros.DurableRest](https://github.com/msdickinson/DickinsonBros.DurableRest "https://github.com/msdickinson/DickinsonBros.DurableRest")

## Deployment

* Builds
* 

DI

Proxy

SQL

Postman