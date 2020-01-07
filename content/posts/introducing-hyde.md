---
layout: post
title: Roller Coaster Overview (Part 1)
date: 2013-12-28T00:00:00.000+00:00

---
Roller Coaster 2020

# Roller Coaster

### Project Summary

Roller coaster is a game on a website that will allow you to build, ride, and share coasters. To accomplish this, I built versions of this game before with web assembly (Blazor C#) and WebGL (Three.Js). This time around I intend to build the game and surrounding services in the same way I would solve problems in my place of work. I will first focus on the surrounding services before working on the game itself.

Roller coasters stand outs in the past have been its flexibility and simplicity. Mainly the game is designed that you should be able to mash buttons and still complete a coaster. My latest version was built in 2018 at [http://rollercoaster.dickinsonbros.com/](http://rollercoaster.dickinsonbros.com/ "http://rollercoaster.dickinsonbros.com/"). This version only allows you to build roller coasters and the builder is not fully feature complete. In past versions you could ride the coaster as well.

![](/uploads/Tracks.PNG)

### User Stories

These user stories are desired features for the application from the type of user’s perspective. At a high level 4 domains came out with account, coasters, achievements, and reporting.

### Coding Principles

I have decided on 4 principles to drive the project. After and having many discussions with work colleagues I determined coding principles highly depend on our project. For example, I have a brother who writes mobile app games for a living. Many don’t store any user data like a username or a password, so security is less important. Additionally, putting in the extra effort to help ensure correctness, and that the game is maintainable may not be worth the effort until he knows the game is popular enough. This is totally acceptable as they are in a sense a prototype. I am instead aiming for a production ready system and these 4 principles fit with my user stories.

I have seen differing opinions on the degree of correctness and ability to maintain a project. My experience has shown me even for short lived projects It is generally worth the extra effort to ensure its correct and maintainable. Having to patch a project depending on the company even for a 1 liner can take meetings, coding, code reviews, releases and post release monitoring. Even if these can be done quickly it adds context switching to what you would have been working on. If this is done with low degrees throughout enough systems, you may find your self-spending most of your time patching.

### Conclusion

I have laid out the project overview and user stories and decided to build a production ready system over a prototype. I have created coding principles and items that to help measure and ensure they are meet. Finally, I added coding practices that I feel fit well with the coding principals. These high-level decisions will guide design decisions, and creation of the project.