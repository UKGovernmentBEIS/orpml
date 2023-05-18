# orpml
Contains the schemas and example usage of ORPML (TO BE TRANSFERED TO DBaT WHEN READY)

These schemas describe ORPML - the markup language used by the Department for Buinsiness and Trade's [Open Regualtory Platform]().

Data passed to the ORP (for ingestion) and search results for queries on the ORP, will both use these schemas for accepting and formatting data.

The design goals for these schemas are:

1) Don't reinvent the wheel - use existing markup dialects where possible
2) Clearly demarcate metadata (at a document level) from document content
3) Allow for extention

The schema relies heavily on two existing markup languages:

1) [Dubllin Core](http://purl.org/dc/elements/1.1/)
2) [HTML](https://html.spec.whatwg.org/multipage/)

Dublin core is used as the basis for the document level metadata - ORP specific extentions are also added but in a separate namespace.
HTML is used for the markup of the document contents.

In terms of document markup, structural or ‘flow’ elements supported that are relevant to regulation material are:

`<section id=”section_1”>` - documents can be broken down into sections - sections can be nested

`<blockquote>` in combination with `<cite>` - can allow for detailed citation markup

It is envisiged that giving examples of how to create regulator items using only HTML is preferable to introducing new elements, e.g. `<sup>` - can be used to create footnote or reference indicators in the text.

Examples will be given in this repo for the above conventions.

Pending Decisions:

1) Should we extend HTML as the document content markup? Specifically, should we add `<date>, <entity>` and perhaps <subSection>` elements?
2) Should we add optional element parameters instead of introducing new elements, e.g. `<div class=”entity|date|..”>`?  
