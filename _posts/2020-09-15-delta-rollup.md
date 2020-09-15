---
layout: post
title: Delta Rollups
date: 2020-09-15T13:16:07.625Z
description: Today’s post will be about a new feature that we’re working on
  called ‘delta rollups’. Delta rollups will hopefully make queries much faster
  in the future, especially when done on databases that have a lot of commits.
  We’ll probably roll out this feature in an upcoming minor release, so stay
  tuned!
image: /blog/assets/uploads/cover-1-.jpg
author:
  - value: Matthijs
    label: Matthijs Van Otterdijk
tags:
  - Database
---


Hi there! Matthijs here.

I’m one of the developers of TerminusDB, where I work on the backend. I’m the primary author of terminusdb-store, our storage backend, as well as a contributor to the database server. Unless things go terribly wrong, my work is largely invisible to people actually using TerminusDB, which is a shame, because it is fascinating stuff! Therefore, I have decided to start writing blog posts to highlight some of these things.

Today’s post will be about a new feature that we’re working on called ‘delta rollups’. Delta rollups will hopefully make queries much faster in the future, especially when done on databases that have a lot of commits. We’ll probably roll out this feature in an upcoming minor release, so stay tuned!

To describe what delta rollups are, we first need to discuss what problem it tries to solve. As you probably know, TerminusDB is append-only. This means that when you delete some data, that data is not actually thrown away. Instead, those deletions are saved in a new data layer on top of your old data. When querying, the query engine looks through all those layers to reconstruct your data from all the additions and deletions contained in the various layers. The big advantage of doing things this way, rather than modifying the data directly, is that we can actually reason about the changes done to a database over time. Among other things, this is what allows TerminusDB to be used collaboratively. It is what allows us to do branching, rebasing, pushing and pulling of data.

There is however a big disadvantage: the more commits in a database, the more layers queries need to search through to find data. Each layer adds some cost to the query. This is fine for a small number of layers, but when you have hundreds of commits, this really starts being noticeable. Roughly, query time scales linearly with the number of layers that need to be searched, so to keep query speeds low, we would rather not have so many layers.

A straightforward way to reduce the number of layers would be to squash them all together into one giant layer. This is an approach we actually already take for some of our own internal graphs, such as the system database. However, doing this throws away history. After doing a squash, it is no longer clear what sequence of operations led a database to be in a certain state. This is no problem for some internal databases where we don’t care about the history, but it is definitely a problem for any normal database. For example, if you were to squash your own database, and then tried to pull in changes from somebody else who worked off the original, the system would no longer be able to recognize the common history, and would reject the change.

What we need is a way to squash a layer, without throwing away the history. This is what delta rollups aim to achieve. A delta rollup is a squashed layer which to the system appears to be exactly the same as the layer stack that it replaces. It allows queries to be much faster, while still allowing all the collaboration features to work.

Let’s talk about what is required for delta rollups to deliver on this goal. First, it is important that the original changes aren’t thrown away. Delta rollups should be considered an optimization that is fully transparent. We don’t want to rewrite history. So rather than rewiring layer stacks to point at a new delta rollup layer, we’ll instead update the existing layer to point at its delta rollup. When loading the layer, the query engine will be able to detect this information and load the rollup version instead, all the while pretending to the rest of the system that it is looking at the original layer.

The second important feature for delta rollups to work properly is id stability. In terminusdb-store, each node, predicate or value string is assigned an id. Each layer stores newly added strings in dictionary files, where ids are assigned to them in the lexicographical order for that layer. Each layer then takes up an id range adjacent to its parent layer. So for example, the first layer takes up ids 1 through 10, the second layer 11 to 20, etcetera. This means that when you merge a bunch of dictionaries, the ids may change. Continuing our example, you’d now have a single layer with ids ranging from 1 to 20, and unless each string that originally came out of layer 2 is lexicographically ahead of each string from layer 1, those strings will now be interspersed, completely changing their ids.

We can’t just change the ids of strings. Suppose someone did some work based on the original layer, they may have done insertions and deletions with those strings, which are stored under the original id. If we were to change ids on delta rollup, such a change could no longer be pulled in cleanly. Therefore, delta rollups will store an id remap table, mapping the internal id to an external one and back.

The third important bit in making delta rollups work properly is dealing with queries that investigate the additions and removals to a layer. We can’t just query the delta rollup layer for these, since the delta rollup layer has a different set of additions and removals than the layer it replaces. In order to support these operations, we are introducing a way to load in the original layer on-demand, without loading in the entire layer stack. Addition and removal queries can then transparently use this layer delta, rather than the delta rollup layer.

In order to support all this, we’ve been hard at work over the past few weeks to revamp a lot of our internals. A lot of the logic dealing with building and querying of layers has been refactored to be much more modular so that it can be recombined in a different way to support the building and querying of delta rollup layers. As a side-effect, these changes should also bring in general query speedups, so everybody wins!

So that’s it! Delta rollups, coming soon to a TerminusDB near you! May it grace your data with ever-improving query speeds!