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

TODO
Referencing an existing resource within the model is done by using its identifier, denoted by `@identifier`.

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
Agreements link a signatory (an instance of [`Person`][model-person]) to a [`Contract`][model-cont] on a given date.  In our example, there is one agreement that was signed by the submitter (the `contract-text` is elided for brevity), whereby a license is granted by the submitter of materials to the repository for the purpose of archiving and dissemination.

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

{% highlight json %}
{% raw %}


{% endraw %}
{% endhighlight %}
#### Awards

{% highlight json %}
{% raw %}


{% endraw %}
{% endhighlight %}
#### Publications

{% highlight json %}
{% raw %}


{% endraw %}
{% endhighlight %}

#### Authors

{% highlight json %}
{% raw %}


{% endraw %}
{% endhighlight %}

#### Files

{% highlight json %}
{% raw %}


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

