---
layout: post
title: "TerminusDB: Importing, Reordering & Exporting a CSV"
date: 2020-07-13T18:17:34.790Z
description: Sarah unwarbels the widgets and delivers for Nathan
image: /blog/assets/uploads/terminators-e1.jpg
author:
  - value: luke
    label: Luke Feeney
tags:
  - database
  - data
  - graph
  - sarah
  - nathan
---
Remember [Sarah](https://youtu.be/xseqLEMF7YQ)? She’s a junior developer at Industrial Gizmos and Widgets. Her boss Nathan needs to figure out how to unwarble some widgets. It’s not an easy task and her senior team members don’t seem interested.

Industrial Gizmos and Widgets have some seriously antiquated IT systems and that makes everything harder.

The main gizmo and widget ERP is built on a DB2 that was first installed in 1986 when DB2 was still young. The database schema is out of control and the source code for the application has been lost. The only person that knows how to fix the DB2 was poached by IBM and they charge Industrial Gizmos and Widgets $2,500 per hour of her time.

![Database mess](/blog/assets/uploads/0-y0pbrnz9kyjcbz64.png)

The system is a mess.

Sarah thinks that what they really need is some sort of abstraction layer to allow them to better understand the important things in the ERP data — leave the DB2 chugging away and extract the key information into a knowledge graph. Much easier to manipulate and query once it’s there.

If they had such a tool, solving the warbled widgets problem would be relatively easy.

Sarah saw Cheuk’s [‘World of WOQLcraft’ stream on Twitch](https://www.twitch.tv/videos/578697754) and thought she could use some of the methods to solve the warbled widgets problem.

After watching Cheuk’s videos, Sarah ran [TerminusDB Quickstart](https://github.com/terminusdb/terminusdb-quickstart).

She opened up a terminal (or Git Bash for a Windows user — Sarah runs Ubuntu) and ran:

```git
git clone https://github.com/terminusdb/terminusdb-quickstart.git
cd terminusdb-quickstart
mkdir csvs
```

Sarah ran the following afterwards:

`TERMINUSDB_LOCAL="$(pwd)/csvs" ./terminusdb-container run`

Windows users can use this:

`TERMINUSDB_LOCAL=$(cmd //c cd)\\csvs ./terminusdb-container run`

The mkdir csvs command made sure that Sarah had a local directory - Sarah will save the output of this tutorial in that folder. That is also the purpose of the TERMINUSDB_LOCAL environment variable, it sets the directory for TerminusDB to mount to the local csvs directory.

After installing TerminusDB using Quickstart, Sarah opens the Console. The default address is <http://127.0.0.1:6363/console>.

The TerminusDB console appears in Sarah's browser:

![TerminusDB Console](/blog/assets/uploads/1-xwodx4vjqd2rfhjy4s6vyw.png)

Sarah creates a new database so she can import and manipulate her warbled widgets CSV. She calls it `ProjectSpreadsheet` .

![Database home page](/blog/assets/uploads/capture.jpg)

Next Sarah wants to eyeball the data to see what she is dealing with. So she [grabs the relevant csv extract](http://terminusdb.github.io/terminusdb-web-assets/tutorials/episode_1/items.csv). It is a mess:

![](/blog/assets/uploads/1-afcu7rbzqti3mf6b_4vbyq.png)

Sarah quickly writes a short WOQL query to load the CSV into TerminusDB, filter the data to only contain the warbled widgets, and export a new CSV.

Sarah knows that the warbled widgets contain the date “1970–01–01T00:00:00” as the DB2 was set up in 1986 and no widgets could possibly have been created at the company at that time!

Here is the query she uses:

```
WOQL.put(
    WOQL.as('Sku', 'v:Sku')
        .as('date_added', 'v:Date'),
    WOQL.and(
    WOQL.get(
        WOQL.as('sku', 'v:Sku')
            .as('date_added', 'v:Date')
            .as('category', 'v:Category')
    ).remote('http://terminusdb.github.io/terminusdb-web-assets/tutorials/episode_1/items.csv'),
        WOQL.eq('v:Date', '1970-01-01T00:00:00'),
        WOQL.eq('v:Category', 'widgets')
    ), WOQL.file('/app/local_files/warbled_widgets.csv'))
```

She puts it into the query pane on her TerminusDB console and uses the commit message `Importing, Ordering and Exporting CSV` so she knows what happens if she has to come back to check. She then hits `run query` .

![Query Pane](/blog/assets/uploads/1-x78t8j595bj3vwtenc5ccg.png)

The query returned in just under 2 seconds and Sarah found the exported CSV in the csvs directory in the terminusdb-quickstart folder. All tidied up as well:`warbled_widgets.csv` .

![](/blog/assets/uploads/1-_mlemhvuwa4s9zz5spzqga.png)

When she opens the CSV, Sarah found that it had been filtered just the way she wanted it:

![](/blog/assets/uploads/1-siz8rg_72p-ud0fboiqzmq.png)

Sarah took the csv and emailed it to her boss, Nathan. He rang her the next morning to thank her for her excellent work and, while he still called her Sandra, she was happy to have solved a meaningful problem.

Sarah thought — if I could do that so easily with TerminusDB, maybe I should take a deeper look.

See you soon for Episode 2!