---
description: >-
  This document is intended to make easier for operators of Aleph instances to
  follow the development and perform upgrades to their local installation.
---

# Changelog

### 3.8.9

* Move the linkages API \("god entities" / record linkage\) to use entity sets instead of its own database model.
* Remove soft-deletion for some model types \(permissions, entities, alerts, mappings\).

{% hint style="danger" %}
Aleph 3.8.9 combines all database migrations before Aleph 3.2 into a single version. If you want to upgrade from an Aleph older than 3.2, we recommend you move via 3.8.0, upgrade to that version, before migrating across this version.
{% endhint %}

### 3.8.6

* Date histogram facet and filtering tool on search results.
* Added example code for how to [add text processors](adding-text-processors.md) to Aleph.
* Re-worked collection stats caching to avoid super slow requests when no cache is present.
* Tons of bug fixes.

### 3.8.5

* Introduce EntitySets, as user-curated sets of ... entities! All diagrams are now entitysets, as will be timelines and bookmarks. 

### 3.8.3

* Refactor queue and processing code for the Aleph worker. 

### 3.8.1

* "Expand node" support in network diagrams pulls relevant connections from the backend and shows them to the user while browsing a network diagram.
* Correctly handle the use of multi-threading when using Google Cloud Storage Python client libraries.

### 3.8.0

* We've re-worked the way entities are aggregated before they are being loaded into the search index. This was required because Aleph is become more interactive and needs to handle non-bulk operations better. It also improves metadata handling, like which user uploaded a document, or when an entity was last updated. Aleph will now always keep a full record of the entities in the SQL database, whichever way they are submitted. To this end, we've migrated from `balkhash` to `followthemoney-store` \(i.e. balkhash 2.0\). This will start to apply to existing collections when they are re-ingested or re-indexed.
* Aleph has two new APIs for doing a collection `reingest` and `reindex`. The existing `process` collection API is gone. `alephclient` now supports running `reingest`, `reindex`, and `delete` on a collection.
* Operators can expedite the rollout of the new backend by running `aleph reingest-casefiles` and `aleph reindex-casefiles` to re-process all existing personal datasets.
* Numerous UI fixes make the table editor and network diagrams much more smooth.

### 3.7.2

* We've introduced a table editor in the user interface for manually editing entities in personal datasets.

### 3.7.0

* A graph expand API for entities returns all entities adjacent to an entity for network-based exploration of the data.

### 3.6.4

* **Linkages**, a new data model. A linkage is essentially an annotation on an entity saying it is the same as some other entities \(in other datasets\). This would, for example, let you group together all mentions of a politician into a single profile. Linkages are currently created via the Xref UI, which now has a ‘review mode’.
* In the future, profiles \(ie. the composite of many linkages\) will start showing up in the UI in different places, to introduce an increasingly stronger notion of data integration. Because linkages are based on a reporter’s judgement, they belong to either a\) them, or b\) a group of users — so they are always a bit contextualised, not fully public.
* Our hope is also that the data collected via linkages will provide training material for a machine learning-based approach to cross-referencing.

### 3.6.3

* Users who employ OAuth may need to change their settings to define a `ALEPH_OAUTH_HANDLER` in their `aleph.env` . By default, the following handlers are supported: `google`, `keycloak`, `azure`.

### 3.5.0

* Run VIS2 / [Network diagrams](../guide/building-out-your-investigation/network-diagrams.md) on Aleph as a testing feature.

### 3.4.9

* Two SECURITY ISSUES in the software: one that would let an attacker enumerate registered users, and the other could be exploited for XSS with a forged document. They were discovered by two friendly hackers from blbec.online who kindly reported them to us.

### 3.4.0

* The **mapping UI**. Prototyped by [@Felix Ebert](https://alephdata.slack.com/team/UE32DAC4S), [@Kirk](https://alephdata.slack.com/team/UL1AWH89X) and [@sunu](https://alephdata.slack.com/team/UE1EFLX5K) have done great work on this. The idea is that for a simple CSV file you can just upload it, and use the UI to map it into entities that you can cross-reference. It’s really simple to use and useful.
* `synonames`. This is an extension to our install of ElasticSearch that allows us to expand names into cultural transliterations. So for example doing a search for `Christoph` will now also search `Кристоф`, even though they aren’t literally the same names. This should increase recall for cross-cultural queries. The whole thing was a project from [@Aparna](https://alephdata.slack.com/team/UML9VA9K5), generating these aliases from Wikidata entries.
* These changes come alongside a lot of UI and backend polishing, so things should be much more smooth all around.

### 3.0.0

The goal of `aleph` 3.0.0 is to harmonise the handling of data inside the index. Instead of having different formats and mappings for documents, entities, table rows and document pages, there is now just one type of index object: an entity.

This means that document-based data is now completely 'translated' to the `followthemoney` ontology used by `aleph` \(meaning that in theory, each page of a document and each row of a table is now a node in the object graph of the `aleph` platform\).

#### Upgrading

In order to accomplish this, a complete re-index is required in all cases. The recommended path of migrating from a 2.x.x installation is this set of commands in an aleph container shell \(`make shell`\):

```bash
# Re-create the indexes:
aleph resetindex
# Apply a database schema change:
aleph upgrade
# Re-index collections and documents:
aleph repair --entities
```

Be advised that any data loaded via the entity mapping mechanism will need to be re-loaded after this. It is also worth noting that at OCCRP, we have now started generating mapped data via the `followthemoney` command-line tool, and are using `alephclient` to bulk-load the resulting stream of entities into the system. This has proven to be significantly quicker than the built-in mapping process.

#### Other changes

* Settings `ALEPH_REDIS_URL` and `ALEPH_REDIS_EXPIRE` are now `REDIS_URL` and

  `REDIS_EXPIRE`.

* Variable `ALEPH_OCR_VISION_API` is now `OCR_VISION_API`, it will enable use of

  the Google Vision API for optical character recognition.

* The `/api/2/collections/<id>/ingest` API now only accepts a single file, or

  no file \(which will create a folder\). The response body contains only the ID

  of the generated document. The status code on success is now 201, not 200.

