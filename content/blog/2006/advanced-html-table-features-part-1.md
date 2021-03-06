---
title: Advanced HTML table features, Part 1
date: "2006-05-06"
url: /blog/2006/05/06/advanced-html-table-features-part-1/
categories:
  - Web
---
After a long break for a bunch of SQL posts, I'm continuing to explain how to push HTML tables to their limits. In this series I discuss the seldom-used features many people don't know exist in HTML tables, such as axes and the `headers` attribute. These are the tools you need to really use tables to their full power, instead of shoving some square-peg data into a round-hole table. Granted, many times you don't need these features, but when you do, they're like the "ludicrous speed" lever on your spaceship: nothing else will do. Hang on to your hat! Tabular data is where HTML gets really interesting.

For the authoritative details, refer to the [Tables in HTML documents](http://www.w3.org/TR/html4/struct/tables.html) section of the HTML spec (and the DTD, if you really want the final answer). Here's what I'll cover: Part 1 (this article) will discuss `caption`, `summary`, `abbr`, `scope`, the `headers` attribute, and `th`. Part 2 will cover `axis`, hierarchical vs. relational data, and the old multiple-hierarchy problem.

### `caption`, `summary`, and `abbr`

The `caption` is a label for the table. It's semantically associated with the table because it is contained inside it, as opposed to some text that is "near" the table (a common, but meaningless, method of labelling tables and images). Most browsers display it above the table, centered by default. The HTML spec defines some methods of positioning the caption, but they're not fully supported in all browsers. Here's an example:

```sql
<table class="borders collapsed">
<caption>apples and oranges</caption>
<thead><tr><th>Type</th><th>Variety</th><th>Price</th></tr></thead>
<tbody>
<tr><td>Apple</td><td>Fuji</td><td>5.00</td></tr>
<tr><td>Apple</td><td>Gala</td><td>6.00</td></tr>
<tr><td>Orange</td><td>Valencia</td><td>4.00</td></tr>
<tr><td>Orange</td><td>Navel</td><td>5.00</td></tr>
</tbody>
</table>
```

And here is the result:

<table class="borders collapsed">
  <caption>apples and oranges</caption> <tr>
    <th>
      Type
    </th>

    <th>
      Variety
    </th>

    <th>
      Price
    </th>
  </tr>

  <tr>
    <td>
      Apple
    </td>

    <td>
      Fuji
    </td>

    <td>
      5.00
    </td>
  </tr>

  <tr>
    <td>
      Apple
    </td>

    <td>
      Gala
    </td>

    <td>
      6.00
    </td>
  </tr>

  <tr>
    <td>
      Orange
    </td>

    <td>
      Valencia
    </td>

    <td>
      4.00
    </td>
  </tr>

  <tr>
    <td>
      Orange
    </td>

    <td>
      Navel
    </td>

    <td>
      5.00
    </td>
  </tr>
</table>

`summary` is an attribute of the `table` element itself. It is not displayed visually by any browser of which I'm aware, but it may be read by screen readers. Here's how to use it:

```sql
<table summary="Prices of apples and oranges"...>
```

`abbr` is not unique to tables. It is both a [phrase element](http://www.w3.org/TR/html4/struct/text.html#edef-ABBR) and an [attribute for table cells](http://www.w3.org/TR/html4/struct/tables.html#adef-abbr). The first use is familiar to more people than the second, in my experience. In the second usage, it is <q cite="http://www.w3.org/TR/html4/struct/tables.html#adef-abbr">used to provide an abbreviated form of the cell's content.</q> I use it in my article on [tables and data](/blog/2006/01/02/tables-and-data-part-1/).

Note that there is both an `abbr` **attribute** and an **element**. They're different. The attribute is for table cells, and is used for **providing** an abbreviated form of content. The element is for **defining** abbreviations by providing a non-abbreviated form of content.

### `scope` and `headers`

The `scope` attribute is technically defined for both `th` and `td` (because they're defined identically in the DTD), but it's really only meant for `th` elements. It indicates what the cell is a header for---a row, a column, a column group, or a row group. For many common cases, the first row in a table contains headers for the rest of the table. In this scenario, each header's scope is a column---those cells appearing directly below it. Another common way to use headers is as the first cell in each row; in this case the cell's scope is a row (all the cells to the right of it). The exact legal values are defined in the DTD as `(row|col|rowgroup|colgroup)`.

The `headers` attribute is the inverse of the `scope` attribute on header cells. Like `scope`, it's defined for both `th` and `td`, but only has meaning for `td`. Whereas `scope` indicates which data cells "belong to" the header cell, `headers` indicates which header cells contain header information for the data cell.

The HTML authors probably included this attribute because tabular data can be very complex (I haven't asked them, so I'm not sure about their reasons). The attribute holds a space-separated list of the `ID`s for the applicable header cells. It's a reverse pointer from the cell to its headers.

### To be continued

Next time I'll talk about adding meta-data to tables, multi-dimensional tables, and related concepts.
