---
mimo_pageDescription: This article describes a method for creating card-style responsive tables using only CSS.
mimo_pageTitle: Card-Style CSS-Only Responsive Tables
mimo_pageID: card-style-css-only-responsive-tables
mimo_date: Apr 27, 2018
mimo_shareOnFacebook: true
mimo_shareOnTwitter:
    hashtags: css, responsivetables
    via: JeringTech
---

<!-- changes
    - gif showing how table changes
    - don't use stupid cats as examples so we can remove redundant references thing
-->

This article describes a CSS-only method for creating card-style responsive tables. Such tables
display each row as a card when space is limited. This article 
also discusses alternative methods, some of
which are useful in certain situations.

## The Method

### Result
+{
    "type": "markdown",
    "sourceUri": "../contentResources/card-style-css-only-responsive-tables/exampleTableWithWrappers.html",
    "clippings": [{
        "beforeContent": "<div id=\"method-example\">\n",
        "afterContent": "</div>\n"
    }]
}

! Adjust your window's width to test the table's responsiveness.

### Code
@{
    "lineNumberLineRanges":[{}],
    "language": "html",
    "title": "HTML"
}
+{ 
    "sourceUri": "../contentResources/card-style-css-only-responsive-tables/exampleTableWithWrappers.html",
    "clippings": [{ "collapseRatio": 0.5 }]
}

@{
    "lineNumberLineRanges":[{}],
    "language": "css",
    "title": "CSS"
}
+{ 
    "sourceUri": "../contentResources/card-style-css-only-responsive-tables/methodExample.css",
    "clippings": [{ "collapseRatio": 0.5 }]
}

### HTML Breakdown

#### Description
The only difference between the HTML for this method and the HTML for a typical table is that contents of `<td>` elements 
must be wrapped. For example, in the HTML for this method, the contents of `<td>` elements are wrapped in `<span>` elements:

@{
    "lineNumberLineRanges":[{"firstNumber": 12}],
    "language": "html",
    "title": "HTML"
}
+{ 
    "sourceUri": "../contentResources/card-style-css-only-responsive-tables/exampleTableWithWrappers.html",
    "clippings": [{ "startLineNumber": 12, "endLineNumber": 15, "collapseRatio": 0.5 }]
}

! [\<td\> elements](https://html.spec.whatwg.org/multipage/tables.html#the-td-element) have the 
! [flow content](https://html.spec.whatwg.org/multipage/dom.html#flow-content) content model,
! so it is fine for them to have child `<span>` elements.

#### Rationale
Wrapping allows the content of a `<td>` element to be laid out in its own [box](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model) 
when the table is in card mode. For example, consider a `<td>` element in the DOM tree when the 
table is in card mode:

@{"language": "html"}
```
<td data-label="Breed">
  ::before
  <span>Persian Cat</span>
</td>
```

This structure allows `Persian Cat` to be laid out in a box adjacent to the `::before` pseudo element's box. This can be done using styles like
`td:before, td > span { display: inline-block; }` or `td:before, td > span { display: table-cell; }`. If there is no wrapper element, the
same `<td>` element would have the following structure in the DOM tree:

@{"language": "html"}
```
<td data-label="Breed">
  ::before
  Persian Cat
</td>
```

`Persian Cat` would be a text node in the same box that contains the `::before` element. Hacky solutions would be required to lay `Persian Cat` out adjacent to 
the `::before` element. Some such solutions are discussed in the [alternative methods](http://localhost:8080/articles/css-only-responsive-tables#without-wrappers-in-td-elements) 
section.

### CSS Breakdown

#### Description
If the non-optional (non-presentational) styles for this method were inlined, the `<table>` element would look like this in the DOM tree:

@{"language": "html"}
```
<table style="display: block">
  <thead style="display: none">...</thead>
  <tbody style="display: table">
    <tr style="display: table-row-group">
      <td data-label="label" style="display: table-row">
        ::before <!-- content: attr(data-label); display: table-cell; -->
        <span style="display: table-cell">value</span>
      </td>
      <!-- More <td> elements ommitted for brevity -->
    </tr>
    <!-- More <tr> elements ommitted for brevity -->
  </tbody>
</table>
```

This is equivalent to a HTML table with the following structure:

@{"language": "html"}
```
<table> <!-- <tbody> element with display: table -->
  <tbody> <!-- <tr> element with display: table-row-group -->
    <tr> <!-- <td> element with display: table-row -->
      <td> <!-- ::before pseudo element with display: table-cell -->
        label
      </td> 
      <td> <!-- <span> element with display: table-cell -->
        value
      </td>
    </tr>
    <!-- More <tr> elements ommitted for brevity -->
  </tbody>
  <!-- More <tbody> elements ommitted for brevity -->
</table>
```

#### Rationale
The above-mentioned table structure is a simple and reliable structure for two-column cards. The following are some reasons why:
- No need to set column widths or row heights, since browsers handle table [column widths](https://www.w3.org/TR/CSS22/tables.html#auto-table-layout) and 
[row heights](https://www.w3.org/TR/CSS22/tables.html#height-layout).
- `<tbody>` elements demarcate cards, allowing for per-card styles, such as alternating background colors.
- The structure can be made accessible to screen readers through the use of [ARIA roles](https://www.w3.org/TR/2017/NOTE-wai-aria-practices-1.1-20171214/examples/table/table.html).
This is elaborated on in the next section.

### Notes
#### Accessibility
Accessibility is a problem for responsive tables. The root issue is that table elements (`<table>`, `<tr>`, `<td>`, etc) lose their semantic meanings 
when their `display` properties are changed. For example,
consider the accessibility tree (viewable in Chrome's [accessibility pane](https://developers.google.com/web/updates/2018/01/devtools#a11y-pane)) of
a row in the example table when in normal mode (`display` properties unchanged):

```
> table
  > row
    > gridcell "Persian Cat"
    > gridcell "12-17 years"
    > gridcell "Iran"
    > gridcell "The Persian Cat is a..."
```

The browser correctly interprets semantic meanings, identifying `<tr>` elements as rows, `<td>` elements as gridcells and so on. 
On the other hand, when the table is in card mode (`display` properties changed), the accessibility tree of the table is just a bunch 
of `GenericContainers`.

Unfortunately, all CSS-only, card-style, responsive table methods change `display` properties. That said, when using this method, JS can be used to improve accessibility by applying
[ARIA roles](https://www.w3.org/TR/2017/NOTE-wai-aria-practices-1.1-20171214/examples/table/table.html). For example, consider the card mode DOM tree with ARIA attributes applied:

@{"language": "html"}
```
<tbody role="table">
  <tr role="rowgroup">
    <td data-label="Breed" role="row">
      ::before
      <span role="cell">Persian Cat</span>
    </td>
    <!-- More <td> elements ommitted for brevity -->
  </tr>
  <!-- More <tr> elements ommitted for brevity -->
</tbody>
```

The accessibility tree of a row in the table would be as follows:

```
> table
  > row
    > gridcell "Breed"
    > gridcell "Persian Cat""
```

! Chrome correctly infers the semantic meaning of the `::before` element.

#### Use of Standard Table Elements
For this method, `<div>` elements coupled with `display` properties and ARIA attributes can be used in place of table elements. 
That said, table elements offer some conveniences:
- No need to specify CSS `display` properties when in normal mode.
- No need to specify ARIA roles when in normal mode.

#### Further Enhancements
- This method does not consider table elements like `<caption>`, `<col>` or `<tfoot>`, nor does it consider table element attributes like `span`. If this method is used on tables with such elements and
attributes, additional styles are required.
- If a page relocates elements for responsiveness, [EQCSS](https://elementqueries.com/) or [ResizeObserver](https://developers.google.com/web/updates/2016/10/resizeobserver)
could improve the user experience. For example, this page relocates its side menus to the top of the page when window width decreases past specific breakpoints.
Just after a side menu has been relocated to the top of the page,
tables are wider than they were immediately before; because of this, having tables to go from normal to card mode 
when a certain table width is met (using EQCSS or ResizeObserver) instead of when a certain window width is met, could offer a better user experience.
- The example code for this method uses plain CSS for simplicity's sake. Implementing this method in [Sass](https://sass-lang.com/) or [Less](http://lesscss.org/) could provide greater flexibility. 

## Alternatives

### Without Wrappers in \<td\> Elements 
There are situations where wrapping the contents of `<td>` elements isn't possible. Methods for card-style responsive tables in such situations
typically involve
taking `td:before` out of [normal flow](https://developer.mozilla.org/en-US/docs/Web/CSS/Visual_formatting_model#Normal_flow). The following method
uses `position: absolute` to do just that:

#### Result
+{
    "type": "markdown",
    "sourceUri": "../contentResources/card-style-css-only-responsive-tables/exampleTableNoWrappers.html",
    "clippings": [{
        "beforeContent": "<div id=\"alternative-example\">\n",
        "afterContent": "</div>\n"
    }]
}

! The table's responsiveness can be tested by adjusting your window's width.

#### Code
@{
    "lineNumberLineRanges":[{}],
    "language": "html",
    "title": "HTML"
}
+{ 
    "sourceUri": "../contentResources/card-style-css-only-responsive-tables/exampleTableNoWrappers.html",
    "clippings": [{ "collapseRatio": 0.5 }]
}

@{
    "lineNumberLineRanges":[{}],
    "language": "css",
    "title": "CSS"
}
+{ 
    "sourceUri": "../contentResources/card-style-css-only-responsive-tables/alternativeExample.css",
    "clippings": [{ "collapseRatio": 0.5 }]
}

#### Notes
Arbitrary dimensions are often used for such methods. This causes
brittleness. For example,
in the method above, `td:before` has `width: 50%` so that labels do not wrap and overflow the `<td>` element. 
Even then, if a label is longer than `50%` of the table's width, wrapping will still occur.  

This brittleness makes it difficult to apply such methods across an entire site without making tweaks for each table.
For example, like the method above, [this method](https://codepen.io/AllThingsSmitty/pen/MyqmdM) takes `td:before` out of normal flow, using `float: left` - 
as is, it cannot be applied to the example table content used in this article.
Nonetheless, such methods are useful when it isn't possible to wrap the contents of `<td>` elements or when table content is always similar.

### With Wrappers in \<td\> Elements 
There alternatives methods to create card-style responsive tables when the contents of `<td>` elements are wrapped:
- `display: inline-block` and `display: flex` can be used to lay out card rows. The downside to these layout methods is that column widths will not be
handled by browsers.
- CSS grid could be used in place of CSS tables after the
[subgrids](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout#Subgrid) feature is released.

## Conclusion
This article describes a useful, CSS-Only, general solution for card style, responsive tables. While useful in many situations, the method is not a silver bullet - circumstances should be taken into account. 
Thanks for reading, I hope this article has been useful, feel free to share tips or to point out mistakes in the comments below!

## Additional Reading
- [HTML spec for tables](https://html.spec.whatwg.org/multipage/tables.html)
- [CSS spec for tables](https://www.w3.org/TR/CSS22/tables.html)

## Citations
- Persian cat. (n.d.). In Wikipedia. Retrieved April 27, 2018, from https://en.wikipedia.org/wiki/Persian_cat
- Maine Coon. (n.d.). In Wikipedia. Retrieved April 27, 2018, from https://en.wikipedia.org/wiki/Maine_Coon
- British Shorthair. (n.d.). In Wikipedia. Retrieved April 27, 2018, from https://en.wikipedia.org/wiki/British_Shorthair