+++
date = 2020-02-25T15:00:00Z
layout = "post"
title = "Roller Coaster Cross Cutting Concerns (Part 3)"

+++
## Overview

In [part 1](https://www.marksdickinson.com/posts/roller-coaster-overview-part-1.html "part 1") we walked through the projects summary, user stories and principles. Then in [part 2](https://www.marksdickinson.com/posts/roller-coaster-design-decisions-part-2.html "part 2") we walked though design decisions, and came up with a set of cross cutting concerns after prototyping.

This post will focus on the cross-cutting concerns (packages), building packages early, continuous integration with links to all builds, releases, and packages.

## Cross Cutting Concerns

Roller coaster has multiple packages and APIs and all of them require testing. Building out a stack will help me follow SOLID by not repeating myself. As mentioned in [part 2](https://www.marksdickinson.com/posts/roller-coaster-design-decisions-part-2.html "part 2") The abstraction layer (a set of interfaces) is built to help reduce coupling.

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

For my packages I am going to ignore SOLIDS extend and prefer breaking changes whenever it improves maintainability. I have taken time to do extensive planning and prototyping to scope in features. I have built out my packages more then I need today ignoring agile. I consider this in experiment as I have not worked this way before but expect less context switching and more code churn with increased maintainability.

### Continuous Integration

#### Local

For building local I created a PowerShell script to drop my packages into a root folder "C:\\Packages" and I add "ci-" with a datetime stamp to ensure quick development.

![](https://www.marksdickinson.com/Media/Part3-Local.png)

![](https://www.marksdickinson.com/Media/Part3-LocalPower.png)

#### Production

This pipeline will build the projects, run the tests and release to NuGet. A major learning point is the power of a release pipeline such as azure devops releases. It gives you additional check points that you can require manual intervention. Although currently it is not necessary for the packages it will be for the APIS to come.![](https://www.marksdickinson.com/Media/Part3-CIProd2.PNG)

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

Note This step will fail If you do modify the version in the projs config. This is by design to ensure I don’t release NuGet packages without following sematic versioning.

### Project Layout

Each project has the same general layout.

1. A library folder containing the library project and test project
2. A misc. folder containing both builds, unit test settings, and a prerelease power PowerShell script to be able to run your CI local.

![](https://www.marksdickinson.com/Media/Part3-ProjectOverview2.png)

### Projects

Below I have listed all packages completed with a summary and link to their source code.

All Builds are viewable at [Pipelines (Builds)](https://dev.azure.com/marksamdickinson/DickinsonBros/_build?view=folders "Pipelines (Builds)")

All Releases are viewable at [Releases](https://dev.azure.com/marksamdickinson/DickinsonBros/_release?_a=releases&view=all&definitionId=15 "Releases")

#### DickinsonBros.Test

A test library to assist with unit testing

* DI unit tests in a clean fashion
* Easily pull out mocks
* Unit test controllers by injecting headers, claims and taking over the HttpContext for more precise unit tests

[https://github.com/msdickinson/DickinsonBros.Test](https://github.com/msdickinson/DickinsonBros.Test "https://github.com/msdickinson/DickinsonBros.Test")

#### DickinsonBros.DateTime

A wrapper Library for DateTime

* Adds extensibility via abstraction
* Allows for unit testing

[https://github.com/msdickinson/DickinsonBros.DateTime](https://github.com/msdickinson/DickinsonBros.DateTime "https://github.com/msdickinson/DickinsonBros.DateTime")
[https://github.com/msdickinson/DickinsonBros.DateTime.Abstractions](https://github.com/msdickinson/DickinsonBros.DateTime.Abstractions)

#### DickinsonBros.Encryption

Encrypt and Decrypt strings

* Certificate based encryption
* Configure certificate location

[https://github.com/msdickinson/Dickinsonbros.Encryption](https://github.com/msdickinson/Dickinsonbros.Encryption "https://github.com/msdickinson/Dickinsonbros.Encryption")
[https://github.com/msdickinson/DickinsonBros.Encryption.Abstractions](https://github.com/msdickinson/DickinsonBros.Encryption.Abstractions)

#### DickinsonBros.Guid

A wrapper Library for Guid

* Adds extensibility via abstraction
* Allows for unit testing

[https://github.com/msdickinson/DickinsonBros.Guid](https://github.com/msdickinson/DickinsonBros.Guid "https://github.com/msdickinson/DickinsonBros.Guid")
[https://github.com/msdickinson/DickinsonBros.Guid.Abstractions](https://github.com/msdickinson/DickinsonBros.Guid.Abstractions)

#### DickinsonBros.Redactor

A redactor that can take a json string or an object and return a redacted string in json.

* Configurable properties to redact by name
* Configurable regular expressions to validate against

[https://github.com/msdickinson/DickinsonBros.Redactor](https://github.com/msdickinson/DickinsonBros.Redactor "https://github.com/msdickinson/DickinsonBros.Redactor")
[https://github.com/msdickinson/DickinsonBros.Redactor.Abstractions](https://github.com/msdickinson/DickinsonBros.Redactor.Abstractions "https://github.com/msdickinson/DickinsonBros.Redactor.Abstractions")

#### DickinsonBros.Logger

A logging service that redacts all logs

* Redacts all logs
* Allows for dictionary to become first class properties in the log.
* Ability to add a correlation id that works though async in straight forward fashion
* Allows for improved testability

[https://github.com/msdickinson/DickinsonBros.Logger](https://github.com/msdickinson/DickinsonBros.Logger "https://github.com/msdickinson/DickinsonBros.Logger")
[https://github.com/msdickinson/DickinsonBros.Logger.Abstractions]()

#### DickinsonBros.Middleware

Middleware for ASP.Net that adds redacted logging.

* Logs requests redacted
* Logs responses redacted and Status Codes
* Creates correlation Id
* Catch all uncaught expectations and log them redacted

[https://github.com/msdickinson/DickinsonBros.Middleware](https://github.com/msdickinson/DickinsonBros.Middleware "https://github.com/msdickinson/DickinsonBros.Middleware")

#### DickinsonBros.SQL

SQL abstraction that adds increased logging on exceptions

* Improved Logs
* Allows for improved testability

[https://github.com/msdickinson/DickinsonBros.SQL](https://github.com/msdickinson/DickinsonBros.SQL "https://github.com/msdickinson/DickinsonBros.SQL")
[https://github.com/msdickinson/DickinsonBros.SQL.Abstractions](https://github.com/msdickinson/DickinsonBros.SQL.Abstractions)

#### DickinsonBros.DurableRest

Handles requests in a durable fashion with reporting

* Ability to retry requests
* Timeouts
* Logs all requests redacted with meta data

[https://github.com/msdickinson/DickinsonBros.DurableRest](https://github.com/msdickinson/DickinsonBros.DurableRest "https://github.com/msdickinson/DickinsonBros.DurableRest")
[https://github.com/msdickinson/DickinsonBros.DurableRest.Abstractions](https://github.com/msdickinson/DickinsonBros.DurableRest.Abstractions)

#### DickinsonBros.Stopwatch

A wrapper library for Stopwatch

* Adds extensibility via abstraction
* Allows for unit testing

[https://github.com/msdickinson/DickinsonBros.Stopwatch](https://github.com/msdickinson/DickinsonBros.Stopwatch)
[https://github.com/msdickinson/DickinsonBros.Stopwatch.Abstractions](https://github.com/msdickinson/DickinsonBros.Stopwatch.Abstractions)