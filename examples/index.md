---
navigation_weight: 30
---
# Example Bag

## Bag Declaration
This is an example [`bagit.txt`][bag-decl], which is standard file included in all BagIt bags.  The BAGEND [profile][bagend-profile] accepts version `0.97` or `1.0`.

{% highlight text %}
BagIt-Version: 1.0
Tag-File-Character-Encoding: UTF-8
{% endhighlight %}

## Bag Metadata
Here is an example [`bag-info.txt`][bag-info] file, which contains metadata about the bag.  The BAGEND specification [requires][spec-bagit] this file be present.  BAGEND bags _must_ include a metadata tag `BagIt-Profile-Identifier` with the value of `http://bagend.io/bagit-profile/0.1/`.

Since most, if not all, BAGEND bags contain a [`Submission`][model-sub], two additional tags _should_ be present:
* BagEnd-Submission-File
* BagEnd-Submission-Resource

These elements allow BAGEND processors to locate and process the resource model in the bag.

Addtional bag metadata is recommended, but it is not required.

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

In this example, a decision was made by the creator of the bag to use the correlation identifer of the `Submission` resource as the `External-Identifier` in the bag metadata.  The correlation identifer will be discussed in more detail later.

## BAGEND resources walkthrough
The resources file in a BAGEND bag is located under the `/resources/bagend` directory of the bag.  It can be named whatever you wish; its location is specified by the `BagEnd-Submission-File` element in `bag-info.txt`.  All of the BAGEND resources exist in the same file, and form a graph rooted in the `Submission`.  The identify of the `Submission` resource is specified by the `BagEnd-Submission-Resource` element in `bag-info.txt`.  A BAGEND bag doesn't have to contain a `Submission`, but if it does, it must only contain one.

> A BAGEND bag lacking a `Submission` would still allow for the metadata elements in `bag-info.txt` to be [interpreted according to BAGEND semantics][spec-align]

Recall that BAGEND resources are represented as JSON-LD.  If you aren't familiar with JSON-LD, that should be OK.  JSON-LD elements will be prefixed with the `@` character, but otherwise ought to be pretty self-explanatory.  For example, a JSON object containing the element `"@type": "Submission"` and `"@id": "foo"` can be understood to carry the "type" of "Submission" and identifer "foo".  The JSON-LD context (`@context`) carries special significance, as it provides JSON-LD aware processors with the information necessary to interpret the JSON as instance of an RDF data model.

### Outline of a BAGEND resource model
Here is an overview of the major elements contained in an example resource file.  

The `Submission` is the root resource, and it serves to aggregate the remaining elements in the resource graph.  The immediate resources are the article (whose files reside in the bag payload directory), any agreements related to the submission process, and contact information the various parties involved in creating the submission.  

The `Submission` can be thought of the resource that maintains the bookkeeping related to the transfer of the custodial content of the bag.  In theory, once a bag has been received, the information in the `Submission` _could_ be thrown away or ignored, only processing the `Article` which describes the custodial content of the bag.

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

### Submitter, Custodial Contact, and Infrastructure Contact

### Agreements

### Article

#### Awards

#### Publications

#### Authors

#### Files

[bagend-profile]: /bagit-profile/0.1/
[bag-decl]: https://tools.ietf.org/html/rfc8493#section-2.1.1
[bag-info]: https://tools.ietf.org/html/rfc8493#section-2.2.2
[spec-bagit]: /spec/0.1/#relationship-to-bagit
[spec-align]: /spec/0.1/#alignment-with-bagit
[model-sub]: /model/0.1/model-datadictionary.html#submission
