---
description: >-
  This page defines some useful terms for understanding the basics of Aleph and
  how the Aleph ecosystem is structured to support your investigations.
---

# The building blocks of Aleph

Aleph is more than just a place to search and explore **source documents**. It also provides **structured entities** to model the primary units of an investigation, **datasets** to house both source material and investigation files, and **groups** to help you share and manage access to knowledge as your investigation grows.

## Documents

With Aleph, you can [**search**](search.md), [**upload**](building-out-your-investigation/uploading-documents.md)**, organize**, and **share** many different types of documents that would otherwise be difficult to explore. Aleph provides support for extracting meaningful information from PDFs, Excel files, CSVs, email archives, web sites, and other document formats to make source material easier to browse and analyze no matter in which form it comes.

While documents often provide the evidentiary backbone of an investigation, Aleph is built to explore complicated relationships that are often difficult to glean from documents alone - this is where structured entities come in.

## Entities

Aleph provides an ever-growing [vocabulary](../developers/followthemoney.md) for describing and modeling the people, companies, assets, and relationships that are often at the center of your investigations.

![](../.gitbook/assets/screen-shot-2020-07-30-at-12.30.31.png)

Each entity type contains a fixed set of possible properties to describe relevant details of that entity, and can be linked to other entities via relationships.

![](../.gitbook/assets/screen-shot-2020-07-30-at-12.39.08.png)

This structured vocabulary allows entities to be more easily searched, filtered, and cross-referenced with other data sources to find relevant co-occurrences and further enrich your investigation.

## Datasets

In Aleph, **datasets** serve as the primary tool for organizing and managing collections of documents and entities. Any document or entity in Aleph must be a part of a dataset. Datasets come in two forms: 

### Source datasets

**Source datasets** reflect the accumulated contents of a single source \(i.e. a company registry, an email leak, a court archive\). They are managed by the data administrators of Aleph and cannot be changed or edited by any other users of the platform.

![](../.gitbook/assets/screen-shot-2020-07-30-at-13.02.55.png)

### Personal datasets

**Personal datasets** are contained workspaces within Alph where you can **upload**, **edit**, and **organize data** related to an investigation or topic of interest - and they can be shared with any other user or access group within Aleph.

While source datasets cannot be modified, **personal datasets** allow you to edit their contents by [uploading documents](building-out-your-investigation/uploading-documents.md), [mapping data into structured entities](building-out-your-investigation/generating-multiple-entities-from-a-list.md), and [creating your own network diagrams](building-out-your-investigation/network-diagrams.md). 

Read more about how to get started creating and managing a personal dataset [here](building-out-your-investigation/creating-a-personal-dataset.md).

## Groups

While browsing Aleph, you are only able to view or edit data in Aleph according to the respective access permissions you have been granted.

Access in Aleph is managed through **Groups**. Once granted permission, members of a group have access to all of the datasets that have been shared with the group. 

Groups are managed by site administrators, and cannot, therefore, be created or deleted through the Aleph user interface.

Read more about managing access [here](building-out-your-investigation/creating-a-personal-dataset.md#managing-access-to-your-personal-dataset).



**Now that you've gotten acquainted with some of the core concepts of Aleph, it's time to dive in. Read on to learn more about** [**searching in Aleph**](search.md) **and** [**building out your investigation**](building-out-your-investigation/)**.**

