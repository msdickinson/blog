---
layout: post
title: Roller Coaster Overview (Part 1)
date: 2013-12-31T00:00:00+00:00

---
# Roller Coaster

### Project Summary

Roller coaster is a game that allows you to build and ride coasters. Over the past 8 years I have had a version on warcraft 3, windows phone, and the web. Roller coaster has brought in over 1 Million downloads on windows back when windows phone was a thing. My latest version in 2018 was a web version using web assembly (Blazor C#) and WebGL (Three.Js). My next version will allow you build, ride and share coasters on the web.

12 months ago, I entered a new job and I joined a team with significantly different practices then what I was a custom to. Eventually most of those practices sunk in. There is a strong difference though between following practices, high level design decisions, and packages that were largely set in place vs going though that process myself.

This project primary purposes are growing my skills by working though design decisions and practicing my craft. Build a portfolio project at a professional grade, create a foundation for other projects to come and to create a successful (100 new accounts daily) roller coaster game.

[http://rollercoaster.dickinsonbros.com](http://rollercoaster.dickinsonbros.com "http://rollercoaster.dickinsonbros.com") (2018 Version - Builder Only)

![](/uploads/Tracks.PNG)

### User Stories

For now, I will only scope in the new features instead of the game itself that does the building, riding and rendering. These user stories are desired features for the application from the user’s perspective.

_Account_

* Create Account (Email Optional)
* Login
* Reset Password (If Email Exist)
* Update Password
* Update Email Settings directly from all emails
* Update Email Settings when logged in

_Coaster_

* Create
* Save
* Load
* Share (Private Link)
* Publish (Public)
* Find and rate published coasters
* Get notifications when your coaster is is ridden or rated

_Achievement_

* Gain achievements
* View achievements
* Get notification when you receive an achievement

### Principles

After much consideration and speaking with many colleagues I determined principles highly depend on our project. For example, I have a brother who writes mobile app games for a living and many of his projects do not store any personal data or passwords. Putting in the extra effort to help ensure secure, correct and that the game is maintainable may not be worth the effort until he knows the game is popular enough. This is totally acceptable as many of them are in a sense a prototype. I am aiming for a production ready system that contains some personal data and this drove me to the following principles.

_Secure_

* Modern password encryption
* Login attempts guarded
* Communication encrypted
* No personally identifiable information (PII) In logs or reporting database

_Correct_

* Testing - Unit, Integration, Manual
* Multiple environments
* Blue / Green Deployment
* High fidelity logging
* Durable

_Service Level Agreement_

* Load testing
* Monitoring

_Maintenance_

* High fidelity logging (Yes this in here twice)
* Reporting - accounts, coasters, achievements, and system health
* Source control
* Continuous integration
* Documentation
* Health checks

For production applications I have seen differing opinions on the degree of correctness and ability to maintain a project. Lower quality projects tend to have more patching and having to patch even for a 1 liner can take meetings, coding, code reviews, releases and post release monitoring. My experience has shown me It is worth the extra effort to ensure its secure, correct, handles expected load, and is maintainable.

### Conclusion

I have laid out the project and user stories and decided to build a production ready system. I have created principles and items that will help me measure and ensure they are meet. These high-level decisions will guide design decisions and creation of the project.