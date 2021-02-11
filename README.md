
## A guide for describing Legislation in schema.org 

_last updated_ : 2021-02-11
**/!\ This is work in progress**

### Welcome

This guide is intended for data publisher that wish to disseminate structured metadata about legislation on the web using [schema.org Legislation extension](http://schema.org/Legislation). This is especially targeted at stakeholders of the [European Legislation Identifier](https://eur-lex.europa.eu/eli-register/about.html) initiative, that are already engaged in the dissemination of structured metadata using the [ELI ontology](https://op.europa.eu/en/web/eu-vocabularies/model/-/resource/dataset/eli). This is also useful for:
  
  - Official Journals of non-EU member states wishing to engage into structured data dissemination
  - Local administrations creating legal act or regulations
  - EU institutions publishing regulations
  - Private legal publishers interested in making their content more visible

This guide assumes that the reader is comfortable with what schema.org is, and how to decode the [JSON-LD](https://json-ld.org/) syntax, since exemples are given in this syntax.


### Why describe Legislation using schema.org ?

_To share and link legislation data at web-scale, in a decentralized way_.

These 2 mockups show our dream in terms of search use-cases around legislation in major search engines :

  1. A rich snippet in a search result page showing a Legislation :
  



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
