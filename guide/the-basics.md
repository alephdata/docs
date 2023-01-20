---
description: >-
  This page defines some useful terms for understanding the basics of Aleph and
  how the Aleph ecosystem is structured to support your investigations.
---

# The building blocks of Aleph

Aleph is more than just a place to search and explore **source documents**. It also provides **structured entities** to model the primary units of an investigation, **datasets** to house both source material and investigation files, and **groups** to help you share and manage access to knowledge as your investigation grows.

## Documents

With Aleph, you can [**search**](search/), [**upload**](building-out-your-investigation/uploading-documents.md)**, organize**, and **share** many different types of documents that would otherwise be difficult to explore. Aleph provides support for extracting meaningful information from PDFs, Excel files, CSVs, email archives, web sites, and other document formats to make source material easier to browse and analyze no matter in which form it comes.

While documents often provide the evidentiary backbone of an investigation, Aleph is built to explore complicated relationships that are often difficult to glean from documents alone - this is where structured entities come in.

## Entities

Aleph provides an ever-growing [**vocabulary**](../developers/followthemoney/) for describing and modeling the **people**, **companies**, **assets**, and **relationships** that are often at the center of your investigations.

![](<../.gitbook/assets/Screen Shot 2020-07-30 at 12.30.31.png>)

Each entity type contains a fixed set of possible properties to describe relevant details of that entity, and can be linked to other entities via relationships.

![](<../.gitbook/assets/Screen Shot 2020-07-30 at 12.39.08.png>)

This structured vocabulary allows entities to be more easily searched, filtered, and cross-referenced with other data sources to find relevant co-occurrences and further enrich your investigation.

## Mentions

When you upload unstructured documents (for example, PDF documents) to Aleph, Aleph tries to extract names, locations, IBAN account numbers, and more from the document contents. Mentions are different from entities in that they're stored as text and do not contain structured data. You can search Aleph for other datasets and documents with matching mentions. For example, if an uploaded document mentions an IBAN, Aleph allows you to search for other datasets and documents that mention the same IBAN.

## Datasets & Investigations

In Aleph, **datasets** and **investigations** serve as the primary containers for organizing and managing collections of documents and entities. Any document or entity in Aleph must be a part of either a dataset or an investigation.

### Source datasets

**Source datasets** reflect the accumulated contents of a single source (i.e. a company registry, an email leak, a court archive). They are managed by the data administrators of Aleph and cannot be changed or edited by any other users of the platform. Learn about how to find a specific dataset or datasets pertaining to a specific country \*\*\*\*[**here**](search/searching-for-a-dataset.md).

![](<../.gitbook/assets/Screen Shot 2020-07-30 at 13.02.55.png>)

### Investigations

**Investigations** are contained workspaces within Aleph where you can upload, edit, and organize \*\*\*\*data related to an investigation or topic of interest - and they can be shared with any other user or access group within Aleph.

While source datasets cannot be modified, **investigations** allow you to edit their contents by [**uploading documents**](building-out-your-investigation/uploading-documents.md), [**mapping data into structured entities**](building-out-your-investigation/generating-multiple-entities-from-a-list.md), and [**creating your own network diagrams**](building-out-your-investigation/network-diagrams.md).

Read more about how to get started creating and managing an investigation [**here**](building-out-your-investigation/creating-an-investigation.md).

## Groups

While browsing Aleph, you are only able to view or edit data according to the respective access permissions you have been granted.

Access in Aleph is managed through **Groups**. Once granted permission, members of a group have access to all of the **source** **datasets** and **investigations** that have been shared with the group.

Groups are managed by site administrators, and cannot, therefore, be created or deleted through the Aleph user interface.

Read more about managing access [**here**](building-out-your-investigation/creating-an-investigation.md#managing-access-to-your-personal-dataset).

**Now that you've gotten acquainted with some of the core concepts of Aleph, it's time to dive in. Read on to learn more about** [**searching in Aleph**](search/) **and** [**building out your own investigation**](building-out-your-investigation/)**.**
