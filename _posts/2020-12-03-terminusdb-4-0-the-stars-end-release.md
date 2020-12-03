---
layout: post
title: TerminusDB 4.0 - The Star's End Release
date: 2020-12-03T14:31:58.044Z
description: TerminusDB 4.0 - The Star's End Release
image: /blog/assets/uploads/banner.png
author:
  - value: luke
    label: Luke Feeney
tags:
  - database
  - release
  - revision control
  - graph
  - CMS
  - content
  - versioning
  - collaboration
  - data
---
# TerminusDB 4.0 - The Star's End Release

**Version 4.0 of TerminusDB - the Star's End Release** * - is a big step forward in solidifying and extending the functionality of TerminusDB and TerminusHub.

[TerminusDB](https://terminusdb.com/) is an [open source](https://github.com/terminusdb/terminusdb), revision control graph database designed for distributed collaboration. TerminusDB allows you to push, pull, time-travel and merge data, much in the way that is possible with code in Git.

With this release TerminusDB takes a significant step towards our vision of a general-purpose tool for data & content management and collaboration.

TerminusDB 4.0 extends the revision control features to allow greater visibility and reproducability by allowing you to look inside individual commits to see what changes were made. Provenance, lineage, and compliance are increasingly essential in the management of data and content. 

The *major* additional features are:

### Model Building Tool

The database ships with an integrated visual schema building tool. It allows users to build complex data models using point and click tooling. This makes building models significantly quicker, more efficient, and inclusive of a broader range of people by lowering the technical bar for the application of business rules to your data. 

It lets you visually design knowledge graphs in the same way they are presented to business users. We have tried to make the tool easy to use, while preserving the ability to model the most complex domains. 

Like everything in TerminusDB, the models are versioned so you can make changes in testing and if it breaks your database, you can easily roll back to an earlier working version. This **collaborative knowledge graph design and implementation functionality** does not exist elsewhere. 

You can currently share your models/schemas through TerminusHub - the next step is to make modelling and collaboration even easier by providing data-libraries on TerminusHub, including schema.org but also basic standard models for common domains such as CRM, accounting, and inventory. Users can then generate and share models with collaborators or the broader public.

### Document Editor

TerminusDB 4.0 has full surfability, clickability and editability of database documents through the console. It is a wiki or catalogue of all your data that you can edit in place. You don't have to write code or execute a query, just edit the document directly. If you have a collaborative project, this is a useful approach to building and curating data assets.

The documents link to associated documents delivering a linked data application. You can filter documents, you can find the document you want using search, you can create a new document in the interface - it is a **general-purpose tool for managing your data**.

This is the beginning of the TerminusDB content management system, which we will be expanding over the coming months - next step is to provide web accessibility to TerminusHub databases and then a publisher API to provide the ability to publish the results of any query as a static content set.

### CSV Manager

With a single click, you can now build a database from a CSV or a group of CSVs. You can easily use TerminusDB and TerminusHub as a place to version and collaborate when working with CSV data. This will be especially useful with fast changing data as frequently used in machine learning and data science projects.

We have tried to make it as simple as possible for users to interact with the CSV tooling. You can view and edit CSVs directly in the document viewer. You can also add CSVs from this interface. No need to write code.

The database automatically spots if the CSV is already in the database and will just add the delta. There is an append mode that just adds the data that is not there and an update mode that updates the entire CSV.

**Manage your CSVs and other data via TerminusDB and Hub.**

### Command Line Interface

We are excited to launch the TerminusDB CLI. You can connect to TerminusDB with a Git-like CLI to run queries or use the revision control features. 

You can create databases and branches, you can list databases/branches, you can query in the command line and you can load CSVs. You also get full error reporting in the CLI. Use the CLI to import a CSV, commit changes and push the changes to TerminusHub.

The CLI will continue to develop and add new operations. We will make it easier to execute queries directly from the CLI. We're also planning to launch CLIs for the JavaScript and Python clients.



TerminusDB is a big step towards our ambition to be a complete decentralized data and content management system that is accessible to all. 

**END**

\* Star's End is a reference to the mysterious location of the Second Foundation in the Asimov series of novels. The home of the First Foundation was Terminus.  

![](/blog/assets/uploads/4.0_2.png)