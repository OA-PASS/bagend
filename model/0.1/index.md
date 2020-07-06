---
navigation_weight: 20
---
# Resource Model Overview

## Submission and Article
The BAGEND Resource Model is used to describe the payload of a BAGEND bag.  Each bag contains a single [`Submission`][4], which aggregates the remaining resources.  A [`Submission`][4] must have exactly one [`Article`][5]; therefore a single bag may only contain resources that describe a single publication.  If multiple articles are to be submitted for deposit, then a bag must be created and submitted for each article.

## People
The model goes to some effort to distinguish between the individual persons involved with a submission.  These individuals include the submitter, custodial contact, and infrastructure contact.  The submitter is typically the person who has a legal or contractual obligation to fulfil various policies (vis-&#xe0;-vis their institution or funders), i.e. a Principal or Co-Investigator.  The custodial contact supports a proxy submission use case, whereby the intellectual content of the submission was prepared by someone other than the submitter (e.g. a graduate student or administrative assistant).  Finally, the infrastructure contact is the individual or agent who oversees the technical aspects of creating and transferring a bag.

## Agreements and Contracts
In our personal experience we are familiar with generic workflows which require agreement to terms of use, or grant of a non-exclusive license for re-use of content.  [`Agreement`][6] and [`Contract`][7] are two resources that assert an individual has acknowledged, accepted, or otherwise agreed to the terms and conditions presented by a "contract".  These resources are meant to allow for machine attestation of these kinds of "click-through" agreements.  Recording these attestations in a bag provides assurance on the part of the receiver that proper protocol is being followed with respect to the transfer of content.

Upon receipt of a bag, a workflow may be initiated on behalf of the submitter.  For example, deposit of an article to the NIH initiates a workflow in NIHMS on behalf of the submitter.  If the agreements and attestations are properly represented in the bag, these workflows would not have to present clickthrough agreements that have already been signed by the submitter.

## Relationship to BagIt
The BAGEND specification builds upon the [BagIt standard][1] by:
1. Requiring that valid and complete bags be used as defined in [RFC 8493][1], 
2. Requiring that bags conform to the BAGEND [BagIt profile][2], and 
3. Requiring the definition and use of two novel BagIt metadata tags: `BagEnd-Submission-File` and `BagEnd-Submission-Resource`.

Requiring complete and valid bags ensures that any files referenced by BAGEND BagIt tags and BAGEND model resources will be present when the bag is processed.  

The BAGEND metadata tags `BagEnd-Submission-File` and `BagEnd-Submission-Resource` are used by bag processors to locate the file containing the resource model, and identify the [`Submission`][4] resource within the file.  After a consumer has located and identified the file containing the `Submission` resource, the BAGEND resource model may be used to interpret the content of the payload found in the `data/` directory of the bag.

## Identity Model
Instances of resources defined by the BAGEND model do not necessarily have an identity or existence outside of the bag they are contained within.  For example, creating a BAGEND bag necessitates the creation of a number of resources (e.g. [`Submission`][4], [`Article`][5], etc.) that never existed prior to the creation of the bag.  Therefore, these resources may not have a location on the web; there is no requirement that BAGEND resources be published online.

In any case, identifiers for BAGEND resources must be URIs.  The BAGEND identity model calls for resource identifiers that are unique within the scope of a single bag.

If a graph of BAGEND resources are placed on the web prior to the creation of a bag, the URLs of the web resources may be used as resource identifiers for the resources serialized within the bag, noting that the bag should contain the full serialization of the resource (supporting "by-reference" semantics in a bag is not a supported usecase).  Independent of the producer, the consumer of a bag may publish the resources on the web.  The degree to which a producer or consumer's infrastructure supports Linked Data may influence the desire to publish these resources, but the BAGEND specification does not make this a requirement.  In the case where the decision to publish bag resources is made, it may be appropriate to maintain a mapping of bag resource identifiers and their public locations on the web.

## Identifiers
Related to the identity model above is the general use of identifiers within the BAGEND model.  The primary identifier for all BAGEND resources must be URIs, and they must be unique within the scope of a bag.  BAGEND resources may link or relate to existing concepts or resources, which may or may not have a representation on the web.  This is generally accommodated in the Resource Model by the `identifiers` field present on each object in the model (note this field is a string and not required to be a URI), and specifically accommodated for certain concepts or identifiers such as DOIs for [`Article`][5] or ORCIDs for [`Person`][8].  The generic `identifiers` field and specialized identifier fields acknowledge that BAGEND resources may have equivalent or related resources in other systems, and these fields allow consumers and producers to reason about these links and relationships.

For example, one may be able to look up an [`Article`][5] resource given a DOI, or a [`Person`][8] resource given an ORCID.  Persons are intriging because institutions often have identifiers for people provided by a central directory service (e.g. LDAP).  To provide for local bookeeping, a producer of BAGEND bags may choose to include these opaque identifiers on the [`Person`][8] `identifiers` field.  If opaque identifiers provided by a producer are preserved by the consumer upon receipt and processing, a producer could theoretically look up resources it provided to the consumer using the identifiers known to the producer.

## Linked Data Adoption
The BAGEND resource model is designed to support Linked Data by providing a [JSON-LD][3] context and graph of resources rooted with the [`Submission`][4], but makes particular recommendations with respect to the serialization of the resource model within a BAGEND bag.  A specific goal of BAGEND is to allow adoption of the specification using idiomatic JSON, and to reduce the degree to which JSON-LD influences serialization of the resource model.  The idea is to strike a balance between the consumers who wish to adopt a Linked Data paradigm, and those who don't.  Ideally, those who don't will gradually update their infrastructure, data, and services to accommodate this paradigm.  In the meantime, current adopters of Linked Data may find the recommendations in the specification contrived or limiting.

In particular, the bag should contain a full representation of a given resource.  Requiring the consumer of the bag to resolve, download, and process BAGEND resources, all the while navigating the nuances of supported JSON-LD serializations, increases implementation complexity signfigantly.  This is because it couples the infrastructure of the producer (e.g., serving resources) of the bag to the processing implementation of the consumer (e.g., resolving and processing resources) in ways that may not be adequately specified within the scope of this work.  The BAGEND specification does encourage the use of Linked Data where it does not couple producers and consumers in unspecified ways.

If BAGEND provides value and gains traction, advanced Linked Data use cases will be considered and accommodated in future revisions of the specification.  The potential of Linked Data to provide for transparent and open processing of federally funded research are myriad, and BAGEND hopes to further its adoption.  Of particular interest is the publication of submitted resources by federal agencies to a shared Linked Data platform, which would allow for the navigation and mining of provenance, funding, metadata, and data of federally funded reasearch by the public.  A shared platform would facilitate linking of articles to data, which are being deposited in a range of repositories across agencies and institutions.  The BAGEND model may not be the basis for such a platform, but may be used to influence its design and adoption.

## Trust Model
BAGEND presumes a relationship between the producer and consumer of bags has been established out-of-band.  That is, trust is established between the sender and receiver in a way that is not specified by BAGEND.  This simplifies certain aspects of the implementation; for example, cryptographic verification of the bag or its content is not specified.


[1]: https://tools.ietf.org/html/rfc8493
[2]: http://bagend.io/bagit-profile/0.1/
[3]: https://www.w3.org/TR/json-ld11/
[4]: /model/0.1/model-datadictionary.html#submission
[5]: /model/0.1/model-datadictionary.html#article
[6]: /model/0.1/model-datadictionary.html#agreement
[7]: /model/0.1/model-datadictionary.html#contract
[8]: /model/0.1/model-datadictionary.html#person
