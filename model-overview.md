---
navigation_weight: 20
---
# Resource Model Overview

## Relationship to BagIt
The BAGEND specification builds upon the [BagIt standard][1] by 1) requiring that valid and complete bags be used as defined in [RFC 8493][1], 2) requiring that bags conform to the BAGEND [BagIt profile][2], and 3) the definition and use of two novel BagIt metadata tags: `BagEnd-Submission-File` and `BagEnd-Submission-Resource`.

Requiring complete and valid bags ensures that any files referenced by BAGEND BagIt tags and BAGEND model resources will be present when the bag is processed.  

The BAGEND metadata tags `BagEnd-Submission-File` and `BagEnd-Submission-Resource` are used by bag processors to locate the file containing the resource model, and identify the `Submission` resource within the file.  After a consumer has located and identified the file containing the `Submission` resource, the BAGEND resource model may be used to interpret the content of the payload found in the `data/` directory of the bag.

## Linked Data Adoption
The BAGEND resource model is designed to support Linked Data by providing a [JSON-LD][3] context and graph of resources rooted with the `Submission`, but makes particular recommendations with respect to the serialization of the resource model within a BAGEND bag.  A specific goal of BAGEND is to allow adoption of the specification using idiomatic JSON, and to reduce the degree to which JSON-LD influences serialization of the resource model.  The idea is to strike a balance between the consumers who wish to adopt a Linked Data paradigm, and those who don't.  Ideally, those who don't will gradually update their infrastructure, data, and services to accommodate this paradigm.  In the meantime, current adopters of Linked Data may find the recommendations in the specification contrived or limiting.

## Identity Model
Instances of resources defined by the BAGEND model do not necessarily have an identity or existence outside of the bag they are contained within.  For example, creating a BAGEND bag necessitates the creation of a number of resources (e.g. `Submission`, `Article`, etc.) that never existed prior to the creation of the bag.  Therefore, these resources may not have a location on the web; there is no requirement that BAGEND resources be published online.  

The BAGEND identity model calls for resource identifiers that are unique within the scope of a single bag.  If a graph of BAGEND resources are placed on the web prior to the creation of a bag, those urls may be used as resource identifiers.  Independently, the consumer of a bag may publish the same bag resources on the web.  The degree to which a producer or consumer's infrastructure supports Linked Data may influence the desire to publish these resources, but the BAGEND specification does not make this a requirement.  In the case where the decision to publish bag resources is made, it may be appropriate to maintain a mapping of bag resource identifiers and their public locations on the web.


[1]: https://tools.ietf.org/html/rfc8493
[2]: http://bagend.io/bagit-profile/0.1
[3]: https://www.w3.org/TR/json-ld11/
