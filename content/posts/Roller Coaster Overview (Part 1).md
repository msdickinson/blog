---
layout: post
title: Roller Coaster Overview (Part 1)
date: 2013-12-24T00:00:00+00:00

---
# Roller Coaster

### Project Summary

Roller coaster is a game on a website that will allow you to build, ride, and share coasters. To accomplish this, I built versions of this game before with web assembly (Blazor C#) and WebGL (Three.Js). This time around I intend to build the game and surrounding services in the same way I would solve problems in my place of work. I will first focus on the surrounding services before working on the game itself.

Roller coasters stand outs in the past have been its flexibility and simplicity. Mainly the game is designed that you should be able to mash buttons and still complete a coaster. My latest version was built in 2018 at [http://rollercoaster.dickinsonbros.com/](http://rollercoaster.dickinsonbros.com/ "http://rollercoaster.dickinsonbros.com/"). This version only allows you to build roller coasters and the builder is not fully feature complete. In past versions you could ride the coaster as well.

![](/uploads/Tracks.PNG)

### User Stories

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

### Principles

After much consisdartion and speaking with many collagues I determined coding principles highly depend on our project. For example, I have a brother who writes mobile app games for a living. Many of his projects do not store any personal data or passwords. Additionally, putting in the extra effort to help ensure correctness, and that the game is maintainable may not be worth the effort until he knows the game is popular enough. This is totally acceptable as many of them are in a sense a prototype. I am aiming for a production ready system that contains some pessonal data this drove me to the following project principles. a users email and password and landed on decided on these principles.

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

I have seen differing opinions on the degree of correctness and ability to maintain a project. I am of the thought that you can not be secure if you can not ensure correctness. Having to patch even for a 1 liner can take meetings, coding, code reviews, releases and post release monitoring. My experience has shown me It is generally worth the extra effort to ensure its secure, correct, handles expected load (SLA), and is maintainable. 

### Conclusion

I have laid out the project and user stories and decided to build a production ready system. I have created principles and items that to help measure and ensure they are meet. These high-level decisions will guide design decisions and creation of the project.