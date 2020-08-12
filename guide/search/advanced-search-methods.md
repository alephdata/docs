# Advanced Search Methods

You can use a variety of advanced search methods to either expand your search or constrain it, narrowing the number of results Aleph returns. 

By default, Aleph tries to find matches based off your keywords pretty broadly, returning matches that include all your keywords at the top, but Aleph doesn't exclude matches that include only one of your keywords. It just pushes down in the order of search results. 

For example, if you type the keywords:

```text
Ilham Aliyev
```

Aleph will return all matches that have the workd Ilham and Aliyev, and then matches that have either Ilham or Aliyev in them. This might be too broad for your needs.

However, you can explicitly tell Aleph to be more exact in how it determines matches. 

### Finding an exact phrase or name

If you want Aleph to only return matches that have exactly "Ilham Aliyev", then you should put quotations around those two keywords.

```text
"Ilham Aliyev"
```

![](../../.gitbook/assets/ilham_aliyev_exact.png)

### Allow for Variations in Spelling

Sometimes a name can be spelled many different ways or even mispelled many different ways. One way to solve this problem is to simply type each variation in the search form:

```text
Aliyev Əliyev Aliyeva Əliyeva
```

You might capture all the variations you want, but you also might miss some by accident. Another way to tell Aleph to look for variants of a name is to use the ~ operator:

```text
Aliyev~2
```

What this translates to is: Give me matches that include the keyword Aliyev, but also me matches that include up to any 2 letter variations from the keyword Aliyev. These variations include adding, removing, and changing a letter. This includes Aliyev, of course, but also includes Əliyev, which is just one letter variation different, and Əliyeva, which is two letter variations different from Aliyev. 

![](../../.gitbook/assets/variation_search.png)

{% hint style="info" %}
Using this operator with too high a number \(greater than 3, for example\) will cause slow searches and may return too many false results.
{% endhint %}

### Search for words the should be in proximity to each other

If you do not want to find a precise keyword, but merely specify that two words are supposed to appear close to each other, you might want to use a **proximity search,** which also uses the ~ operator. This will try to find all the requested search keywords within a given distance from each other. For example, to find matches where the keywords Trump and Aliyev are ten or fewer words apart from each other, you can formulate the search as:

```text
"Trump Aliyev"~10
```

![](../../.gitbook/assets/proximity_search.png)



