---
layout: post
title: Collaboration for structured Data
date: 2020-10-15T14:59:28.622Z
description: Structured data gives us the ability to surface the data we need to
  software which can then give it to people
image: /blog/assets/uploads/collaboration2.jpg
author:
  - value: gavin
    label: Gavin Mendel Gleason
tags:
  - database
  - graph database
  - revision control
  - collaboration
---
Structured data gives us the ability to surface the data we need to software which can then give it to people. Data analytics/data science is required for good decisions, but machines need to be able to eat the data. Currently, 80% of the time is taken in curation. Curation and editing to obtain high-quality data are therefore critical and it is facilitated by well-structured (schema controlled) data. Publishing of data requires easy access (query) and sufficient information to surface the data appropriately in graphs, charts, forms, web-pages, etc.

Currently many ML/AI shops use CSVs + email or slack for the storage and distribution of both data and features. This creates an enormous headache for AI practitioners for a number of reasons:

1.  Data versioning is \*required\* but not uniformly specified or facilitated. Usually, it ends up being a suffix on a file, rather than a proper version-controlled system as is now common with Code.
2. Data communication is undertaken by sending large files via slack or e-mail. This means multiple inconsistent versions can float around the team without any visibility to project managers, and without any assurance that it is the latest version.
3.  CSVs contain no type of information, so information is routinely marshaled into and out of formats leading to extra "cleaning" time and hard to find errors (NaNs) which may not even appear in the final feature selection.
4.  Feature selections can not be compared easily between approaches. All management has to be undertaken by the individual practitioners in their experiment and is not visible to the organization and has to be duplicated for each person involved.

TerminusDB solves these problems by creating a convenient repository for the storage and communication of complex data sets on a model similar to Git. We allow ML/AI programmers to easily obtain the latest data, enrich it with new features experimentally, record the methodology and store it in a versioned graph for longer-term comparison with other features or other methods of obtaining features. This is a critical infrastructure for evaluating the quality of changes to the ML/AI algorithms and approaches used - otherwise, comparison of effectiveness and avoiding quality regression becomes impossible.

Managing data means collaborating - we need databases designed for distributed collaboration. TerminusDB is exactly that, it's a data collaboration platform designed to facilitate AI pipelines. TerminusDB is competing with CSV+email+slack or in the case of rarer and more sophisticated AI/ML houses, with git, which is not designed for data, can not scale data, can not hold typing information consistently about the data, and can not be queried for the data.

If you're interested in an open-source solution to collaborative revision control for graphs or complex datasets, you should give [TerminusDB](https://terminusdb.com/) a try!