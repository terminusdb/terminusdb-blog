---
layout: post
title: "TerminusDB — what’s in a name?"
date: 2019-08-21
description:
image: /assets/images/terminusdb_logo.png
author: Luke Feeney
tags:
  - Machine Learning
  - Terminus
  - Database
  - Data
  - Development

---
TerminusDB is an open source model driven graph database for knowledge graph representation designed specifically for the web-age. Our launch date is 1 October later this year.

![TerminusDB logo](/assets/images/terminusdb_logo.png)

The core development team is busy putting the pieces into place and we are all really excited about bringing TerminusDB to the world.

We’ll be pushing out plenty of content and documentation about the capabilities of the database over the coming weeks.

We have long dreamed of a future where databases learn by automatically extracting information both from existing networked data-sources and from their users to continuously improve the reliability and accuracy of the information they contain. We wanted to build a database that would be capable of automatically generating sophisticated curation and analysis tools to allow users to access and manage the data they contain and to feed the results of these analyses into real-time automated workflows and learning algorithms. A big ask, but TerminusDB will be a strong step towards making it a reality.

![BDFL Gavin Mendel-Gleason](https://miro.medium.com/max/1334/1*HleRZdBTjZqwcOBkeY8x2g.jpeg)

Our Benevolent Dictator for Life is the one and only Gavin Mendel-Gleason. He’s a database maniac who dreams of logic, mathematics and formal methods. Gavin and the rest of the team have poured years of high-intensity work into building TerminusDB. Considerable innovation — both theoretical and practical — was required to build the database. Not only did we end up having to write the software ourselves, along the way we had to develop and formalise a new theoretical model of computer programming for data intensive systems; develop, formalise and prove a new modular logic for data objects; and devise, develop and deploy a new general purpose logical approach to describing and querying graph shapes and constraining their transitions. It has been a labour of love by all the contributing team.

![Immovable & Persistent](https://miro.medium.com/max/672/1*f8egU0CASEEqGPmvma8Gyg.jpeg)

Terminus was the Roman god of boundaries. He is an old Roman god — probably introduced by Romulus after the founding of the city. There is a famous story told of the last king of Rome (so disliked & proud that he was overthrown and the Republic declared) — he wanted to reshape Rome as an imperial capital and engaged in a large scale building programme. As part of his works, he set about reconstructing the Temple of Jupiter on the Capitol Hill. Tarquin thought that constructing a massive temple over the remains a holy site consecrated by Romulus would cement his place as a great King of Rome. To facilitate the reconstruction, the older shrines to various gods had to be removed; however, when they attempted to move the shrine of Terminus, it would not budge. Terminus was the immovable rock of the Capitol. Even Jupiter, the king of the gods, had to kneel to Terminus.

![The Feast Before the Altar of Terminus. (c. 1642) By Giovanni Benedetto Castiglione (1609–1664)](https://miro.medium.com/max/1658/1*lF_rZhXDtYOeBrEV1_3qLw.jpeg)

Ovid, the great Roman poet, writing 500 years later, interpreted these events:

> ‘What happened when the new Capitol was built?

> Where he was found, and shares the temple with great Jupiter.

> Even now there’s a small hole in the temple roof,

> So he can see nothing above him but stars.

> Since then, Terminus, you’ve not been free to wander:

> Stay there, in the place where you’ve been put,

> And yield not an inch to your neighbour’s prayers,

> Lest you seem to set men above Jupiter.’

Terminus, as the God of boundaries, is particularly appropriate for our database management system. All good databases need boundaries — you need to know what’s in there and what isn’t. One of our major innovations in building TerminusDB is implementing a closed world version of OWL, the web ontology language. In the semantic web academic tradition, OWL has an open world assumption which means that the truth value of a statement may be true irrespective of whether or not it is known to be true. For TerminusDB any statement that is true must also be known to be true. This makes the database practical and usable for a broad range of industrial applications.

![Design for a Stained Glass Window with Terminus. (31 December 1524) By Hans Holbein the Younger.](https://miro.medium.com/max/844/1*J__EDmwjRNNVu4EkAbUEXA.jpeg)

The Roman God Terminus’ slogan ‘Concedo Nulli’ — concede to nobody — is also particularly appealing to the core TerminusDB team. As anybody who has delivered a production ready database will know, you have to walk through walls to get it done and yield to nobody. We bear the scars of those battles.

![Terminus is the capital planet of the 1st Foundation](https://miro.medium.com/max/600/1*FTRE73m34GjDViAY1W25AA.jpeg)

Asimov fans will also recognise Terminus as the home planet of the Foundation. Hari Seldon, inventor of Psychohistory, predicted a 30,000 year interregnum of barbaric dark ages until the rise of a New Empire. Seldon created a plan to shorten this interregnum from 30,000 years to 1,000 years. The Foundation was set up as an organisation intent on preserving all of human knowledge during the 1,000 years of chaos. Seldon’s true intention for the Foundation was to preserve the sciences and provide a political starting point for the Second Empire. The Foundation grows from Terminus to become a federation of planets under the direct control of Terminus or under its sphere of influence.

TerminusDB powers Seshat, the Global History Databank, which is like the Foundation in that Seshat is dedicated to bringing together the most current and comprehensive body of knowledge about human history in one place. The massive collection of historical information allows researchers to rigorously test different hypotheses about the rise and fall of large-scale societies and human history. The core TerminusDB team are active in the Cliodynamics community. Cliodynamics treats history as science — practitioners develop theories that explain such dynamic historical processes, translate these theories into mathematical models and then test predictions against data.

![‘Ages of Discord’ by Peter Turchin](https://miro.medium.com/max/1814/1*kCm-Owh43PHf5h7UF-Oy8w.jpeg)

The father of Cliodynamics, Peter Turchin, recently authored a book ‘Ages of Discord’, which uses cliodynamic approaches and the data in the Global History Databank to give an insight into the future.

The parallels to Asimov’s psychohistory are very strong. Cliodynamics is the real world version of psychohistory and Seshat is the real world version of the Foundation. And TerminusDB is the analogue of Terminus in the Foundation universe.

We want people around the world to use our database, but we also want you to contribute to the project. We believe in the ideals of the free software movement. We’ll therefore release TerminusDB under GPL V3. That will allow everybody to inspect and use the full code base.

We believe in a data-centric approach to development. Hoarding data in proprietary and complex applications is a mistake. Validation logic should be embedded in the data layer where it belongs and not housed in expensive to maintain application code. Long term cost of data ownership has been a huge consideration as we worked on this project.
TerminusDB contributors will be posting on this blog over the coming weeks and months.

![we can do it](https://miro.medium.com/max/400/1*_xTYaO0GlB_pyVCzb1AMJg.jpeg)

Join the data-centric revolution!
