---
layout: post
title: TerminusDB 4.2 - Encyclopaedia Galactica Release
date: 2021-02-25T15:20:01.405Z
description: ‘This one will take us to Titan’
image: /blog/assets/uploads/4.2_edited.png
author:
  - value: luke
    label: Luke Feeney
tags:
  - database
  - release
  - knowledge graph
---
*‘This one will take us to Titan’*

Today we’re excited to announce the release of TerminusDB 4.2!

For those who aren’t familiar with TerminusDB, it’s an open-source graph database and document store. With TerminusDB and TerminusHub you can collaboratively build data-intensive applications and knowledge graphs. It’s a distributed immutable data store with a powerful query language. You should use TerminusDB if you want to get your knowledge graph up and running fast; if you’re working with complex data; if time-travel, immutability, data lineage, or collaboration are important to your project; or if you need easy & powerful query. To learn more about TerminusDB you can [visit our website](https://terminusdb.com/) or our [github repo](https://github.com/terminusdb/terminusdb). We’re biased, but we think there is no better knowledge graph or knowledge collaboration technology anywhere in the world. And unlike most of the competition, we’re open source.

## Encyclopaedia Galactica

Based on the number of new features in TerminusDB 4.2, this could almost be a 5.0.

We’ll focus on the three big developments, but there are a bunch of smaller improvements that you can read about in [the release notes](https://github.com/terminusdb/terminusdb/blob/master/RELEASE_NOTES.md).

## Optimize!

TerminusDB is immutable - all the past states of the database are stored so you can easily query what the database looked like today, yesterday or last year. When querying your TerminusDB, the query engine looks through all that history to construct your data from all the additions and deletions contained in the past states. The big advantage of doing things this way, rather than modifying the data directly, is that we can reason about the changes to a database over time. We can show exactly what changes were made to the database, how they were made, by whom, and when.

All that history – especially if there are lots of commits – can slow queries over time. But fret no more – the Encyclopaedia Galactica Release solves the problem by introducing an optimize capability. This works by squashing those histories together which then appears to be exactly the same as the histories that it replaces. This allows queries to be much faster, while still making the collaboration and query features work.

This is a huge development and opens a range of interesting high assurance use-cases. With TerminusDB 4.2, you have a tamper-evident history system with proof of data inclusion and historical consistency in real time. Data ownership is now verifiable by clients, auditors, or anyone else you want.

## Large Files with Tus

TerminusHub is about collaborative data integration. It allows you to share, build and integrate knowledge graphs with your team and the wider world. Under the hood, we use [succinct data structures](https://terminusdb.com/t/papers/terminusdb-git.pdf) to enable sparing use of main memory resources. Even with prudent use of memory and sharing just database deltas on TerminusHub, we still ran into problems when handling uploads of large files. The connections were sometimes dropped or interrupted, and users were forced to start again.

TerminusDB 4.2 [implements Tus](https://github.com/terminusdb/tus)– a project that makes resumable file uploads easy. With Tus, TerminusHub can deal with large uploads even if those are from unreliable connections. In traditional uploading implementations progress would be lost in such situations but Tus enables you to recover from these interruptions and continue where the upload was stopped. We can now start to think about providing the ability to pause an upload. Best of all, we are growing an open-source standard and not relying on proprietary upload solutions. TerminusDB is committed to open source and open standards!

## Console

Lots of Terminators use clients or the CLI to interact with the database, but the main TerminusHub access is through the database console. We’re working hard to try to make this better and have introduced a new ‘manage’ tab that does exactly what it says on the tin – manage the database. You can optimize your database; you can squash, reset or delete branches.

The document interface in the console is something that we are quite proud of and will continue to develop. We think it offers a great low-code way for users to interact with their linked documents. Just surf through, find the links, visualize as a graph… simple. Some properties in your database can have different values. For example, an owner might be either a "Company" or a "Person". In these cases, the document viewer now allows the user to choose which type the property takes with a drop-down menu showing all the choices. The TerminusDB 4.2 console now has JSON support – just click to add your JSONs to your database. Everybody loves JSON (which is why I think Jason is the worst name for a DBA).

We’ve also squashed bugs, made the database faster and fixed some other bits so check out the release notes and take some time to explore! And come say hi in our [Discord Server](https://discord.gg/HynPgtY7Wg).

## Why Encyclopaedia Galactica

TerminusDB is named for the home planet of the [Foundation in the Asimov series](https://en.wikipedia.org/wiki/Foundation_series), so what better name than Encyclopaedia Galactica for this release. TerminusDB is a knowledge graph and that’s what those Encyclopaedists were building on Terminus. A compendium of all knowledge then available in the Galactic Empire, intended to preserve that knowledge in a remote region of the galaxy in the event of a foreseen galactic catastrophe. No need to wait for the Galactic Empire, TerminusDB 4.2 can be that for you today.