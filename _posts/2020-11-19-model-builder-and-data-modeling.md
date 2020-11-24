---
layout: post
title: Model Builder and Data Modeling
date: 2020-11-19T00:00:15.289Z
description: Let’s model your data in TerminusDB 
image: /blog/assets/images/schema_builder-01.png
author:
  - value: francesca
    label: Francesca Bitto
tags:
  - graph
  - Terminus
  - Database
  - Data
  - Development
---

In this post, I will guide you through the steps needed to design your database schema using TerminusDB 4.0 Console and its model builder.

Having your data in the right format makes it easier to analyze the data properly but you can model your data in more than one way, this is a reality for all the databases, so how to get the right decision?

The model builder is a tool that lets you visualize and edit your database schema. It is very useful for designing and understanding complex data models.
I’ll show you as things can be simple at the start, but they can become complex in no time.

if you have never used TerminusDB before, this article includes everything you need to get started with TerminusDB. [My First TerminusDB Graph Visualisation — Bike Share Data](https://terminusdb.com/blog/2020/09/01/my-first-terminusdb-3-0-graph-bike-share-data/).

------------------------------------------------------------------------------

### The Dataset

In our examples we use the collection of data about the bike journeys between stations in Washington D.C., USA. 

We have used this data in other blogs, where with woql.js, a javascript layer that help you to compose [**WOQL query** (Web Object Query Language)](https://terminusdb.com/docs/reference/server/woql/), we have created our database scheme.

![data about the bike journeys between stations in Washington D.C](/blog/assets/images/bike_table.png)

The CSV data used this tutorial is available at <https://terminusdb.com/t/data/bike_tutorial.csv>

---------------------------------------------------------

### How do I model my schema using the model builder?

I assume you already have TerminusDB and Console running in your system 
and you have created a db named **myBikes**.

![Go to model builder](/blog/assets/images/schema_builder-02.png)

Form the **myBikes** main page.

1. Select the menu **Schema->Schema Builder** to arrive at the model builder interface.
2. In the whiteboard select the node **myBikes Schema**, the list of all available node types will show up.

![See the whiteboard](/blog/assets/images/schema_builder-03.png)

Let's see the different nodes in detail.

**Object** contains a piece of the graph and can be accessed by a unique url.
An Object can have multiple properties and can be contained inside other objects. 

**Document** is a special type of Object. Documents are always top-level objects, never embedded inside other objects, using the Link Property you can link documents to each other.

**Enum** or Enumerated is a special data type that enables for a property to be a set of predefined constants. The property must be equal to one of the values that have been predefined for it. To use this set of values you'll need to create a Enum Property and link it to a Emun Element.

Let's use this elements in practice.

-----------------------------------------

### Add Nodes

For storing our data we have to create 3 different Documents  **Bicycle**, **Station** and **Journey**

![Node List](/blog/assets/images/schema_builder-04.png)

Follow the steps and start to build your schema graph.

1. Select the **myBikes Schema** node in the whiteboard and click the + icon
2. Select **Add Document** from the menu. A new node will be added under the node **Documents**
3. We start to add **Bicycle** Document. Fill the form in the right sidebar with the data for the image above. (Unique Id:Bicycle..)
4. Follow the same steps above for adding the other Documents **Station** and **Journey**
5. Click on the **Save** icon button in the tools bar to save your work.

**At this point our documents are completely unrelated.**

![Documents](/blog/assets/images/schema_builder-05.png)


-------------------------------------------------------------

### Add Properties

For descrive your Documents and for link them each others we need to add the properties.
We have different type of properties identified by datatypes

DataProperty : String/Numeric/Geo/Temporal Property refers to the format of data storage

ObjectProperty : Enum/Link Property refers at the relationship between elements

![Property list](/blog/assets/images/schema_builder-06.png)

Let's add the property **start_station**.

1. Select the node **Journey** in the whiteboard, in the right panel click on the **Properties**
2. In the properties panel, click **Add Property** in the pop up menu you can see the list of the available property types.
3. Select **Property Link** after the form shows up, fill the fields with the value reported in the table above (Unique ID/Label/Description).
4. Click on the **Link to Type** menu and Select **Bike Station** 
  *Links to Type is a list with all the node that you can linked*
5. Follow the step 1 to 4 for adding the others properties **end_station**, **journey_bicycle** 

![Property start_station](/blog/assets/images/schema_builder-07.png)

6. Click on the **Save** icon button in the tools bar to save your work.
7. Select **Relationships** in the right panel top bar to see the links.


**We have created our relationship beetween Documents.**

![Property start_station](/blog/assets/images/schema_builder-08.png)

--------------------------------------------------------------------

###  Schema Evolution and Compatibility

An important aspect of data management is schema evolution. 
My initial schema is defined, but my application now need to change it, what can I do? 

Let's see.

![Property start_station](/blog/assets/images/schema_builder-09.png)

Now we need to model the Document **Station** as Object, so we have to delete the Document **Station** and create a new node type.

The database doesn't allow you to delete a node if this is related with other node.
In details the node can not have children and it can not be a Link Property/Enum 
Property range.
Constraint is used to prevent actions that would destroy links between documents.

----------------------------------------------------------------------

### Delete a Node

Here the step for doing this.
First we have to remove all the relationship related with our node.

![Property start_station](/blog/assets/images/schema_builder-10.png)

1. Select the Document **Journey** node in the whiteboard, in the right panel Select **Properties**. the list of the properties will show up. 
2. Select **End Station** Property and delete it using the delete red icon. 
3. Follow the above steps for deleting **Start Station** Property too.
4. All the **Station** node constraints have been removed
5. Select the **Station** node in the whiteboard, in the right panel, Click the delete red icon.

Delete node is easy in this moment because we don't have data Documents inside our database.

----------------------------------------------------------------------------------

### Add an Object Type

Now we add the **Station** node as Object Type.

1. Select the **Schema myBikes**, click on the + icon, from the menu Select **Add Object** a new node will be add in the whiteboard. 
2. As we did before we have to add the Unique ID/Station and the Label/Description Bike Station in the fields in the right panel.
3. Select the Node **Journey** and add the Link Property **Start Station** **End Station** again (Follow the add property steps above).

![Property start_station](/blog/assets/images/schema_builder-10.png)

--------------------------------------------------------------------------

### Differents between Object and Document in data entry

The differents to Link a Property with a Document than a Object is in how your data will be stored.

Here an example with bike data 

If the **Station** is an Document Type you have to create a Document **Station**  
and link the id of the Document **Station** in the Document **Journey**

This means that if you remove one **Journey** Document 
it remove the link but not the data in the **Station** document related.

If the **Station** is an Object Type the data will be embedded in the Document **Journey**
So if you remove one Journey Document entry it will be remove the related **Station**
Object.

------------------------------------------------------------------------

### Add an Enum Node

We keep with our Schema evolution adding the **Type Bike** enum node

![Property start_station](/blog/assets/images/schema_builder-12.png)

1. Select the **Schema myBikes**, Select the + icon, from the menu Select **Add Enum** 
2. The Enum node will be added in the whiteboard
3. Fill the fields in the right panel (Unique Id:Bike_Type..)
4. Select values, Fill the fields and click the button **Add a value** for adding the list of possible values  

--------------------------------------------------------------------------

### Link the Enum Node with the Enum Property 

We are going to create our Enum property for the **Bicycle** Documents

1. Select the **Bicycle** node
2. In the right panel, Select the **Properties** tab 
3. Select the **Add Property** menu and Click **Enum Property**
4. Fill the Property fields in the Property panel (Unique ID:bicycle_type ....)
5. From the **Enum Type** menu Select **Bike Type**

We said that the property **Bicycle Type** in **Bicycle** Document is a string with a value chosen from a list of permitted values. 

![Property start_station](/blog/assets/images/schema_builder-13.png)

--------------------------------------------------------------------------

### Add Children

![Property start_station](/blog/assets/images/schema_builder-14.png)

We are going to add others elements to create a more complex hierarchy
 
At this point in our project we need to add information about the bike user, we suppose we can have 2 types of user that rent the bike, a registered user and a guest user.
 
First we create an abstract Document **User** for grouping the different type of user.

An Abstract Document is a completely "abstract class" that is used to group related properties. 
 
Abstract Document cannot be used to create Data Document, you cannot add data inside **User**
Document. An abstract Document can have children, all the children will inherit all parent Properties and if the children are not abstract, you can insert data inside the children. 

![Property start_station](/blog/assets/images/schema_builder-15.png)

Let's see all the steps for creating an abstract document and his children.

1. Select the **Document** node in the whiteboard, Select the + icon and from the list that Show up Select **Add Document**.
2. A new node Document will be added
3. In the right panel fill the fields for the new Document (Unique ID:User, Label:Bike User....)
4. Check the **Abstract** checkbox
5. Select the tab **Properties**, from the menu **Add Property** Click **String Property**
6. The String Property Panel will show up, fill the fields (Unique ID: email, Label:Email)
![Property start_station](/blog/assets/images/schema_builder-16.png)
7. For the **String Type** menu Select Email 
8. In the **Cardinality Min** field add 1 (this means that if you add an User you have to add one email property)

*we added the Abstract **User** Document with its properties, now let's add the children.*

1. Select the node **Bike User** in the whiteboard, Click on the + icon, from the menu Select **Add Child**
2. A new Node will be added, in the right panel fill all the fields (Unique Id:Guest ....)
3. Follow the above steps for add the **Member** Document too

We can decide to not add new properties in the Guest document so the guest user will be identified by the email, but for the **Member** Document we need to add at least the String property Password.

We have to create a Link Property **Bicycle User** in the **Bike Journey** Document for link the **User** Document with the **Bike Journey** Document.

![Property start_station](/blog/assets/images/schema_builder-17.png)

------------------------------------------------------------------------

### Add a Parent

At this point of our project we need that our nodes will be grouped again.
 
We have to create an element that can be **a Parent** for an Object node and a Document node. 
Object can be child only of Object type, Document can be child of Object Type and Document Type, so we have to create a **New Object Node**
 
We can call this node **Entity**. The **Entity** node is abstract and has a Geo Property  **Position**
 
I am sure you already know all the steps for doing this 😊.

![Add Parent](/blog/assets/images/schema_builder-18.png)

Let's group **Bike Station** and **Bicycle** under this new node.
 
1. Select the node **Bicycle** in the whiteboard, in the right panel Select **Relationships**
2. Under **Add/Remove Parents** panel from the menu Select **Object** option 
3. In **Add Object as Parent** list Select **Entity** 
4. The node **Bicycle** will be moved under the new parent **Entity**.

![Add Parent](/blog/assets/images/schema_builder-19.png)
 
Good Job, you made your first TerminusDB schema using the model builder.
Data is constantly evolving, be ready to change❗

You can clone the full schema from [TerminusHub](https://terminusdb.com/hub/)

