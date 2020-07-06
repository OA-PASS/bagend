---
navigation_weight: 30
---
# Example Bag
Throughout this document we'll be working with an [exemplar bag][exbag-bin] whose structure is listed below.  It is a simple, but real, example of how a published resource may be transmitted and described using BAGEND.  Those familiar with [BagIt][bagit] will recoginze this layout:
{% highlight text %}
.
├── bag-info.txt
├── bagit.txt
├── data
│   ├── JCDL2015_DemoArticle.pdf
│   └── JCDL2015_Poster_HandoutSize.pdf
├── manifest-sha1.txt
├── manifest-sha512.txt
├── resources
│   └── bagend
│       └── resources.jsonld
├── tagmanifest-sha1.txt
└── tagmanifest-sha512.txt
{% endhighlight %}

This document doesn't explain BagIt itself (there are plenty of existing resources online for that), but we will go over where BAGEND uses or extends BagIt for its purposes.

## Bag Declaration
[This][exbag-decl] is an example [`bagit.txt`][bag-decl], which is a standard file included in all BagIt bags.  The BAGEND [profile][bagend-profile] accepts version `0.97` or `1.0`.

{% highlight text %}
BagIt-Version: 1.0
Tag-File-Character-Encoding: UTF-8
{% endhighlight %}

## Bag Metadata
Below is an [example][exbag-info] [`bag-info.txt`][bag-info] file, which contains metadata about the bag.  The BAGEND specification [requires][spec-bagit] this file be present.

{% highlight text %}
BagIt-Profile-Identifier: http://bagend.io/bagit-profile/0.1/
BagEnd-Submission-File: /resources/bagend/resources.jsonld
BagEnd-Submission-Resource: https://pass.jhu.edu/fcrepo/rest/submissions/d8/67/77/ca/d86777ca-f17b-41ea-8ad2-5fe5f10ebff2
Source-Organization: Digital Research and Curation Center, Johns Hopkins University, Sheridan Libraries
Organization-Address: 3400 N Charles St, Baltimore MD, 21218
Contact-Name: DRCC Operations
Contact-Email: infra@library.jhu.edu
Bagging-Date: 2018-11-27
External-Identifier: https://pass.jhu.edu/fcrepo/rest/submissions/d8/67/77/ca/d86777ca-f17b-41ea-8ad2-5fe5f10ebff2
Payload-Oxum: 296508.2
{% endhighlight %}

BAGEND bags _must_ include a metadata tag `BagIt-Profile-Identifier` with the value of `http://bagend.io/bagit-profile/0.1/`.

Since most, if not all, BAGEND bags contain a [`Submission`][model-sub], two additional tags _should_ be present.  These tags allow processors to locate the `Submission` resource.
* `BagEnd-Submission-File`
* `BagEnd-Submission-Resource`

> Another option for referencing and locating resources within a bag was proffered by the [Data Conservancy Packaging specification][dc-baguri].  BAGEND did not go so far as to define or reuse the `bag://` URI scheme, but future iterations of BAGEND may (re-)introduce them.

A couple of things to note:
* The additional tags (`Source-Organization`, `Organization-Address`, etc.) are recommended, but not required.
* A decision was made by the creator of the bag to use the correlation identifer of the `Submission` resource as the `External-Identifier` in the bag metadata.  The correlation identifer will be discussed in more detail later.
* BAGEND bags are not required to have a `Submission`.  This allows for a usecase where metadata in `bag-info.txt` are [interpreted or mapped][spec-modeldesc] to BAGEND resource model elements.  We do not anticipate this to be the primary mode of BAGEND resource representation.

## BAGEND resources walkthrough
The resources file in a BAGEND bag is located under the `/resources/bagend` directory.  It can be named whatever you wish; its location is specified by the `BagEnd-Submission-File` element in `bag-info.txt`.  All of the BAGEND resources exist in the same file, and form a graph rooted in the `Submission`.  The identify of the `Submission` resource is specified by the `BagEnd-Submission-Resource` element in `bag-info.txt`.  A BAGEND bag doesn't have to contain a `Submission`, but if it does, it must contain only one.

> A BAGEND bag lacking a `Submission` would still allow for the metadata elements in `bag-info.txt` to be [interpreted according to BAGEND semantics][spec-align]

Recall that BAGEND resources are represented as JSON-LD.  If you aren't familiar with JSON-LD, that should be OK.  JSON-LD elements will be prefixed with the `@` character, otherwise the resources should read easily.  For example, a JSON object containing the element `"@type": "Submission"` and `"@id": "foo"` can be understood to carry the "type" of "Submission" and identifer "foo".  The JSON-LD context (`@context`) carries special significance, as it provides JSON-LD aware processors with the information necessary to interpret the JSON as instance of an RDF data model.

A link to the example resources file is [here][exbag-model], and the JSON-LD context can be found [here][exbag-ctx].

## Resource identifiers
The `@id` field contains the resource identifier, and is how resources in BAGEND refer to each other.  Resource identifiers must be URIs, but other than that, there are no restrictions.  

> Referencing an existing resource within the model is done by using its identifier, denoted by `@identifier`.

Resource identifiers in the example bag come in a few different forms, and it is worth asking if some forms should be preferred over others.
* Some resources are identified using URIs from a different context (for example, URIs prefixed with `https://pass.jhu.edu` or `http://www.jhu.edu`) that resolve to a representation of the resource that context.
* Other resources have a "made up" identifier that looks like it is from a different context, but do not resolve (e.g. the identifier for the `Agreement` in this example is `http://pass.jhu.edu#agreement`, which looks like a URL but doesn't actually resolve)
* Still other resources use the `instance` prefix for compact IRIs, e.g. `instance:Article1` which expands to `http://bagend.io/instance/0.1#Article1`.  These URIs do not resolve.

> The JSON-LD prefix `instance` is provided by the JSON-LD context, and may be used to create IRIs for resources.

In addition to identifying and linking resources, the model provides for reconciling or mapping between the BAGEND resource and other contexts.  For example, the [`Article`][model-art] supports identifiers for popular contexts such as CrossRef, PubMed, PMC, and the publisher's item identifier.  BAGEND cannot account for all existing identifier schemes and contexts, and it cannot anticipate new schemes or contexts.  It supports ppopular contexts and schemes based on stakeholder feedback, and more may be added in the future.

The generic `identifiers` field, which is present on every resource, can be used to capture those identifiers whose scheme or context is not yet represented in the BAGEND resource model.  This includes identifiers that may be relevant to the producer of the bag which they wish to be preserved or captured by the consumer of the bag.

### Outline of a BAGEND resource model
Here is an overview of the major elements contained in an example resource file, rooted in the `Submission`.  We'll consider each of these resources in turn.

{% highlight json %}
{% raw %}
{
  "@context": "http://bagend.io/model/0.1/context.jsonld",
  "@type": "Submission",
  "@id": "https://pass.jhu.edu/fcrepo/rest/submissions/d8/67/77/ca/d86777ca-f17b-41ea-8ad2-5fe5f10ebff2",
  "identifiers": [
    "https://pass.jhu.edu/fcrepo/rest/submissions/d8/67/77/ca/d86777ca-f17b-41ea-8ad2-5fe5f10ebff2"
  ],

  "correlation-id": "https://pass.jhu.edu/fcrepo/rest/submissions/d8/67/77/ca/d86777ca-f17b-41ea-8ad2-5fe5f10ebff2",
  "creation-date": "2018-11-27T16:02:46.95Z",
  "description": "The goal of the RMap Project is to create a prototype service that can capture and preserve maps of relationships amongst the increasingly distributed components (article, data, software, workflow objects, multimedia, etc.) that comprise the new model for scholarly publication.",
  
  "submitter": { ... },
  
  "custodial-contact": { ... },
  
  "infrastructure-contact": { ... },
  
  "agreements": [ ... ],
  
  "article": { ... }
}
{% endraw %}
{% endhighlight %}

### Submission
The `Submission` can be thought of the resource that maintains the bookkeeping related to the transfer of the custodial content of the bag.  It accounts for the persons or agents who contributed to the creation of the bag and its content, captures any licenses, terms of service, or other agreements encountered (or anticipated) in the submission process, and provides a detailed description of the published article and any data contained in the bag payload directory.

### Submitter, Custodial Contact, and Infrastructure Contact
BAGEND provides the ability to record three different roles related to the creation of a bag.  Each of these resources are technically optional, but the receiver of a bag may use them to facilitate processing of the bag.
* The person who is actually submitting the bag
* The person who is responsible for the technical infrastructure creating the bag
* The person who is responsible for the content of the bag payload

Each of these roles will be represented by a [`Person`][model-person].

{% highlight json %}
{% raw %}
  "submitter": {
    "@id": "https://pass.jhu.edu/fcrepo/rest/users/21/36/86/ff/213686ff-da91-455b-977d-b1bae238d9b6",
    "@type": "Person",
    "given-name": "Sayeed",
    "family-name": "Choudhury",
    "email": "sayeed@jhu.edu",
    "identifiers": [
      "johnshopkins.edu:employeeid:00000000",
      "johnshopkins.edu:hopkinsid:ABCDEF",
      "johnshopkins.edu:jhed:gchoudh2"
    ],
    "affiliation": {
      "@id": "http://www.jhu.edu",
      "@type": "Organization",
      "organization-name": "Johns Hopkins University, Sheridan Libraries",
      "geo-location": "geo:39.3290571,-76.6216137",
      "country-name": "United States",
      "region": "Maryland",
      "locality": "Baltimore",
      "postal-code": "21218",
      "street-address": "3400 N Charles St"
    }
  }
{% endraw %}
{% endhighlight %}

You can see that a `Person` encapulates expected attributes like name, email etc, but it also accommodates their affiliation (an [`Organization`][model-org]).  If you have a second `Person` in the resource model that shares the same affiliation, you don't need to repeat the affiliation; you can reference it by its `@identifier`.

> JSON processors will need to be prepared to handle objects or object references.  The `affiliation` of the `custodial-contact` below refers to the `Organization` object defined above in the `submitter`.

{% highlight json %}
{% raw %}
  "custodial-contact": {
    "@id": "https://pass.jhu.edu/fcrepo/rest/users/00222680",
    "@type": "Person",
    "given-name": "Karen",
    "family-name": "Hanson",
    "email": "karen.hanson@jhu.edu",
    "orcid": "https://orcid.org/0000-0002-9354-8328",
    "identifiers": [
      "johnshopkins.edu:employeeid:11111111",
      "johnshopkins.edu:hopkinsid:GHIJKL",
      "johnshopkins.edu:jhed:khanson5"
    ],
    "affiliation": "http://www.jhu.edu"
  }
{% endraw %}
{% endhighlight %}


### Agreements
Agreements link a signatory (an instance of [`Person`][model-person]) to a [`Contract`][model-cont] on a given date.  In our example, there is one agreement that was signed by the submitter (the `contract-text` is elided for brevity), whereby a license is granted by the submitter of materials to the repository for the purpose of archiving and dissemination.  At this point, any agreements are associated with the entire [`Submission`][model-sub], including the [`Article`][model-art] and all [files][model-file].

{% highlight json %}
{% raw %}
  "agreements": [
    {
      "@type": "Agreement",
      "@id": "http://pass.jhu.edu#agreement",
      "signatory": "https://pass.jhu.edu/fcrepo/rest/users/21/36/86/ff/213686ff-da91-455b-977d-b1bae238d9b6",
      "effective-date": "2018-11-27T19:58:34.790Z",
      "contract-role": "license",
      "contract": {
        "@type": "Contract",
        "@id": "http://pass.jhu.edu#j10p-license",
        "contract-name": "Deposit requirements for JScholarship",
        "contract-description": "Grant of license by submitter for materials submitted to JScholarship",
        "contract-text": "NON-EXCLUSIVE LICENSE FOR USE OF MATERIALS This non-exclusive license defines the terms for the deposit of Materials ... "
      }
    }
  ]
{% endraw %}
{% endhighlight %}

### Article
The [`Article`][model-art] represents the intellectual content of a BAGEND payload.  An overview of the [`Article`][model-art] is below, showing the linkages to other resources.

{% highlight json %}
{% raw %}
  "article": {
    "@type": "Article",
    "@id": "instance:Article1",
    "doi": "10.1145/2756406.2756952",
    "crossrefId": "https://api.crossref.org/works/10.1145/2756406.2756952",
    "title": "The RMap Project",
    "abstract": "The goal of the RMap Project ... ",
    
    "awards": [ ... ],
    
    "publications": [ ... ],
    
    "authors": [ ... ],
    
    "files": [ ... ]
  }
{% endraw %}
{% endhighlight %}

The [`Article`][model-art] resource provides links or identifiers to its representation in other contexts.  The CrossRef `works` identifier is linked to, and the DOI of the published article is also provided.  Note that the DOI is not encoded as a URL (e.g. https://dx.doi.org/10.1145/2756406.2756952).  If a processor wishes to resolve a DOI, they are responsible for employing the necessary logic to do so.

The awards represent the funding used to produce any of the research in the article or its associated files.  The publications are the representation of the BAGEND article in a published journal (print or online, or both).  The authors link to [people][model-person] that authored the article, and the files link to the article's representation along with any associate data.


#### Awards
[Awards][model-award] identify the funding sources for the research present in the article or its data files.

{% highlight json %}
{% raw %}
    "awards": [
      {
        "@id": "https://pass.jhu.edu/fcrepo/rest/grants/53/9f/73/9e/539f739e-ac21-4dab-9bf8-987e9bc5af54",
        "@type": "Award",
        "identifiers": [
          "johnshopkins.edu:grant:116920"
        ],
        "award-name": "Alfred Sloan Foundation Data Curation Infrastructure",
        "agency-award-number": "2014-3-24",
        "award-start": "2014-03-28T00:00:00.000Z",
        "award-end": "2017-04-01T00:00:00.000Z",
        "pi": "https://pass.jhu.edu/fcrepo/rest/users/21/36/86/ff/213686ff-da91-455b-977d-b1bae238d9b6",
        "award-contact": "https://pass.jhu.edu/fcrepo/rest/users/21/36/86/ff/213686ff-da91-455b-977d-b1bae238d9b6",
        "sponsor": {
          "@id": "https://pass.jhu.edu/fcrepo/rest/funders/ea/79/7b/d5/ea797bd5-f982-4ef3-8797-c785d53229d3",
          "@type": "Organization",
          "identifiers": [
            "johnshopkins.edu:funder:302749"
          ],
          "organization-name": "Alfred P Sloan Research Foundation"
        }
      }
    ]
{% endraw %}
{% endhighlight %}

Note the use of the `identifiers` field.  In this example, the producer of the bag includes local identifiers for the award (`johnshopkins.edu:grant:116920`) and the awarding organization (`johnshopkins.edu:funder:302749`).  This implies that this [`Award`][model-award] has some identity in a context that is opaque to anyone else other than the creator of this particular identifier.  The rationale for including this identifier in the BAGEND resource model is that it may be maintained and even indexed by the consumer.  This would allow for the producer of the bag to search and find the [`Award`][model-award] (or all [`Submission`][model-sub] or [`Article`][model-art]) with that identifier.

> Often organizations will have different identifiers for the same concept, and may not maintain a local mapping between the institutions identifier and the funders identifier.  If BAGEND consumers commit to maintaining and even indexing the opaque `identifiers` for BAGEND resources, this increases their discoverabily by the institutions producing those resources.

#### Publications
A [`Publication`][model-pub] represents the BAGEND [`Article`][model-art] in the context of an online or print publication.  Typically the [`Publication`][model-pub] will include an embedded [`Journal`][model-journal], as is shown here.

{% highlight json %}
{% raw %}
    "publications": [
      {
        "@type": "Publication",
        "@id": "instance:Publication1",
        "publication-date-electronic": "2020-04-20T00:00:00Z",
        "journal": {
          "@id": "instance:ProcACMIEEJoinConfDigitLibr",
          "@type": "Journal",
          "journal-title": "Proceedings of the 15th ACM/IEEE-CE on Joint Conference on Digital Libraries - JCDL '15",
          "issn-print": "1552-5996",
          "issn-linking": "1552-5996",
          "publisher-name": "ACM Press",
          "journal-id-nlm": "101587668",
          "journal-id-nlmta": "Proc ACM/IEEE Joint Conf Digit Libr"
        }
      }
    ]
{% endraw %}
{% endhighlight %}

Note the use of the `instance` prefix; the producer of this bag used it to generate the resource IRIs for the `Publication` and `Journal`.

#### Authors
The authors of the article are captured by the `authors` key of the [`Article`][model-art].  Each author is a [`Person`][model-person] resource.

{% highlight json %}
{% raw %}
    "authors": [
      "https://pass.jhu.edu/fcrepo/rest/users/00222680",
      {
        "@id": "instance:Author2",
        "@type": "Person",
        "given-name": "Tim",
        "family-name": "DiLauro",
        "email": "timmo@jhu.edu",
        "orcid": "https://orcid.org/0000-0002-9997-3464",
        "affiliation": "http://www.jhu.edu"
      },
      {
        "@id": "instance:Author3",
        "@type": "Person",
        "given-name": "Mark",
        "family-name": "Donoghue",
        "email": "m.donoghue@ieee.org",
        "orcid": "https://orcid.org/0000-0002-2803-1816",
        "affiliation": {
          "@id": "uri:urn:IEEE",
          "@type": "Organization",
          "organization-name": "IEEE",
          "street-address": "45 Hoes Lane",
          "locality": "Piscataway",
          "region": "New Jersey",
          "postal-code": "08854",
          "country-name": "United States"
        }
      }
    ]
{% endraw %}
{% endhighlight %}

The interesting thing about this example is that there are three authors: one of them is referenced using its `@identifer`, while the other two authors are provided as JSON objects.  It so happens that the custodial contact of this submission is also one of the authors.  A [`Person`][model-person] with an `@identifier` of `https://pass.jhu.edu/fcrepo/rest/users/00222680` was defined earlier as the `custodial-contact` of the [`Submission`][model-sub].  Rather than re-key in all the information of the author, they can simply be referenced.  

> JSON processers will need to be able to handle the situation where an object may be provided by value or by reference.

An interesting consideration is what happens when a [`Person`][model-person] is defined elsewhere, but you wish to add an addtional attribute: what if the `custodial-contact` was defined but their ORCID was not included:
{% highlight json %}
{% raw %}
  "custodial-contact": {
    "@id": "https://pass.jhu.edu/fcrepo/rest/users/00222680",
    "@type": "Person",
    "given-name": "Karen",
    "family-name": "Hanson",
    "email": "karen.hanson@jhu.edu",
    "identifiers": [
      "johnshopkins.edu:employeeid:11111111",
      "johnshopkins.edu:hopkinsid:GHIJKL",
      "johnshopkins.edu:jhed:khanson5"
    ],  
    "affiliation": "http://www.jhu.edu"
  }
{% endraw %}
{% endhighlight %}

And when listing the authors, you wanted to include the individual's ORCID?  There are two options, one of which is preferred by BAGEND.

* Option 1: Add the ORCID to the [`Person`][model-person] instance defined by the `custodial-contact` (this is approach taked by the  [example][exbag-model])
* Option 2: Embed an instance of [`Person`][model-person] as the author, use the same `@identifier` as the `custodial-contact`, and just add their ORCID as a property

Example of Option 2:
{% highlight json %}
{% raw %}
    "authors": [
      {
        "@id": "https://pass.jhu.edu/fcrepo/rest/users/00222680",
        "@type": "Person",
        "orcid": "https://orcid.org/0000-0002-9354-8328"
      },
      {
        ...
      },
      {
        ...
      }
    ]
{% endraw %}
{% endhighlight %}

From a JSON-LD perspective, either option will produce the same RDF.  But if you are processing the resources as plain JSON, the processing required to support option 2 is more sophisticated.  For this reason, we prefer that an instance of a resource be fully defined in a single place, rather than adding additional properties to the instance throughout.

#### Files

{% highlight json %}
{% raw %}
    "files": [
      {
        "@type": "File",
        "@id": "instance:File1",
        "location": "/data/JCDL2015_DemoArticle.pdf",
        "file-name": "JCDL2015_DemoArticle.pdf",
        "media-type": "application/pdf",
        "size-bytes": "239795",
        "checksums": [
          "sha1:1f0cabe0b8ae2afe5468df1b6ad98a4a51c77373"
        ],
        "file-roles": [
          "manuscript"
        ]
      },
      {
        "@type": "File",
        "@id": "instance:File2",
        "location": "/data/JCDL2015_Poster_HandoutSize.pdf",
        "file-name": "JCDL2015_Poster_HandoutSize.pdf",
        "media-type": "application/pdf",
        "size-bytes": "56713",
        "checksums": [
          "sha1:b59b769295fe84374d5a1f152233c4a7cd259b04"
        ],
        "file-roles": [
          "figure"
        ]
      }
    ]
{% endraw %}
{% endhighlight %}

[bagend-profile]: /bagit-profile/0.1/
[bagit]: https://tools.ietf.org/html/rfc8493
[bag-decl]: https://tools.ietf.org/html/rfc8493#section-2.1.1
[bag-info]: https://tools.ietf.org/html/rfc8493#section-2.2.2
[spec-bagit]: /spec/0.1/#relationship-to-bagit
[spec-align]: /spec/0.1/#alignment-with-bagit
[spec-modeldesc]: /spec/0.1/#model-description
[model-sub]: /model/0.1/model-datadictionary.html#submission
[dc-baguri]: http://dataconservancy.github.io/dc-packaging-spec/dc-packaging-spec-1.0.html#a4
[exbag-ctx]: /model/0.1/context.jsonld
[exbag-bin]: /assets/samples/0.1/rmap-bagend-example.tar.gz
[exbag-decl]: /assets/samples/0.1/rmap/bagit.txt
[exbag-info]: /assets/samples/0.1/rmap/bag-info.txt
[exbag-model]: /assets/samples/0.1/rmap/resources/bagend/resources.jsonld
[exbag-payload]: /assets/samples/0.1/rmap/data/
[model-person]: /model/0.1/model-datadictionary.html#person
[model-org]: /model/0.1/model-datadictionary.html#organization
[model-cont]: /model/0.1/model-datadictionary.html#contract
[model-sub]: /model/0.1/model-datadictionary.html#submission
[model-art]: /model/0.1/model-datadictionary.html#article
[model-file]: /model/0.1/model-datadictionary.html#file
[model-award]: /model/0.1/model-datadictionary.html#award
[model-pub]: /model/0.1/model-datadictionary.html#publication
[model-journal]: /model/0.1/model-datadictionary.html#journal

