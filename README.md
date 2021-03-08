  - _last updated_ : 2021-02-11
  - _author_ : Thomas Francart (thomas [dot] francart [at] sparna [dot] fr) 
  - _status_ : **/!\ This is work in progress**
  - _feedback_ : please send your feedback on this document in the Github repository [{{ site.github.repository_name }}]({{ site.github.repository_url }})

---

* TOC
{:toc}

---

## Welcome

This guide is intended for data publishers that wish to disseminate structured metadata about legislation on the web using **[schema.org Legislation extension](http://schema.org/Legislation)**. It is especially targeted at stakeholders of the [European Legislation Identifier (ELI)](https://eur-lex.europa.eu/eli-register/about.html) initiative, that are already engaged in the dissemination of structured data using the [ELI ontology](https://op.europa.eu/en/web/eu-vocabularies/model/-/resource/dataset/eli). This guide should also be useful for:
  
  - Official Journals of non-EU member states wishing to engage into structured data dissemination
  - Local administrations creating legal act or regulations
  - EU or internation institutions publishing regulations
  - Private legal publishers interested in making their content more visible on the web

This guide assumes that the reader is comfortable with schema.org, with the [JSON-LD](https://json-ld.org/) syntax, and with legislation publishing and consolidations.

## Legislation on the web, schema.org, and ELI

### Why describe Legislation using schema.org ?

_To share and link legislation data at web-scale, in a decentralized way_.

These 2 mockups show the "dream" in terms of search use-cases around legislation in major search engines :

  - A rich snippet in a search result page showing a Legislation
  
  ![Legislation rich snippet](/images/legislation-rich-snippet.png)


  - A legislation shown as a knowledge graph entity on the side of a search results page :
  
  ![Legislation in knowledge graph](/images/legislation-knowledge-graph.png)

These mockups display specific structured data about the act:
  - Title of the act
  - Summary
  - Whether the act is still in force or not
  - Keywords
  - Links to latest or previous version of the legal text, with the date at which that version was published
  - Number, or reference
  - Jurisdiction (geographical area on which the act applies)
  - Type of act
  - Links to other texts (here, an abrogation link)
  
These are only mockups, of course, and they do not represent a commitment of any search engine to implement this as it is depicted.

### Resources

Schema.org Legislation extension is at [**http://schema.org/Legislation**](http://schema.org/Legislation). It was derived from the [ELI ontology](https://op.europa.eu/en/web/eu-vocabularies/model/-/resource/dataset/eli), which itself is the result of discussions amongst 14+ Official Journals of EU Member States about how to best represent and describe legislation metadata.

#### Community discussions

Interested readers can find more information on the community discussions that happened when the extension proposal was submitted :
  - Initial proposal in [schema.org Github issue #1156](https://github.com/schemaorg/schemaorg/issues/1156)
  - Second [issue #1743](https://github.com/schemaorg/schemaorg/issues/1743)
  - And the third one to align it to ELI 1.3, [issue #2698](https://github.com/schemaorg/schemaorg/issues/2698)
  
#### ELI ontology guides

The ELI ontology guides for publishing legislation metadata using ELI may also provide useful background information, although not targeted at schema.org :
  - [ELI methodological guide](https://op.europa.eu/en/publication-detail/-/publication/514875b4-5efd-11e8-ab9c-01aa75ed71a1)
  - [ELI technological guide](https://op.europa.eu/en/publication-detail/-/publication/8159b75d-5efc-11e8-ab9c-01aa75ed71a1)

#### ELI to schema.org converter

The Office of Publications of the EU has provided a [conversion tool to generate schema.org markup from ELI metadata](https://webgate.ec.europa.eu/eli-validator/eli2sdo).


### Overview of the schema.org metadata available to describe Legislation (and why this is insufficient)

The necessary attributes to describe legislation in schema.org are summarized in this diagram, showing how the description of legislation is mixing generic attributes available on the class [CreativeWork](http://schema.org/CreativeWork), and specific attributes proposed for the specific description of Legislation :

![Legislation schema.org diagram](/images/legislation-schemaorg-diagram.png)

This diagram gives a good overview of which properties are interesting/necessary for the description of legal acts, however it is not sufficient :
  - It does not capture the difference between what is a "base act" published in an Official Journal, what is an amending act, what is a consolidated version, etc.
  - It does not reflect the good practices in terms of legal act description, as seen in different EU Member States within the ELI Taskforce.
  - It does not indicate that some properties have slightly different meaning depending on where they are expressed.

The rest of this guide captures the good practices in terms of data modeling for the description of legal acts and how to apply them in the context of the schema.org Legislation extension. It renders these good practices as an _application profile_ of schema.org for the description of legislation. It is certainly not the only and definitive solution for the description of legislation, alternative solutions are of course possible.


## Good practices for the description of Legislation in schema.org

### What does schema:Legislation means ?

The [`schema:Legislation`](http://schema.org/Legislation) type in schema.org may be used to describe different things:

  1. A **base act** published in an official journal. This is the "birth" of a legal act.
  2. An **amending act**, also published in an official journal, amending the base act. This is expressed like a "diff" applied on the base act (_"In the base act, point 2 of article 3 is replaced by the following : ...."_)
  3. A **consolidated version** of the base act, as amended by one or more amending act. This is the base act, with all its "diffs" applied on it.
  4. The **"abstract" act**, that is the act as an intellectual work, independantly of one of its (consolidated) version. This is intuitively what we are using when writing a legal reference without specifying a version : _"Council Directive (EC) 93/104 concerning certain aspects of the organisation of working time [1993] OJ   L307/18 (Working  Time  Directive)"_.
  5. An **article or another subdivision** inside the base act, or one of its version.
  
### Base act, amending acts, consolidated versions, abstract act

The relation between the base act, its amending act, the resulting consolidated versions, and the abstract act is shown in the diagram below:

![Relation between base act, amending act, consolidated versions and abstract act](/images/structure-conso-abstract.png)

This is the sequence of events that this diagram depicts :

1. A base act is published in an Official Journal.
2. Immediately, a first consolidated version of the act is produced (Consolidated act V0).
3. A first amending act is published in the OJ.
4. The base act and the first amending act are consolidated in Consolidated act V1
5. A second amending act is published in the OJ.
6. The base act, the first and second amending acts are consolidated in Consolidated act V2.


There are a few important things to note:

- Even though the textual content of the Consolidated version V0 is identical to the base act, it is really considered as another document, hence another entity. It is usually not published by the same system, does not have the same legal value as the base act, is not under the same responsibilities, may differ in its cover page or number, is not presented in the same web page, etc. For all these reasons, it is something different - but related.
- The abstract act encompasses the successive consolidated versions of the act, but not the base act itself. The base act "gives birth to" the consolidated versions, but is not considered a version in itself; instead the consolidated version v0 represents the first version of the act.
- As they are not versionned strictly speaking, the base act and the amending acts do not have their "abstract" level, like the consolidated versions have.

The relationships between these entities is as follow:

1. The abstract act refers to its successive versions using the [`schema:workExample`](http://schema.org/workExample) property. (in the web pages of each consolidated version, only one of these links will be present, not all).
2. Each consolidated version of the act points to the base act and all the amending acts being consolidated in this version using the [`schema:legislationConsolidates`](http://schema.org/legislationConsolidates) property.
3. The abstract act points to the base act that it derives from using the [`schema:isBasedOn`](http://schema.org/isBasedOn) property.

### Legal analysis links

[`schema:Legislation`](http://schema.org/Legislation) specifies a certain number of links between Legislation entities that pertain to _legal analysis_, that is to the analysis of the actual content of the legal act (vs. identification properties) and how it relates to other acts. These links are (click to get the definition on schema.org website):

- [`schema:legislationChanges`](http://schema.org/legislationChanges) and its subproperties
  - [`schema:legislationAmends`](http://schema.org/legislationAmends)
  - [`schema:legislationRepeals`](http://schema.org/legislationRepeals)
  - [`schema:legislationCommences`](http://schema.org/legislationCommences)
- [`schema:legislationCorrects`](http://schema.org/legislationCorrects)
- [`schema:citation`](http://schema.org/citation)
- [`schema:isBasedOn`](http://schema.org/isBasedOn)

These links:
- are stated **on the acts published in the Official Journal** (base act or modifying act), because it is these acts that have legal value (usually).
- point to a specific consolidated version of the act being changed/corrects/cited/taken as basis, so that we know which precise version of the act the links points to.
- can be expressed as inverse links (JSON-LD `@reverse`) on the consolidated versions so that the data is available when browsing that version of the act.
- can point to specific article or subdivision of a consolidated version.

### Articles and Subdivisions

Articles and other subdivisions are also typed as [`schema:Legislation`](http://schema.org/Legislation). The whole act refers to its subdivisions using [`schema:hasPart`](http://schema.org/hasPart), and subdivisions can contain other subdivisions. The usage of subdivision is not mandatory, and the data can stop at describing the whole act only.

The combination of _legal analysis_ links and subdivisions is depicted in the below diagram:


![Legal Analysis links and subdivisions](/images/structure-legal-analysis.png)

This is the content depicted in this diagram:
1. The initial base act has 2 articles, article 1 and article 2. Hence the Consolidated Version V0 has the same structure.
2. Modifying act 1, through its article 1, amends the article 1 of the initial act, hence the `legislationAmends` link points to the article 1 of the Consolidated Version V0, because it is the actual article being amended.
3. Modifying act 1 is consolidated by the Consolidated Act V1 (link not shown for readability).
4. Modifying act 2, through its article 1, repeals (cancels) the article 2 of the initial act, hence the `legislationRepeals` link points to the article 1 of the Consolidated Version V1.
5. Modifying act 1 and 2 are consolidated by Consolidated Act V2, that contains only 1 article (because article 2 was repealed).


### Special relations to EU directives or regulations (or other higher-level legal corpus)

In addition to the legal analysis relationships described above that refer to acts in the same legal corpus, other links are provided to refer to acts of another legal corpus, typically to refer to EU directives or regulations that are transposed or implemented by EU Member States. This also covers Non-EU cases of local regulation applying a national legislation. The relations are:

- [`schema:legislationApplies`](http://schema.org/legislationApplies) is a generic link to state that an act somehow transfers another act into another legislative context; this link has subproperties:
  - [`schema:legislationTransposes`](http://schema.org/legislationTransposes) : this link is highly specific to EU directive transposition, and has a precise, legally-binding, meaning;
  - [`schema:legislationEnsuresImplementationOf`](http://schema.org/legislationEnsuresImplementationOf) : to be used for EU regulations that are not transposed, or to state that an act makes sure there is no conflict for another act to apply;
- [`schema:sameAs`](http://schema.org/sameAs) : in the specific case where a legal act published in a different legal corpus is getting republished in this corpus, for example EU directives republished in national Official Journals, or national acts republished in local journals;


## Legislation in schema.org Application Profile

### Act (Base act or Modifying act)

The description of a Base act or modifying act has the same structure.

#### Mandatory properties for Act (Base act or Modifying act)

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| `@id` | URI | 1..1 | URI identifier for the act  |
| `@type` | fixed value `Legislation` | 1..1 | Always set the type to `Legislation`  |
| [`name`](http://schema.org/name) | rdf:langLiteral | 1..n | An act may have more than one title, in case it is multilingual.  |
| [`encoding`](http://schema.org/encoding) | [Legislation file](#legislation-file) | 1..n | Refers to file expressing the act, in PDF, HTML, etc. and in a given language |

#### Recommended properties for Act (Base act or Modifying act)

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`inLanguage`](http://schema.org/inLanguage) | xsd:string | 0..n | Language of the act. Use 2-letters language codes. Repeat if act is multilingual |
| [`legislationIdentifier`](http://schema.org/legislationIdentifier) | xsd:string | 0..1 | Number for the act |
| [`legislationDate`](http://schema.org/legislationDate) | xsd:date | 0..1 | Date at which the text became an act |
| [`legislationDateVersion`](http://schema.org/legislationDateVersion) | xsd:date | 0..1 | |
| [`legislationLegalForce`](http://schema.org/legislationLegalForce) | [`LegalForceStatus`](http://schema.org/LegalForceStatus) | 0..1 | Can be [`InForce`](http://schema.org/InForce), [`NotInForce`](http://schema.org/NotInForce), [`PartiallyInForce`](http://schema.org/PartiallyInForce) |
| [`legislationType`](http://schema.org/legislationType) | xsd:string | 0..1 | Type of the text, as string (law, decree, etc.) |

#### Optional properties for Act (Base act or Modifying act)

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`about`](http://schema.org/about) | xsd:string | 0..n | Keywords on the act, as string |
| [`alternateName`](http://schema.org/alternateName) | xsd:string | 0..n | alternative or short title |
| [`datePublished`](http://schema.org/datePublished) | xsd:date |  0..1 | Date of publication in the Official Journal |
| [`description`](http://schema.org/description) | xsd:string | 0..n ||
| [`isPartOf`](http://schema.org/isPartOf) | [`PublicationIssue`](http://schema.org/PublicationIssue)  |  0..1 | The OJ issue in which the act was published |
| [`legislationCountersignedBy`](http://schema.org/legislationCountersignedBy) | xsd:string |  0..n ||
| [`legislationDateOfApplicability`](http://schema.org/legislationDateOfApplicability) | xsd:date |  0..1 ||
| [`legislationPassedBy`](http://schema.org/legislationPassedBy) | xsd:string |  0..n ||
| [`legislationResponsible`](http://schema.org/legislationResponsible) | xsd:string |  0..n ||
| [`publisher`](http://schema.org/publisher) | [`Organization`](http://schema.org/Organization) |  0..n | Usually refers to the service/administration in charge of publishing legislation in an Official Journal |
| [`spatialCoverage`](http://schema.org/spatialCoverage) | [`Place`](http://schema.org/Place) |  0..n | Jurisdiction, applicability area or sovereignity |
| [`temporalCoverage`](http://schema.org/temporalCoverage) | xsd:string | 0..1 | In force range of the act, from the date it was set in force to the date it was repealed. Use ISO 8601 time interval format, and use `xxxx-xx-xx/..` to represent an open-ended interval for acts that are still in force. |
| all [legal analysis properties](#legal-analysis-properties). |  | 0..n ||
| all [transposition and implementation properties](#transposition-and-implementation-properties). |  | 0..n ||


#### Example

```
{
	"@id" : "http://country.xyz/eli/decree/1234-56/oj",
	"@type" : "Legislation",
	"name" : [
		{ "@value" : "Decree of the XXXX-XX-XX regarding...", "@language" : "en" }
		{ "@value" : "Décret du ... portant sur ...", "@language" : "fr" }
	],
	"legislationIdentifier" : "1234-56",
	"legislationType" : "Decree",
	"inLanguage" : ["fr", "en"],
	"legislationDate" : "2019-09-22",
	"datePublished" : "2019-09-23",
	"isPartOf" : {
		"@id": "http://country.xyz/official-journal/20190923",
		"@type" : "PublicationIssue",
		"name" : "Official Journal from 2019-09-23"
	},

	"encoding" : {
		...
	}
}
```


### Act consolidated version

An Act consolidated version will never be described on its own, but it will always be included in the Abstract act description inside a `workExample` property (see the section on [Abstract act]((#abstract-act)).

#### Mandatory properties for Act consolidated version

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| `@id` | URI | 1..1 | URI identifier for this consolidated version, typically including a date to identify the version  |
| `@type` | fixed value `Legislation` | 1..1 | Always set the type to `Legislation`  |
| [`encoding`](http://schema.org/encoding) | [Legislation file](#legislation-file) | 1..n | Refers to file expressing this act consolidated version, in PDF, HTML, etc. and in a given language |
| `@reverse` [`workExample`](http://schema.org/workExample) | [Legislation (Abstract Act)](#abstract-act) | 0..1 | An Act version must be described within the context of an Abstract Act (the `@reverse` notation indicate we are expecting this entity to be the _value_ / _object_ of the property, and not its subject.) |

#### Recommended properties for Act consolidated version

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`datePublished`](http://schema.org/datePublished) | xsd:date | 0..n | The date at which this version was published (not to be confused with the date at which the original base act was published in the OJ) |
| [`legislationConsolidates`](http://schema.org/legislationConsolidates) | [Legislation (Base act)](#act-base-act-or-modifying-act) | 0..n | Points to the base act and all modifying act taken into account in this version |
| [`legislationLegalForce`](http://schema.org/legislationLegalForce) | [`LegalForceStatus`](http://schema.org/LegalForceStatus) | 0..1 | Indicate the validity of that specific version of the act, as opposed the in-force status of the whole act itself. Can be [`InForce`](http://schema.org/InForce) or [`NotInForce`](http://schema.org/NotInForce) |
| [`temporalCoverage`](http://schema.org/temporalCoverage) | [`Place`](http://schema.org/Place) | 0..n | The validity range of this version of the act, from the date it is consolidated to the date it is replaced by a new version. Not to be confused with the in force time span of the act itself. Use ISO 8601 time interval format, and use `xxxx-xx-xx/..` to represent an open-ended interval. |
| `@reverse` all [legal analysis properties](#legal-analysis-properties) | - | 0..n | Backward links from all modifying acts that refer to this version (the `@reverse` notation indicate we are expecting this entity to be the _value_ / _object_ of the property, and not its subject.) |

#### Optional properties for Act consolidated version

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`hasPart`](http://schema.org/hasPart) | [Legislation (Article or other subdivision)](#article-or-other-subdivision) | 0..n | Points to the articles or other subdivisions in this act version |
| [`publisher`](http://schema.org/publisher) | [`Organization`](http://schema.org/Organization) | 0..n | Usually refers to the service/administration in charge of publishing consolidated texts (could be different than the organization publishing the OJ) |
| [`text`](http://schema.org/text) | xsd:string | 0..1 | May contain the textual content of the act version, as a plain string |
| [`version`](http://schema.org/version) | xsd:string | 0..1 ||


#### Example

```
{
	"@id" : "http://country.xyz/eli/decree/1234-56", 
	"@type" : "Legislation",
	"name" : [
		{ "@value" : "Decree of the XXXX-XX-XX regarding...", "@language" : "en" }
		{ "@value" : "Décret du ... portant sur ...", "@language" : "fr" }
	],
	"legislationIdentifier" : "1234-56",
	"legislationType" : "Decree",
	"inLanguage" : ["fr", "en"],
	"legislationDate" : "2019-09-22",
	"legislationLegalForce" : "InForce",
	"temporalCoverage" : "2019-09-22/..",
	"legislationDateVersion" : "2021-01-28",
	"isBasedOn" : "http://country.xyz/eli/decree/1234-56/oj",
	"workExample" : {
		"@id" : "http://country.xyz/eli/decree/1234-56/20200301",
		"@type" : "Legislation",
		"datePublished" : "2020-03-03",
		"temporalCoverage" : "2020-03-01/..",
		"legislationLegalForce" : "NotInForce",
		"legislationConsolidates" : [
			"http://country.xyz/eli/decree/1234-56/oj",
			"http://country.xyz/eli/modifyingAct/ABC",
			"http://country.xyz/eli/modifyingAct/DEF",
		],
		"text" : "...",
		"@reverse" : {
			"legislationAmends" : [
				"http://country.xyz/eli/modifyingAct/GHI",
				"http://country.xyz/eli/modifyingAct/JKL"
			]
		}
		
		"encoding" : {
			...
		}
	}
}
```




### Abstract act

The abstract act is the one that we intuitively refer to when talking about the legislation without specifying which version we are referring to. Its metadata should contain sufficient information to be able to resolve references to this act; which precise information depends on the legal corpus.

The Abstract act will usually be used within the markup for a specific version of the act, and include one `workExample` to point to the version visible in the current page.

#### Mandatory properties for Abstract act

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| `@id` | URI | 1..1 | URI identifier for the abstract act. This identifier must be the same for each version of the same act. |
| `@type` | fixed value `Legislation` | 1..1 | Always set the type to `Legislation`  |
| [`name`](http://schema.org/name) | rdf:langLiteral | 1..n | Title of the act. An act may have more than one title, in case it is multilingual.  |


#### Recommended properties for Abstract act

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`inLanguage`](http://schema.org/inLanguage) | xsd:string | 0..n | Use 2-letters language codes. Repeat if act is multilingual |
| [`isBasedOn`](http://schema.org/isBasedOn) | [Legislation (Base Act)](#act-base-act-or-modifying-act) | 0..n | Refers to the URI of the Base Act. Although not strictly mandatory if you describe only the abstract act, and not the base act, this is highly recommended  |
| [`legislationIdentifier`](http://schema.org/legislationIdentifier) | xsd:string | 0..1 | Number for the act |
| [`legislationDate`](http://schema.org/legislationDate) | xsd:date | 0..1 | Date at which the text became an act |
| [`legislationDateVersion`](http://schema.org/legislationDateVersion) | xsd:date | 0..1 | |
| [`legislationLegalForce`](http://schema.org/legislationLegalForce) | [`LegalForceStatus`](http://schema.org/LegalForceStatus) | 0..1 | Can be [`InForce`](http://schema.org/InForce), [`NotInForce`](http://schema.org/NotInForce), [`PartiallyInForce`](http://schema.org/PartiallyInForce) |
| [`legislationType`](http://schema.org/legislationType) | xsd:string | 0..1 | Type of the text, as string (law, decree, etc.) |
| [`workExample`](http://schema.org/workExample) | [Legislation (Act version)](#act-consolidated-version) | 0..n | Refers to specific versions of this legislation |


#### Optional properties for Abstract act

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`about`](http://schema.org/about) | xsd:string | 0..n | Keywords on the act, as string |
| [`alternateName`](http://schema.org/alternateName) | xsd:string | 0..n | alternative or short title |
| [`datePublished`](http://schema.org/datePublished) | xsd:date |  0..1 | Date of publication in the Official Journal |
| [`description`](http://schema.org/description) | xsd:string | 0..n ||
| [`isPartOf`](http://schema.org/isPartOf) | [`PublicationIssue`](http://schema.org/PublicationIssue) |  0..1 | The OJ issue in which the act was published |
| [`legislationCountersignedBy`](http://schema.org/legislationCountersignedBy) | xsd:string |  0..n ||
| [`legislationDateOfApplicability`](http://schema.org/legislationDateOfApplicability) | xsd:date |  0..1 ||
| [`legislationPassedBy`](http://schema.org/legislationPassedBy) | xsd:string |  0..n ||
| [`legislationResponsible`](http://schema.org/legislationResponsible) | xsd:string |  0..n ||
| [`publisher`](http://schema.org/publisher) | [`Organization`](http://schema.org/Organization) |  0..n | Usually refers to the service/administration in charge of publishing legislation in an Official Journal |
| [`spatialCoverage`](http://schema.org/spatialCoverage) | [`Place`](http://schema.org/Place) |  0..n | Jurisdiction, applicability area or sovereignity |
| [`temporalCoverage`](http://schema.org/temporalCoverage) | xsd:string | 0..1 | In force range of the act, from the date it was set in force to the date it was repealed. Use ISO 8601 time interval format, and use `xxxx-xx-xx/..` to represent an open-ended interval for acts that are still in force. |

#### Example

```
{
	"@id" : "http://country.xyz/eli/decree/1234-56", 
	"@type" : "Legislation",
	"name" : [
		{ "@value" : "Decree of the XXXX-XX-XX regarding...", "@language" : "en" }
		{ "@value" : "Décret du ... portant sur ...", "@language" : "fr" }
	],
	"legislationIdentifier" : "1234-56",
	"legislationType" : "Decree",
	"inLanguage" : ["fr", "en"],
	"legislationDate" : "2019-09-22",
	"legislationLegalForce" : "InForce",
	"temporalCoverage" : "2019-09-22/..",
	"legislationDateVersion" : "2021-01-28",
	"isBasedOn" : "http://country.xyz/eli/decree/1234-56/oj"
}
```


### Legislation file

A Legislation file, or `LegislationObject` in schema.org naming, is the encoding of a an act, or act version, in a given language and in a given file format. An act or act version will typically be associated to different files of the same text, each in a different format and possibly a different language when the legislation is multilingual.

#### Mandatory properties for Legislation file

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| `@id` | URI | 1..1 | URI identifier for the legislation object  |
| `@type` | fixed value `LegislationObject` | 1..1 | Always set the type to `LegislationObject`  |
| [`contentUrl`](http://schema.org/contentUrl) | URL | 1..1 | URL of the actual file |
| [`encodingFormat`](http://schema.org/encodingFormat) | xsd:string | 1..1 | Media type as a MIME format e.g. application/pdf, text/html, application/xml |
| [`inLanguage`](http://schema.org/inLanguage) | xsd:string | 1..n | Language of the document. Use 2-letters language codes. In the rare case that the _same document_ contains the act text in multiple languages, that property can be repeated at this level. |
| `@reverse` [`encoding`](http://schema.org/encoding) | [Legislation (Base Act)](#act-base-act-or-modifying-act) or [Legislation (Act Version)](#act-consolidated-version) | 1..1 | Backward link from the base act or the act version encoded by this legislation file. (`@reverse` notation indicate we are expecting this entity to be the _value_ / _object_ of the property, and not its subject.) |

#### Recommended properties for Legislation file

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`legislationLegalValue`](http://schema.org/legislationLegalValue) | [`LegalValueLevel`](http://schema.org/LegalValueLevel) | 0..1 | The legal value of this file. Different files may have different legal values. Typically a digitally signed PDF have a "stronger" legal value than the HTML file of the same act. Values from stronger to weaker are [`DefinitiveLegalValue`](http://schema.org/DefinitiveLegalValue), [`AuthoritativeLegalValue`](http://schema.org/AuthoritativeLegalValue), [`OfficialLegalValue`](http://schema.org/OfficialLegalValue), [`UnofficialLegalValue`](http://schema.org/UnofficialLegalValue) |

#### Optional properties for Legislation file

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`copyrightHolder`](http://schema.org/copyrightHolder) | [`Organization`](http://schema.org/Organization) | 0..1 ||
| [`license`](http://schema.org/license) | URL | 0..1 | License URL of this file |
| [`publisher`](http://schema.org/publisher) | [`Organization`](http://schema.org/Organization) | 0..1 | Publisher of the file |

#### Example

```
{
	"@id" : "http://country.xyz/eli/decree/1234-56", 
	"@type" : "Legislation",
	"workExample" : {
		"@id" : "http://country.xyz/eli/decree/1234-56/20200301",
		"@type" : "Legislation",
		"encoding" : {
			"@id" : "http://country.xyz/eli/decree/1234-56/20200301/fr/pdf",
			"@type" : "LegislationObject",
			"inLanguage" : "fr",
			"encodingFormat" : "application/pdf",
			"contentUrl" : "http://country.xyz/wp-content/1234-56-20200301-fr.pdf",
			"legislationLegalValue" : "OfficialLegalValue",
			"license" : "...",
			"copyrightHolder" : "...",
			"publisher" : "..."
		},
		"encoding" : {
			"@id" : "http://country.xyz/eli/decree/1234-56/20200301/en/pdf",
			"@type" : "LegislationObject",
			"inLanguage" : "en",
			"encodingFormat" : "application/pdf",
			"contentUrl" : "http://country.xyz/wp-content/1234-56-20200301-en.pdf",
			"legislationLegalValue" : "OfficialLegalValue"
		},
		"encoding" : {
			"@id" : "http://country.xyz/eli/decree/1234-56/20200301/fr/html",
			"@type" : "LegislationObject",
			"inLanguage" : "fr",
			"encodingFormat" : "text/html",
			"contentUrl" : "http://country.xyz/wp-content/1234-56-20200301-fr.html",
			"legislationLegalValue" : "OfficialLegalValue"
		},
		"encoding" : {
			"@id" : "http://country.xyz/eli/decree/1234-56/20200301/en/html",
			"@type" : "LegislationObject",
			"inLanguage" : "en",
			"encodingFormat" : "text/html",
			"contentUrl" : "http://country.xyz/wp-content/1234-56-20200301-en.html",
			"legislationLegalValue" : "OfficialLegalValue"
		}
	}
}
```

### Article or other subdivision


#### Mandatory properties for Article or other subdivision

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`name`](http://schema.org/name) | xsd:string | 1..1 | Display title of the article, typically "Article 2" |
| [`legislationIdentifier`](http://schema.org/legislationIdentifier) | xsd:string | 1..1 | Number of the subdivision, typically article number |

#### Recommended properties for Article or other subdivision

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| `@id` | URI | 0..1 | Although not strictly mandatory, it is a good practice to assign URI identifiers to subdivisions, too  |
| [`hasPart`](http://schema.org/hasPart) | [Legislation (Article or other subdivision)](#article-or-other-subdivision) | 0..n | A subdivision may contain other subdivisions |
| [`legislationLegalForce`](http://schema.org/legislationLegalForce) | [`LegalForceStatus`](http://schema.org/LegalForceStatus) | 0..1 | The legal force of the article. Articles within the same act can have a different legal force. Can be [`InForce`](http://schema.org/InForce), [`NotInForce`](http://schema.org/NotInForce), [`PartiallyInForce`](http://schema.org/PartiallyInForce) |
| [`text`](http://schema.org/text) |  xsd:string | 0..1 | May contain the textual content of the article, as a plain string |

#### Optional properties for Article or other subdivision

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`spatialCoverage`](http://schema.org/spatialCoverage) | [`Place`](http://schema.org/Place) |  0..n | Articles within the same act can have different applicability areas |
| [`temporalCoverage`](http://schema.org/temporalCoverage) | xsd:string | 0..1 | In force range of this article, from the date it was set in force to the date it was repealed. Use ISO 8601 time interval format, and use `xxxx-xx-xx/..` to represent an open-ended interval |
| all [Legal analysis properties](#legal-analysis-properties) | Legislation ([Act version](#act-consolidated-version) or [Article thereof](#article-or-other-subdivision)) | 0..n | When the article is an article of a base act or a modifying act, it may have legal analysis links to act versions, or articles thereof |
| `@reverse` all [Legal analysis properties](#legal-analysis-properties) | Legislation ([Modifying Act](#act-base-act-or-modifying-act) or [Article thereof](#article-or-other-subdivision)) | 0..n | When the article is an article of a an act version, it may have backward legal analysis links from modifying acts ot article thereof. (the `@reverse` notation indicate we are expecting this entity to be the _value_ / _object_ of the property, and not its subject.) |

#### Example

```
{
	"@id" : "http://country.xyz/eli/decree/1234-56", 
	"@type" : "Legislation",
	"workExample" : {
		"@id" : "http://country.xyz/eli/decree/1234-56/20200301",
		"@type" : "Legislation",		
		"hasPart" : [
		{
			"@id" : "http://country.xyz/eli/decree/1234-56/20200301/art_1", 
			"@type" : "Legislation",
			"name" : "Article 1",
			"legislationIdentifier" : "1",
			"text" : "No person shall drive a vehicle on a section of road which is subject to a variable speed limit at a speed exceeding that indicated by a speed limit sign."
		},
		{
			"@id" : "http://country.xyz/eli/decree/1234-56/20200301/art_2", 
			"@type" : "Legislation",
			"name" : "Article 2",
			"legislationIdentifier" : "2",
			"text" : "...",
			"hasPart" : [
				...
			]
		}
	}
}
```

### Legal analysis properties

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`citation`](http://schema.org/citation) | [`CreativeWork`](http://schema.org/CreativeWork) | 0..n | Legal citation in the text |
| [`isBasedOn`](http://schema.org/isBasedOn) | [`Legislation`](http://schema.org/Legislation) | 0..n | Indicates that an act relies on another, typically secondary legislation is based on primary legislation, itself based on constitution. Not to be confused with an abstract act that is based on the base act. |
| [`legislationChanges`](http://schema.org/legislationChanges) | Legislation (Act version or subdivision thereof) | 0..n | Indicates a modification in the legal content of the act |
| [`legislationAmends`](http://schema.org/legislationAmends) | Legislation ([Act version](#act-consolidated-version) or [subdivision thereof](#article-or-other-subdivision)) | 0..n | A modification in the content |
| [`legislationRepeals`](http://schema.org/legislationRepeals) | Legislation ([Act version](#act-consolidated-version) or [subdivision thereof](#article-or-other-subdivision)) | 0..n | Cancels a legislation or an article |
| [`legislationCommences`](http://schema.org/legislationCommences) | Legislation ([Act version](#act-consolidated-version) or [subdivision thereof](#article-or-other-subdivision)) | 0..n | Sets another legislation or article into force |
| [`legislationCorrects`](http://schema.org/legislationCorrects) | Legislation ([Act version](#act-consolidated-version) or [subdivision thereof](#article-or-other-subdivision)) | 0..n | Indicates a correction that does not affect the legal content of the act |

### Transposition and implementation properties

| Property | Range | Card. | Usage Note |
| ---------| ----- | ----- | ---------- |
| [`legislationApplies`](http://schema.org/legislationApplies) | [`Legislation`](http://schema.org/Legislation) | 0..n | Generic link to state that an act somehow transfers another act into another legislative context |
| [`legislationTransposes`](http://schema.org/legislationTransposes) | [`Legislation`](http://schema.org/Legislation) | 0..n | Specific to EU directives transposition, with a precise, legally-binding, meaning |
| [`legislationEnsuresImplementationOf`](http://schema.org/legislationEnsuresImplementationOf) | [`Legislation`](http://schema.org/Legislation) | 0..n | To be used for EU regulations that are not transposed, or to state that an act makes sure there is no conflict for another act to apply |
| [`sameAs`](http://schema.org/sameAs) | Legislation ([Base act](#act-base-act-or-modifying-act)) | 0..n | In the specific case where a legal act published in a different legal corpus is getting republished in this corpus, for example EU directives republished in national Official Journals, or national acts republished in local journals |


### Semantic Pitfalls

Some properties are used in different places with a slightly different meaning :

- [`isBasedOn`](http://schema.org/isBasedOn) is used on Abstract Act, to indicate the Abstract Act is based on a Base Act, and in Legal analysis properties, to indicate that a secondary legislation is based on primary legislation;
- [`legislationLegalForce`](http://schema.org/legislationLegalForce)  is used on Base Act or Abstract Act to indicate the in-force status of the act, and on an Act Version to indicate the validity status of this specific version;
- [`temporalCoverage`](http://schema.org/temporalCoverage) is used on Base Act or Abstract Act to indicate the in-force date range of the act, and on an Act Version to indicate the validity range of this specific version;
- [`inLanguage`](http://schema.org/inLanguage) is used on Base Act or Abstract Act to indicate the language(s) of the act, and on a Legislation file to indicate the language of the file
- [`datePublished`](http://schema.org/datePublished) is used on Base Act or Abstract Act to indicate the date of publication of the act in the OJ, and on an Act Version to indicate the date of publication of that version

