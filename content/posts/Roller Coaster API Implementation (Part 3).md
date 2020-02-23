+++
date = 2020-01-25T07:00:00Z
draft = true
layout = "post"
title = "Roller Coaster Cross Cutting Concerns (Part 3)"

+++
## Cross Cutting Concerns

Roller coaster has multiple packages and APIs and all of them require testing. Building out a stack will help me follow SOLID by not repeating myself. I covered in my last post how I came up with these packages and the high-level design designs.

![](https://www.marksdickinson.com/Media/Part3-Design.png)

### Building Early Vs Agile

Let’s look at agile pros and cons to building packages.

_Agile Upsides_

* You don’t build things you don’t need. As you won’t know what you need until you actually need it.
* No large upfront cost of building out packages up front. Pay as you go model.

_Agile Downsides_

* Iterating on a package as needed can cause breaking changes, undesirable architecture, over extending, and harder to use and maintain.
* Increased amount of context switching between the API your working and the packages you need to update.

Agile for me has a place but what is often overlooked is design and prototyping outside a small scoped story. My experience as shown me systems often referred to as “the public toilet” and other undesirable terms when code follows SOLIDS extend and agile approach to strongly. Avoiding refactoring and/or breaking changes for too long can cause code rot and over time lock a system down. Working with such a system can cause massive increase to development time and risk.

For my packages I am going to ignore SOLIDS extend and prefers breaking changes whenever it improves maintainability. I have taken time to do extensive planning and prototyping to scope in features. I have built out my packages more then I need today ignoring agile. I consider this in experiment as I have not worked this way before but expect less context switching and more code churn with increased maintainability.
<br><br/>

### Continuous Integaretion

<br><br><br><br>

#### Local

For building local I created a powershell script to drop my packages into a root folder "C:\\Packages" and I add "ci-" with a datetime stamp to ensure quick development.

![](https://www.marksdickinson.com/Media/Part3-Local.png)

![](https://www.marksdickinson.com/Media/Part3-LocalPower.png)

#### Production

My production pipeline should ensure the project builds, tests pass and releaseing to nuget. A major learning point is the power of a release pipeline such as azure devops releases. It gives you addtional check points that you can require manaul intervention. Although currently it is not nesseary for the pacakges it will be for the APIS to come.

![](https://www.marksdickinson.com/Media/Part3-CIPipeline.png)

##### Walking though the Steps

1. Creating a Pull Request kicks off “Build and Unit Test” Pipeline

   ![](https://www.marksdickinson.com/Media/Part3-GitHubPull.png)
   ![](https://www.marksdickinson.com/Media/Part3-BuildRun.png)
2. When the pipeline is able to build and pass all unit tests you can merge it

   ![](https://www.marksdickinson.com/Media/Part3-GitHubBuildPass.png)
3. Once merged the pipeline publish release will run. This will create a release artifact

   ![](https://www.marksdickinson.com/Media/Part3-PipelinePublish.png)
4. When a release pipeline is successful a release will start utilizing the artifacts creates in the release.

   ![](https://www.marksdickinson.com/Media/Part3-Release.png)

Note This step will fail If you do modify the version in the projs config. This is by design to ensure I don’t release nuget packages without following sematic versioning.

### Projects

#### DickinsonBros.Test

A wrapper library for DateTime

**Features**

* DI unit tests in a clean fashion
* Easily pull out mocks
* Unit test controllers by injecting headers, claims and taking over the HttpContext for more precise unit tests

[https://github.com/msdickinson/DickinsonBros.Test](https://github.com/msdickinson/DickinsonBros.Test "https://github.com/msdickinson/DickinsonBros.Test")

#### DickinsonBros.DateTime

A wrapper Library for DateTime

**_Features_**

* Adds extensibility via abstraction
* Allows for unit testing

[https://github.com/msdickinson/DickinsonBros.DateTime](https://github.com/msdickinson/DickinsonBros.DateTime "https://github.com/msdickinson/DickinsonBros.DateTime")

[https://github.com/msdickinson/DickinsonBros.DateTime.Abstractions](https://github.com/msdickinson/DickinsonBros.DateTime.Abstractions)
<br /> <br />

#### DickinsonBros.Encryption

Encrypt and Decrypt strings

**_Features_**

* Certificate based encryption
* Configure certificate location

[https://github.com/msdickinson/Dickinsonbros.Encryption](https://github.com/msdickinson/Dickinsonbros.Encryption "https://github.com/msdickinson/Dickinsonbros.Encryption")

[https://github.com/msdickinson/DickinsonBros.Encryption.Abstractions](https://github.com/msdickinson/DickinsonBros.Encryption.Abstractions)

#### DickinsonBros.Guid

A wrapper Library for DateTime

* Adds extensibility via abstraction
* Allows for unit testing
* Sperate abstractions library to reduce coupling of packages.

[https://github.com/msdickinson/DickinsonBros.Guid](https://github.com/msdickinson/DickinsonBros.Guid "https://github.com/msdickinson/DickinsonBros.Guid")

[https://github.com/msdickinson/DickinsonBros.Guid.Abstractions](https://github.com/msdickinson/DickinsonBros.Guid.Abstractions)

#### DickinsonBros.Redactor

A redactor that can take a json string or an object and return a redacted string in json.

**_Features_**

* Configurable properties to redact by name
* Configurable regular expressions to validate against
* Sperate abstractions library to reduce coupling of packages.

[https://github.com/msdickinson/DickinsonBros.Redactor](https://github.com/msdickinson/DickinsonBros.Redactor "https://github.com/msdickinson/DickinsonBros.Redactor")

[https://github.com/msdickinson/DickinsonBros.Redactor.Abstractions]()

#### DickinsonBros.Logger

A logging service that redacts all logs

**_Features_**

* Redacts all logs
* Allows for dictionary to become first class propertys in the log.
* Ability to add a correlation id that works though async in straight forward fashion
* Allows for improved testability
* Sperate abstractions library to reduce coupling of packages.

[https://github.com/msdickinson/DickinsonBros.Logger](https://github.com/msdickinson/DickinsonBros.Logger "https://github.com/msdickinson/DickinsonBros.Logger")

[https://github.com/msdickinson/DickinsonBros.Logger.Abstractions]()

#### DickinsonBros.Middleware

Middleware for ASP.Net that adds redacted logging.

**_Features_**

* Logs requests redacted
* Logs responses redacted and Status Codes
* creates collreation Id
* Catch all uncaught expectations and log them redacted

[https://github.com/msdickinson/DickinsonBros.Middleware](https://github.com/msdickinson/DickinsonBros.Middleware "https://github.com/msdickinson/DickinsonBros.Middleware")

#### DickinsonBros.SQL

SQL abstraction that adds increased logging on exceptions

**_Features_**

* Improved Logs
* Allows for improved testability

[https://github.com/msdickinson/DickinsonBros.SQL](https://github.com/msdickinson/DickinsonBros.SQL "https://github.com/msdickinson/DickinsonBros.SQL")

[https://github.com/msdickinson/DickinsonBros.SQL.Abstractions](https://github.com/msdickinson/DickinsonBros.SQL.Abstractions)

#### DickinsonBros.DurableRest

SQL abstraction that adds increased logging on exceptions

**_Features_**

* Ability to retry requests
* Timeouts
* Logs all requests redacted with meta data

[https://github.com/msdickinson/DickinsonBros.DurableRest](https://github.com/msdickinson/DickinsonBros.DurableRest "https://github.com/msdickinson/DickinsonBros.DurableRest")

[https://github.com/msdickinson/DickinsonBros.DurableRest.Abstractions](https://github.com/msdickinson/DickinsonBros.DurableRest.Abstractions)
# 
#### DickinsonBros.Stopwatch

A wrapper library for Stopwatch

**_Features_**

* Adds extensibility via abstraction
* Allows for unit testing

[https://github.com/msdickinson/DickinsonBros.Stopwatch](https://github.com/msdickinson/DickinsonBros.Stopwatch)

[https://github.com/msdickinson/DickinsonBros.Stopwatch.Abstractions](https://github.com/msdickinson/DickinsonBros.Stopwatch.Abstractions)

##