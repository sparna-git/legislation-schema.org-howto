
## A guide for describing Legislation in schema.org 

  - _last updated_ : 2021-02-11
  - _author_ : Thomas Francart (thomas [dot] francart [at] sparna [dot] fr) 
  - status : **/!\ This is work in progress**

### Welcome

This guide is intended for data publisher that wish to disseminate structured metadata about legislation on the web using [schema.org Legislation extension](http://schema.org/Legislation). This is especially targeted at stakeholders of the [European Legislation Identifier](https://eur-lex.europa.eu/eli-register/about.html) initiative, that are already engaged in the dissemination of structured metadata using the [ELI ontology](https://op.europa.eu/en/web/eu-vocabularies/model/-/resource/dataset/eli). This is also useful for:
  
  - Official Journals of non-EU member states wishing to engage into structured data dissemination
  - Local administrations creating legal act or regulations
  - EU institutions publishing regulations
  - Private legal publishers interested in making their content more visible

This guide assumes that the reader is comfortable with what schema.org is, and how to decode the [JSON-LD](https://json-ld.org/) syntax, since exemples are given in this syntax.


### Why describe Legislation using schema.org ?

_To share and link legislation data at web-scale, in a decentralized way_.

These 2 mockups show the "dream" in terms of search use-cases around legislation in major search engines :

  - A rich snippet in a search result page showing a Legislation
  
  ![Legislation rich snippet](/images/legislation-rich-snippet.png)


  - A legislation shown as a knowledge graph entity on the side of a search results page :
  
  ![Legislation in knowledge graph](/images/legislation-knowledge-graph.png)

These mockups display specific structured metadata about the act:
  - Title of the act
  - Summary
  - Whether the act is still in force or not
  - Keywords
  - Links to latest or previous version of the legal text, with the date at which that version was published
  - Number, or reference
  - Jurisdiction (geographical area on which the act applies)
  - Type of act
  - Links to other texts (here, an abrogation link)
  
These are only mockups, of course, and they do not represent a commitment of any search engine to implement this as this is depicted.

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
  
### Overview of the schema.org metadata available to describe Legislation (and why this is insufficient)

The necessary attributes to describe legislation in schema.org are summarized in this diagram, showing how the description of legislation is mixing generic attributes available on the class [CreativeWork](http://schema.org/CreativeWork), and specific attributes proposed for the specific description of Legislation :

![Legislation schema.org diagram](/images/legislation-schemaorg-diagram.png)

This diagram gives a good overview of which properties are interesting/necessary for the description of legal acts, however it is not sufficient :
  - It does not indicate that some properties carry different semantics depending on where they are expressed.
  - It does not reflect the good practices in terms of legal act description, as seen in different EU Member States within the ELI Taskforce.
  - It does not express the difference between what is a "base act" published in an Official Journal, what is an amending act, what is a consolidated version, etc.

The rest of this guide captures the good practices in terms of data modeling for the description of legal acts and how to apply them in the context of the schema.org Legislation extension.

In a sense it gives an _application profile_ of schema.org for the description of legislation.


## Good practices for the description of Legislation in schema.org

### What does the Legislation type means ?

The `Legislation` type in schema.org may be used to describe different things:

  1. A **base act** published in an official journal. This is the "birth" of a legal act.
  2. An **amending act**, also published in an official journal, amending the base act. This is expressed like a "diff" applied on the base act ("In the base act, point 2 of article 3 is replaced by the following : ....")
  3. A **consolidated version** of the base act, as amended by one or more amending act. This is the base act, with all its "diffs" applied on it.
  4. The **"abstract" act**, that is the act as an intellectual work, independantly of one of its (consolidated) version. This is intuitively what we are using when writing a legal reference without specifying a version : _"Council Directive (EC) 93/104 concerning certain aspects of the organisation of working time [1993] OJ   L307/18 (Working  Time  Directive)"_.
  5. An **article or another subdivision** of the base act, or one of its subsequent consolidated version.
  
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

- Even though the textual content of the Consolidated version V0 is identical to the base act, it is really considered as another document, hence another entity. It is usually not published by the same system, does not have the same legal value as the base act, is not under the same responsibilities, may differ in its cover page or number, etc. For all these reasons, it is something different.
  - The abstract act encompasses the successive consolidated versions of the act, but not the base act itself. The base act "gives birth to" the consolidated versions, but is not considered a version in itself; instead the consolidated version v0 represents the first version of the act.
  - As they are not versionned strictly speaking, the base act and the amending act do not have their "abstract" level, like the consolidated versions have.

