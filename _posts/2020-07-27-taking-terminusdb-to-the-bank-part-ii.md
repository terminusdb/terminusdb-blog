---
layout: post
title: Taking TerminusDB to the bank (Part II)
date: 2020-07-27T14:52:29.459Z
description: Using the collaboration features of TerminusDB
image: /blog/assets/uploads/collaborate.jpg
author:
  - value: luke
    label: Luke Feeney
tags:
  - database
  - collaborate
  - terminusdb
  - terminushub
  - graph
  - data
  - ""
---
**THIS IS A EARLY RELEASE TUTORIAL TO ACCOMPANY TERMINUSHUB PRIVATE BETA PROGRAM. CONTACT US IF YOU WOULD LIKE TO GET ACCESS**

In the first installment we created a branch_office branch where we added some data. But the strength of the distributed model of TerminusDB is that others can work on data assets in parallel. To make use of this powerful feature, we need to make alterations in a different database.

Since we are using collaboration features, first you will need to log into hub. You can do that by clicking on "Connect to Hub" in the right hand corner.

We'll start with the database as created in Part 1. If you don't have this database anymore, you can rebuild it be following the steps in [part 1 of this tutorial](https://terminusdb.com/blog/2020/07/27/taking-terminusdb-to-the-bank/).

First step - make sure you share the database to hub.

![](/blog/assets/uploads/upload-to-hub.jpg)

# Distributed updates

Click on your "Bank Balance Example" and go to the synchronise page. Select "Push", and then select the branch we named `branch_office` both locally and on the remote and click the button "Push Updates".

![](/blog/assets/uploads/branch-office-on-hub.jpg)

Now we have the data on hub! If you go to the main "Bank Balance Example" page, you can see the remote URL in the box on the right which says "origin Remote". Copy this URL, we're going to make a clone.

![](/blog/assets/uploads/pushed.jpg)

Now go to the main TerminusDB page and click on clone. Then enter the URL of the database remote URL on Hub. 

![](/blog/assets/uploads/clone.jpg)

![](/blog/assets/uploads/post-clone-2.jpg)



This operation could be done from *any* computer. It doesn't have to be the same installation of TerminusDB, but if you only want to install once, then this is certainly easiest.

![](/blog/assets/uploads/post-clone.jpg "Post Clone")

Now go to the manage page and click on pull. We're going to pull the `branch_office` branch to main locally. Once you've done this go to documents. You should see that the documents include `doc:mike` and `doc:jim` (as these were entered from the `branch_office` branch)

![](/blog/assets/uploads/docs-post-pull.jpg)

# Updating the Branch Office Database

Now that we have the data on our local we can go to the query screen again, and enter a new record:

```javascript
WOQL.and(
  WOQL.add_triple("doc:jill", "type", "scm:BankAccount"),
  WOQL.add_triple("doc:jill", "owner", "jill"),
  WOQL.add_triple("doc:jill", "balance", 87)
  )
```

We now have Jill in our main branch in our new database. Now it's time to push our changes to hub. Go to the "Branch Office" database, click on the Synchronize-tab and push changes to remote.

This time we want to push main to branch_office on the remote.

![](/blog/assets/uploads/post-pull-branch-office.jpg)

# Bringing the Branch Office data to Head Quarters

Once you have pushed, you should back into your old "Bank Balance Example" database. From here we can get the changes back from the branch_office changes on hub.

Now let's do another synchronize. Pull changes from hub from the `branch_office` branch to the `branch_office` branch. We now should have Jill on our local database. Let's just check quickly by looking at documents (make sure you select the correct branch in the branch selector in the top right!).

![](/blog/assets/uploads/docs-with-jill.jpg)

We still don't have the changes in main however (although we could have pulled directly) and so now we should do a merge operation again, just as we did in Part 1. We'll leave this as an exercise for the reader... good luck!