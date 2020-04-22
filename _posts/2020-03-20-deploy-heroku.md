---
layout: post
title: "One Click Deploy TerminusDB to Cloud"
date: 2020-03-20
description:
cover_image: /blog/assets/images/francesco-ungaro-2ilsr-Qdfuw-unsplash.jpg
author: Cheuk Ting Ho
tags:
  - Database
  - Dataops
  - Cloud
  - Open Source
  - Deploy

---

Many people didn't know that you can deploy TerminusDB easily on cloud. To demonstrate, we have set up [this repo](https://github.com/terminusdb/terminus-heroku).

![GitHub repo: terminusdb/terminus-heroku](/blog/assets/images/terminus-heroku.png)

There is a button, which automatically pushes TerminusDB on a Heroku dyno. All you need to do is to fill in an app name and log in.

![Log in to Heroku](/blog/assets/images/heroku-login.png)

However, this repo is meant to be used for people to experiment, try out TerminusDB or for following tutorials. It's NOT meant for production which you may want more stable cloud cluster set up which let you preserve your data store. (We have productional TerminudDB on cloud for our internal projects).

So here you go, we made it easy for you to play around with TerminusDB and do some graph visualisation and analysis. Which tutorials do you want to try out?

* [My First TerminusDB Graph Visualisation — Bike Share Data](http://blog.terminusdb.com/2020/01/14/my-first-terminusdb-graph-visualisation-bike-share-data/)

* [TerminusDB graph visualisation of Dublin Council voting data](http://blog.terminusdb.com/2020/01/15/are-you-supporting-the-right-politician/)
