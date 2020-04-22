---
layout: post
title: "Loading your local files in TerminusDB"
date: 2020-01-21
description:
image: /blog/assets/images/cowduck_banner.jpeg
author: Cheuk Ting Ho
tags:
  - Data Science
  - Database
  - Data
  - Developer
  - Tools

---
We have got lot’s of feedback since we have posted our tutorial — [My First TerminusDB Graph Visualisation](https://medium.com/terminusdb/my-first-terminusdb-graph-visualisation-bike-share-data-39c59a1ab86a?source=friends_link&sk=2f877df5dcb2f00b9e4e85d5088f015e). One of the most common questions is how to load a local CSV in the hard drive into your TerminusDB running in a local docker container. So here is how to do it.

-----------------------

If your container is running stop it with:

```
./terminus-container stop
```

> Extra tip: you may want to run `./terminus-container rm-config` now before you mount the volume. This remove old settings to advoid version conflits that may comes in later if there’s a newer version of Docker image avaliable.

set the `TERMINUS_LOCAL` environment variable to the path to the parent directory of your csv:

```
export TERMINUS_LOCAL=/home/me/somewhere
```

>for Windows users:
```
export TERMINUS_LOCAL=c:/Users/you/somewhere
```
>Go to Docker Desktop Settings and make sure that the Drive (i.e. ‘C’) your file on is enabled in Shared Drives

![Windows setup](https://miro.medium.com/max/2474/1*YYgIRufPKRw724nIuWRHzw.png)
Make sure the ‘C’ is clicked and ‘Apply’

then start your container again:
```
./terminus-container run
```
now, `TERMINUS_LOCAL` will be mounted as `/app/local_files`
so, in your WOQL query, you can use:
`.file(/app/local_files/myfilename.csv)` instead of `.remote()`

---------------------------

Hope it helps with lot’s of you who want to load csv locally.
Thanks [Dmytri Kleiner](https://twitter.com/dmytri/status/1219391128287555586) for figuring it out!
