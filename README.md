# ORPML

Contains the explanation, schemas and example usage of ORPML (TO BE TRANSFERED TO DBaT WHEN READY)

## What is ORPML?
ORPML stands for Open Regulation Platform Markup Language.
It describes regulatory content that is resident in the [Open Regulation Platform](https://app.dev.open-regulation.beis.gov.uk/unauthorised/ingest) (ORP).
The ORP is owned by the [Better Regulation Executive](https://www.gov.uk/government/groups/better-regulation-executive) (BRE) - a division within the [Department for Business and Trade](https://www.gov.uk/government/organisations/department-for-business-and-trade) (DBaT).


Data passed to the ORP (for ingestion) and search results for queries on the ORP, will both use these schemas for accepting and formatting data.

The design goals for these schemas are:

1) Don't reinvent the wheel - use existing markup dialects where possible
2) Clearly demarcate metadata (at a document level) from document content
3) Allow for extention

### What is it used for?
ORPML describes documents that reside within the ORP. Specifically, it describes document level metadata and also form the markup of the document content.
Regulators that contribute content to the ORP will provide their content in ORPML and send it to the ORP via an API.
Users of the ORP (RegTech companies, citizens..) can search the ORP and receive results where each returned document is formatted in ORPML.

### Where can I see more technical details?
The DBaT github [organisation](https://github.com/UKGovernmentBEIS) will contain the ORPML repo which will contain the design decisions of the standard and each release of the standard in XML schema. The repository is not currently live but it is envisaged that it will be mid 2023.

### What stage is it in?
Currently, the standard is at v0.1.
Major changes to both the nature and content of the standard should be expected. 
Initial consultation with both regulators and RegTech companies will use v0.1 as a discussion point.

## What has been its creation process to date?
Two major organisations and initiatives have been the initial starting points for consideration of ORPML:
	1. The National Archives
	2. The CDDO
	
### The National Archives
When deciding on the nature of ORPML, the first set of standards considered was [Crown Legislation Markup Language](https://www.legislation.gov.uk/schema/legislation.xsd) (CLML) from [The National Archives](https://www.nationalarchives.gov.uk).
The National Archives are the preeminent organisation in the UK when it comes to legislation and has the most experience in providing government owned data in a structured way to a variety of audiences.
CLML describes both content level metadata and the content itself so this seems like an obvious candidate for the ORP project. The initial question asked was “Should ORP just adopt CLML as its markup language?”.
Evaluation of CLML enabled a decision to not adopt CLML wholesale as the markup of the ORP, but use the majority of its component parts and ideas. Where competing standards are encountered, the deciding factor has been to adopt the standard closest to CLML as regulatory material is closest in format, intent and audience to that of legislative material.
Specifically, the following parts of the way The National Archives uses markup and publishes material, have been adopted by the ORP:

1) [DublinCore](https://www.dublincore.org) is the international standard for describing digital or physical resources. Dublin Core ‘core’ elements, of which there are 15, have been adopted into CLML to represent each digital resource. 
ORPML conforms to CLML’s usage of DublinCore in the following ways:
		1. ORPML accepts the core 15 elements and mandates their inclusion into metadata
		2. ORPML references DublicCore by schema import and uses the same ‘camelCase’ naming conventions

ORPML differs from CLML in its usage of DublinCore in 2 important ways:
		1. ORPML accepts the full range of DublinCore terms elements, not just the core elements - however only the core elements are mandatory
		2. ORPML actually only references the ‘terms’ schema (of which the core elements are now a subset)

2) [Functional Requirements for Bibliographic Records](https://cdn.nationalarchives.gov.uk/documents/cas-82049-presentation-notes.pdf) (FRBR) are the standard way to discover the location of biblegraphic content by reference to a Universal Resource Indicator (URI). Essentially, this schema tells you how to format the urls of the material you hold, so as to allow for discovery by 3rd parties. The National Archives make use of FRBR in their legislation API. It is felt that conforming to this for ORP will benefit users of both systems, in that material will be location in identical ways.

The following parts of CLML have *not* been adopted into ORPML:
	The use of CLML markup for describing the content of material. 
It was determined that CML markup was too specific to legal material (it is based on legal Markup language Akono Ntoso). The nature of regulatory material is very varied and can range from guides and practice notes that are similar in nature to simple ‘how to’ guides, to standards and rules that are highly structured in nature. CLML is focused on redaction, amendment and enforcement of material and has specific abilities to deal with these important parts of legislation. Regulatory material is not so concerned with amendments below the document level, i.e. the section, sub-section or clause level.
In ORPML’s choice of content markup, we learned from some of the issues that The National Archives have faced with Akono Ntoso. The National Archives provide all material in Akono Ntoso, but in a more widespread standard e.g. the use of a HTML5 representation of Akono Ntoso.

### The CDDO
As part of the work to create the Government Data Catalogue and Data Marketplace, the Central Digital and Data Office (CDDO) (which is part of The Cabinet Office), has created DCAT schema for data interchange within governement. It is desirable to be able to share all regulatory material as a discoverable and navigable dataset across governement.
Work has been done to compare DCAT and DublinCore (DCAT is itself based on DublinCore). Many of the mandatory fields in DCAT are direct ports from the mandatory fields in DublinCore. We evaluated the entirety of both DublinCore terms and DCAT and proposed that it is possible and desirable to support both initiatives with a common set of metadata. It is proposed that the entirity of DCAT be supported.

### Other standards considered
[Akono Ntoso](http://www.akomantoso.org) - this was rejected for the reasons stated above. In addition, the creation of Akono Ntoso is typically done using legal publishing tools and it was considered burdensome to regulators to make them use these tools for more loosely structured content.
Schema.org - ORP would have to create a schema for its material. Whilst there is a [guide](https://schema.org/Guide) already, this would not meet the needs of all regulatory material - specifically more formally structured content such as specifications and rules.

## What are the current decisions?
### Metadata
Document level metadata in ORPML is consisted of:
Mandatory elements:
	DublinCore ‘core’ elements
	DCAT mandatory elements
	ORP specific mandatory elements

This list is in preference order so if a metadata field is mandatory in DublinCode, but optional in DCAT, then the field will be mandatory in ORPML. However, if the field is unique to DCAT and mandatory, then it will be mandatory in ORPML. Where an element occurs in both DublinCore and DCAT, the the DublinCore element will be used.
DCAT element names contain punctuation and whitespace. This was considered to not be disearable since all other elements will be in camelCase. Therefore the names of all DCAT elements have been converted into camelCase, e.g. `Time Period Coverage - Start Date` is now `timePeriodCoverageStartDate`

Optional elements:
	DublinCore terms optional elements
	DCAT optional elements and DCAT ‘Mandatory if Applicable’ elements
	
#### DublinCore fields
[image:CD8B8D7D-FD26-4E58-834C-7E7324495C0E-1595-0000E0DE3A7631DA/Screenshot 2023-06-08 at 4.17.54 pm.png]
[image:C0511504-D2AF-4F95-A410-6338AE68EE18-1595-0000E10A414D63D8/Screenshot 2023-06-08 at 4.22.29 pm.png]

#### DCAT fields
[image:1E4A8278-8F6F-4279-8EA7-9E6A472A5154-1595-0000E0EEF3000DD8/Screenshot 2023-06-08 at 4.17.54 pm.png]
[image:37D1BCBB-19E7-4D37-83BE-DCA751959AD0-1595-0000E0EC98E93B42/Screenshot 2023-06-08 at 4.17.42 pm.png]
[image:93BA2F80-17A2-48EC-9992-E1318A6BA10F-1595-0000E117B741C432/Screenshot 2023-06-08 at 4.23.36 pm.png]

In deciding upon the elements from DCAT for inclusion into ORPML, the following documents were used as references:
[Record information about data sets you share with others - GOV.UK](https://www.gov.uk/guidance/record-information-about-data-sets-you-share-with-others)
The CDDO’s document “*CROSS-GOVERNMENT DATA SHARING Metadata Requirements Specification*”

Specifically, the gov.uk guide to record sharing asks you to consider the following fields:

`creator` - present in DublinCore core so used in ORPML

`dateCreated` -  present in DublinC0re as `created` so used in ORPML

`name` - not used in ORPML as `title` is used instead

`description` - present in DublinCore core so used in ORPML

`identifier` - present in DublinCore core so used in ORPML

`encoding format` - present in DublinCore core as `format`  so used in ORPML

`superceededby` - not used as DublinCore has `isReplacedBy`

`superceeds` - not used as DublinCore has `replaces`

`expires` - present in DCAT as `timePeriodCoverage-EndDate` so used in ORPML

`temporal coverage` - not used as this is ‘data collection date’ rather than enforcement date

`conforms to` - not used but would be hardcoded to `ORPML`

`license` - present in DublinCore so used in ORPML

`hasDigitalDocumentPermission` - covered by a combination of `dc:license` and `dcat:securityClassification`

#### Custom fields
[image:A649B530-933B-47F6-AC76-50CDE7AC25B9-1595-0000E0EFA2BD2836/Screenshot 2023-06-08 at 4.17.54 pm.png]
[image:E809E305-8604-4A1D-B428-382E809C4831-1595-0000E0ED571968BC/Screenshot 2023-06-08 at 4.17.42 pm.png]
[image:2C97F04E-6BF3-4951-9A1C-4A077F55003D-1595-0000E0DE3B653EAD/Screenshot 2023-06-08 at 4.18.58 pm.png]
These elements are specific to ORPML content and not part of external schemas.
We have tried to minimise the number of these elements as much as possible.	



### Content [NOT COMPLETED YET]
It is proposed that all content provided in ORPML be HTML.
The benefits of using HTML as the content markup are severalfold:
	* Editorial tools can be used to convert from existing publishing formats such as .pdf or docx into HTML.
	* There is an established industry that Regulators could use to convert from their existing publish formats into HTML
	* ORP can validate content to make sure it is valid HTML as HTML validators are readily available in Open Source Software
	* Rendering HTML in the ORP portal or by RegTech companies, is simple and strightforward

One decision point that is still to be made is “Should we create a superset of HTML with Regulatory specific elements or use element attributes to describe regulatory concepts?”
Our working assumption is that we will only accept standard HTML and that regulatory concepts will be described by element attributes.
Here are some examples to descibe how this could look in ORPML:

Sections
```
<article aria-label="my article name">
	<a href="this/is/the/link/to/the/article" aria-describedby="article link information">
	  <h1>Article name</h1>
  </a>
	<section aria-label="important section">
		<a href="this/is/the/link/to/the/section"/>
This text is really important and it deserves to be in its own section.</br><em>All</em> standard HTML markup will be supported
  </section>
</article>
```

Sub Sections
```
<section data-orp-type="subSection" aria-label="sub section name">
	<a href="this/is/the/link/to/the/subSection" aria-describedby="article link information"/>
This text is not as important so it gets to be in a sub section.</br><em>All</em> standard HTML markup will be supported
</section>
```

Clause
```
<section data-orp-type="clause" aria-label="clause name">
  <a href="this/is/the/link/to/the/clause" aria-describedby="clause link information"/>
This text is really important and it deserves to be in its own section.</br><em>All</em> standard HTML markup will be supported
</section>
```

Entities
```
<section data-orp-type="clause" aria-label="section name">
  <a href="this/is/the/link/to/the/clause" aria-describedby="section link information">
We will be talking about new regulations from <div data-orp-entity="The Bank of England" aria-label="entity">The Bank of England</div>.
</section>
```

Citations
```
<section data-orp-type="clause" aria-label="section name">
  <a href="this/is/the/link/to/the/clause" aria-describedby="section link information"/>
This regulation derives from the <cite><a href=-"https://www.legislation.gov.uk/ukpga/2018/12/contents">Data Protection Act 2018</a></cite>
</section>
```

