---
description: >-
  Aleph connects to OpenRefine, a free data processing tool, in order to perform
  batch lookups for a list of entities (e.g. people and companies).
---

# OpenRefine Reconciliation

**Entity reconciliation** can be used to check if any entry in a column of data mentions persons or companies of interest.

More generally, the function enables another application - typically [OpenRefine](http://openrefine.org) - to link up (_reconcile_) values in it's own data with the entities stored in Aleph.

To use the reconciliation function, first install the free Open Refine tool on your computer and create a new project from a spreadsheet with at least one column that contains the names of people or companies.

In the header of that column, select the drop down menu, _Reconcile_, then _Start reconciling..._.

![](../.gitbook/assets/screenshot-2019-08-29-at-17.56.35.png)

The following window allows you to add a new standard reconciliation service. Paste the URL for Aleph and click _Add Service_.

{% hint style="info" %}
It's recommended to run the reconciliation service against a specific dataset on Aleph, rather than a full site. The reconciliation URL for each dataset is shown on the dataset's overview page.
{% endhint %}

![The reconciliation service URL for each dataset can be found in the details section of the dataset home page.](../.gitbook/assets/screenshot-2019-08-29-at-17.59.27.png)

Then choose _Start reconciling_ to begin the reconciliation process.

![In this reconciliation result, the last row shows a match. Hovering over the match will show its profile page on Aleph.](../.gitbook/assets/screenshot-2019-08-29-at-18.01.06.png)

For further information, consult the following documents:

* [Data Augmentation in Refine](https://www.youtube.com/watch?v=5tsyz3ibYzk#t=2m42) (YouTube video).
* [Reconciliation](https://github.com/OpenRefine/OpenRefine/wiki/Reconciliation) in the Open Refine wiki.
