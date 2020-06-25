---
navigation_weight: 15
---
# DRAFT

This is a DRAFT specification, not intended for use or reference.

## Introduction

This specification documents BagIt Enabled Deposit (BAGEND), a profile for BagIt that incorporates semantic description of the bag payload, specifically supporting the transfer of publications, data, bibliographic metadata, contributors, and their funding sources.

It provides a low barrier to entry for producers and consumers of conformant bags by allowing the use of idiomatic JSON for semantic description. It introduces linked data which allows for the development and evolution of advanced use cases to be implemented on an as-needed basis.

The key words &quot;MUST&quot;, &quot;MUST NOT&quot;, &quot;REQUIRED&quot;, &quot;SHALL&quot;, &quot;SHALL NOT&quot;, &quot;SHOULD&quot;, &quot;SHOULD NOT&quot;, &quot;RECOMMENDED&quot;, &quot;MAY&quot;, and &quot;OPTIONAL&quot; in this document are to be interpreted as described in RFC 2119.

## Relationship to BagIT

Packages conformant with this specification MUST comport with BagIT version 1.0 ([RFC 8493](https://tools.ietf.org/html/rfc8493)), and MUST be [valid and complete bags](https://tools.ietf.org/html/rfc8493#section-3). Bags conforming to this specification MUST advertise their compliance by including a `BagIt-Profile-Identifier` tag in `bag-info.txt` with the value `http://bagend.io/bagit-profile/0.1/`. Consumers MAY ascribe the semantics present in the BAGEND Resource Model to [standard BagIt tags](https://tools.ietf.org/html/rfc8493#section-2.2.2) present in `bag-info.txt`.

If the directory `/resources/bagend` exists (relative to the base directory of the bag) and a `BagIt-Profile-Identifier` tag exists with a value of `http://bagend.io/bagit-profile/0.1/` in `bag-info.txt`, then files contained in this directory SHOULD be interpreted according to this specification as entities of the BAGEND Resource Model describing the content contained in the BagIt [payload directory](https://tools.ietf.org/html/rfc8493#section-2.1.2), `/data`.

If `/resources/bagend` contains a `Submission` from the BAGEND Resource Model, then the full path (relative to the base directory of the bag) to the file containing the `Submission` MUST be specified by the `BagEnd-Submission-File` tag in `bag-info.txt`. The identity of the `Submission` resource SHOULD be specified by the `BagEnd-Submission-Resource` tag in `bag-info.txt`.

TODO: verbiage about the exploded bag directory name matching the serialized bag file basename?

## BAGEND Resource Model

Bags conforming to the BAGEND profile SHOULD serialize and transmit an instance of the Resource Model with the package. If the Resource Model is included, it MUST be located under the directory (relative to the base directory of the bag) named `/resources/bagend`. The Resource Model, if present, MUST contain at most one `Submission`. The `Submission`, if present, MUST contain exactly one `Article`.

Resources MUST be serialized as JSON-LD. Compliant serializations SHOULD be in compacted form, and MUST use the context located at `http://bagend.io/model/0.1/context.jsonld` for representing the instances and relationships of the Resource Model. Arbitrary keys and values MAY be present in resource serializations, and resource consumers SHOULD ignore any unrecognized elements.

Compliant serializations MAY embed the context directly in the resource serialization, or include and reference a copy of the context in the `/resources/bagend` directory to facilitate off-line processing.

Resources serialized as files SHOULD use the file extension `.jsonld`.

Resources SHOULD be serialized in a single file, using embedding (as described in [JSON-LD 1.1 §4.5](https://www.w3.org/TR/json-ld/#embedding)) instead of linking between discreet files in order to manifest relationships between model elements.

Consumers MAY process the Resource Model as JSON without interpreting it as Linked Data.

### Model Description

This section is non-normative.

The BAGEND Resource Model is used to describe the custodial content of the bag, and is composed of the following conceptual entities:

Table 1: Overview of the Resource Model entities

| Resource Model Entity | Description |
| --- | --- |
| Agreement | Represents terms or conditions that govern the submission of the Article. |
| Article | The intellectual content being published, typically a copy of an article that has been accepted for publication, post peer-review. |
| Award | Represents funding that enabled or contributed to the research documented or performed in the article. |
| File | Encapsulates the technical metadata of a finite, ordered, stream of binary digits that are contained within the bag payload directory. An Article may link to a File representing its content, supporting figures, tables, or data. |
| Journal | Encapsulates a Journal and its metadata. |
| Organization | Represents an entity unified by management, vision, or legal framework that may act as an agent. |
| Person | Represents an individual agent that contributed to the Submission in some way. |
| Publication | Represents the Article in the context of a printed publication. |
| Submission | Aggregates the entities of the Resource Model as a cohesive whole and provides a place to record provenance detail. |

### Alignment with BagIt

This section is non-normative.

If a `BagIt-Profile-Identifier` tag exists with a value of `http://bagend.io/bagit-profile/0.1/` in `bag-info.txt`, then the consumer may apply semantics of the Resource Model to bag metadata where their fields align. This may be useful when a bag which conforms to the BAGEND profile does not contain a Resource Model instance. If values present in bag metadata conflict with values in an existing Resource Model, the Resource Model values take precedence.

Table 2: Semantic alignment of BagIt metadata tags and Resource Model elements

| BagIt Tag | Description | Resource Model Element |
| --- | --- | --- |
| Source-Organization, Organization-Address | Described by BagIt as &quot;Organization transferring the content.&quot; | The affiliated Organization of the Submission Contact |
| Contact-Name, Contact-Phone, Contact-Email | Described by BagIt as &quot;Person at the source organization who is responsible for the content transfer.&quot; | Submission Contact |
| External-Description | Described by BagIt as &quot;A brief explanation of the contents and provenance.&quot;
 | Submission Description |
| External-Identifier | Described by BagIt as &quot;A sender-supplied identifier for the bag.&quot; | Submission Correlation ID, or any other suitable Submission identifier |
| Bagging-Date | Described by BagIt as &quot;Date (YYYY-MM-DD) that the content was prepared for transfer.&quot; | Submission Creation Date |
| Internal-Sender-Identifier | Described by BagIt as &quot;An alternate sender-specific identifier for the content and/or bag&quot; | Any suitable Submission identifier |
