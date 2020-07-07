---
layout: post
title: Be like the mammals when the asteroid struck
date: 2020-07-07T15:29:40.636Z
description: From DataChemist to TerminusDB
image: /blog/assets/uploads/dinos.jpg
author:
  - value: luke
    label: Luke Feeney
tags:
  - database
  - startups
  - DataChemist
  - dinosaurs
---
I don’t care what anyone says, global pandemics are not a good time to get a database startup off the ground. There’s talk that some of the biggest internet companies started during a recession (thanks MongoDB), but goddammit a pandemic is different. Trade shows are a leveler – they want exciting startups hanging around, so they make pricing attractive. Meet-ups are a leveler. That’s why startups use those channels so effectively. Getting from 0 to 1 means finding people that really believe in your project and that’s hard in the vast expanse of the internet. 

I take some comfort from the fact that both Google and Salesforce started just before the dot com bust and got on pretty well in the long run – but, as I say, I’m not sure that we’ve seen anything as disruptive as this pandemic/civil strife mega-crisis and it sometimes feels like it’s still getting started.  

This blog is to explain to those that care what happened to DataChemist (a very limited set of people) and why you should be interested about TerminusDB. 

The great thing about working in a small enterprise is agility. We were work from home in early March and were able to quickly transition to the new world. 

But let me roll back a little (appropriate for a time-traveling database), we are a spin-out from Trinity College in Dublin. Our origins are in building data management systems for large scale research projects. We built the architecture behind [Seshat, the Global History Databank](http://seshatdatabank.info/). When we came out of the university system, our idea was to build large scale graph systems for the enterprise. We were focused on data quality and query - the ability to ingest complex data and then quickly return network-oriented queries (for example: give me all the entities that were connected to Goldman Sachs on January 14 in this large - > 1 billion nodes - commercial intelligence dataset)  - and we had some success in signing up enterprise customers. But we were drifting into a consultancy model where our engineers did most of the implementation, which is painful when working with varied complex internal systems. We were much more interested in being a technology partner. In Spring 2019, we decided to open source the database and spent the next few months getting everything ready for release (a nice euphemism for reimplementing all the code and building a custom storage layer).  

We also started to think about a name for the project – very important for software as the name is going to be in all sorts of code repositories etc. – we were convinced of the ‘DB’ suffix early as it does a lot of the product positioning for you (it’s a database). Andy Pavlo of Carnegie Mellon University has [an interesting guide to database naming](http://www.cs.cmu.edu/~pavlo/blog/2020/03/on-naming-a-database-management-system.html); he like ‘Postgres’ and ‘Clickhouse’ and calls his new DB first ‘PoopDish’ and then ‘BusTub’ as he wants memorable two-syllable names made up of one-syllable unrelated words. We didn’t follow that at all. Instead, we picked the name of a planet in the Asimov Foundation world, which is also the name of a Roman God, and went with that. We were nearly forced to change to RegulumDB (law in Latin, but in retrospect sounds like a laxative), but late in the day the French holders of the EU wordmark for Terminus came to the rescue and allowed us to use TerminusDB. RegulumDB is available for purchase to anybody that might want to launch a new database health supplement (clears out the buffers). 

After we open-sourced TerminusDB in October 2019, we pursued a dual open source and enterprise engagement strategy using both the DataChemist and TerminusDB names. This made sense as we were making some money from enterprise engagements and we wanted to grow the number of accounts under management. We also had a middle term bet on finding some SaaS customers via TerminusHub (‘GitHub for data’). The dual strategy was difficult to pursue during the pandemic. The enterprise product was reliant on the twin horsemen of enterprise sales and residency delivery. Enterprise sales for a complex product delivered by a startup requires considerable effort and many in-person meetings (to build confidence and form allies in the enterprise). In an environment such as this, the established partner has an enormous advantage. It also feels like digital transformation allies are weakened as enterprises just try to keep their lips above the rising tide. 

In practice, the dual strategy was a strain on the project. It was difficult to maintain two separate identities with different adoption strategies – one bottom-up and the other mostly top-down. Our potential impact was probably reduced (as we are early stage, it is hard to know exactly as the data is so limited).

We made the early call to drop DataChemist and the direct to enterprise channel so we could focus all our efforts on open source and TerminusDB. To be honest, as an engineering heavy university spin-out open source is a comfortable world. 

It wasn’t an easy decision. We had spent long hard hours building DataChemist software, building a public profile, building relationships with customers and potential customers. That’s a big pill to swallow – a large investment of time and effort (the two things startups lack most) disappears. In the front of house, where I work, it can feel like all your efforts were for nothing as you abandon the thing that you helped to create and with it whatever little brand equity you generated. The sunk costs problem becomes very real when you think about pivoting away from something that you spent countless hours building. Like a lot of tech startup folk, I’m well-schooled in the jargon of success after an intensive diet of business books and podcasts. ‘Low ego’ decision making seems to be almost universally considered a good. In this case, we tried to remove emotion by writing a memo that outlined the new strategy. Once it hit paper, the path became much clearer.  

When we were aligned, we quickly put the DataChemist brand on ice, redirected the website, went remote-first ([Dmytri wrote about that journey](https://terminusdb.com/blog/2020/03/24/reluctantly-remote/)) and haven’t looked back. The amazing team at TerminusDB (I know everybody says that, but you folk just don’t understand) see that the opportunity of distributed data collaboration is huge and that quickly responding to changing circumstances is essential if we’re going to succeed. 

We’ll be writing much more about this over the coming weeks, but revision control is the enabler of collaboration. With revision control, you can work on different versions of the same asset at the same time in a controlled way without risk of data loss. In Software Engineering, the Git revision control system has powered a revolution. For data people there is no equivalent – DevOps, DBAs, Data Scientists and others - everything is still manual migration scripts… like software in 2005. That’s the problem we’re solving. 

Since we made put all our eggs in the Open Source, TerminusDB and TerminusHub baskets at the beginning of the pandemic, we have made huge strides forward. But we are still tiny, so hopefully we can be like the mammals when the asteroid struck: small enough to not get wiped out and resilient enough to dominate the ecosystem once the dust has settled.