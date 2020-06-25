---
navigation_weight: 25
---
# Resource Model Data Dictionary

| **[`Agreement`](#Agreement)** |
| --- |
| Encapsulates any and all form of agreement or contract related to the Submission, Article, or Data Files |

| **Field Name** | **JSON-LD Field Name** | **JSON-LD Type** | **JSON Type** | **Description** |
| --- | --- | --- | --- | --- |
| Agreement Identifier | @id | IRI | string | An identifier supplied by the agent creating the package. The identifier is an opaque IRI that uniquely identifies this Agreement entity within the resource model of the package. |
| Agreement Entity Type | @type | IRI | string | A fixed value: &quot;Agreement&quot; |
| Identifiers | identifiers | Value Object | Array of string | Arbitrary identifiers that are not otherwise defined in the data model. |
| Signatory (a `Person`) | signatory | IRI or Node Object | String or object | The individual who is agreeing to the Contract associated with this Agreement. |
| Effective Date | effective-date | Value Object (xsd:dateTime) | string | The date a license is granted, copyright is claimed, or terms of use agreed upon. |
| Role | contract-role | Value Object | string | The role of the Contract associated with the Agreement (e.g. a &quot;Terms of Service&quot;, &quot;License&quot;, etc.) |
| Contract (a `Contract`) | contract | IRI or Node Object | String or object | The terms of use agreed upon, license being granted, or other contract. |


| **[`Article`](#Article)** |
| --- |
| Represents the Author Accepted Manuscript or Published Article. The primary intellectual output captured by a Submission. |

| **Conceptual Field** | **JSON-LD Field Name** | **JSON-LD Field Type** | **JSON Type** | **Description** |
| --- | --- | --- | --- | --- |
| File Identifier | @id | IRI | string | An identifier supplied by the agent creating the package. The identifier is an opaque IRI that uniquely identifies this File entity within the resource model of the package. |
| File Entity Type | @type | IRI | string | A fixed value: &quot;File&quot; |
| Identifiers | identifiers | Value Object | Array of string | Arbitrary identifiers that are not otherwise defined in the data model. |
| Title | title | Value Object | string | The title of this Article |
| Abstract | abstract | Value Object | string | The abstract of this Article |
| DOI | doi | Value Object | string | The DOI of the published version of this Article. Note that this value should _not_ encode the DOI as a URI. |
| PubMed ID | pubmedId | Value Object | string | The PubMed identifier of this Article |
| PMC ID | pmcId | Value Object | string | The PubMed Central identifier of this Article |
| CrossRef ID | crossrefId | Value Object | string | The CrossRef identifier of this Article |
| PII | pii | Value Object | string | The Publisher Item Identifier of this Article |
| Authors (each is a `Person`) | authors | IRI or Node Object | Array of string or object | The Authors of this Article |
| Publications (each is a `Publication`) | publications | IRI or Node Object | Array of string or object | For example the electronic publication and the print publication. |
| Awards (each is an `Award`) | awards | IRI or Node Object | Array of string or object | Funding that contributed to intellectual content of this Article or associated Files |
| Files (each is a `File`) | files | IRI or Node Object | Array of string or object | Data files for the submission, including the manuscript and any supplementary data. |


| **[`Award`](#Award)** |
| --- |
| The award or awards that funded the research represented in the Article or data |

| **Conceptual Field** | **JSON-LD Field Name** | **JSON-LD Field Type** | **JSON Type** | **Description** |
| --- | --- | --- | --- | --- |
| Award Identifier | @id | IRI | string | An identifier supplied by the agent creating the package. The identifier is an opaque IRI that uniquely identifies this Award entity within the resource model of the package. |
| Award Entity Type | @type | IRI | string | A fixed value: &quot;Award&quot; |
| DOI | doi | Value Object | string | The DOI identifying this Award |
| Identifiers | identifiers | Value Object | Array of string | Arbitrary identifiers that are not otherwise defined in the data model. |
| Award Name | award-name | Value Object | string | The name or title of this Award |
| Agency Award Number | agency-award-number | Value Object | string | The alpha-numeric string assigned by the funding agency to this Award |
| Funding Agency (an `Organization`) | sponsor | IRI or Node Object | String or object | The Organization which funded this Award |
| Award Start | award-start | Value Object | string | The date the funding for this Award started |
| Award End | award-end | Value Object | string | The date the funding for this Award ended, or will end |
| PI (a `Person`) | pi | IRI or Node Object | String or object | The Principle Investigator for this Award |
| Co-Is (a `Person`) | cois | IRI or Node Object | Array of string or object | Any Co-Investigators for this Award |
| Award Contact (a `Person`) | award-contact | IRI or Node Object | String or object | The institutional contact for this Award |


| **[`Contract`](#Contract)** |
| --- |
| Encapsulates any and all form of agreement or contract related to the Submission, Article, or Data Files |

| **Conceptual Field Name** | **JSON-LD Field Name** | **JSON-LD Field Type** | **JSON Type** | **Description** |
| --- | --- | --- | --- | --- |
| Contract Identifier | @id | IRI | string | An identifier supplied by the agent creating the package. The identifier is an opaque IRI that uniquely identifies this Contract entity within the resource model of the package. |
| Contract Entity Type | @type | IRI | string | A fixed value: &quot;Contract&quot; |
| Identifiers | identifiers | Value Object | Array of string | Arbitrary identifiers that are not otherwise defined in the data model. |
| Name | contract-name | Value Object | string | A name for this Contract |
| Description | contract-description | Value Object | string | Short, human-readable description of the agreement, licence, or terms of use. |
| Full Text | contract-text | Value Object | string | The full text of the agreement, license, or terms of use. |
| Canonical Location | contract-location | Value Object (xsd:anyURI) | string | The canonical location of the full text of this Contract. For example, the location of the [legal text](https://creativecommons.org/publicdomain/zero/1.0/legalcode) of the CC-0 license or [ASF 2.0 license](https://www.apache.org/licenses/LICENSE-2.0.txt). |
| See Also | see-also | Value Object (xsd:anyURI) | string | The splash page, or layperson equivalent view of an agreement, licence, or terms of use. For example, the CC-0 [splash page](https://creativecommons.org/publicdomain/zero/1.0/) or ASF 2.0 [splash-page](https://www.apache.org/licenses/LICENSE-2.0). |


| **[`File`](#File)** |
| --- |
| Represents the binary files and their roles associated with an Article. These would be considered part of the custodial content of a package. |

| **Conceptual Field** | **JSON-LD Field Name** | **JSON-LD Field Type** | **JSON Type** | **Description** |
| --- | --- | --- | --- | --- |
| File Identifier | @id | IRI | string | An identifier supplied by the agent creating the package. The identifier is an opaque IRI that uniquely identifies this File entity within the resource model of the package. |
| File Entity Type | @type | IRI | string | A fixed value: &quot;File&quot; |
| Identifiers | identifiers | Value Object | Array of string | Arbitrary identifiers that are not otherwise defined in the data model. |
| Roles | file-roles | Value Object | Array of string | E.g. Manuscript, Supplement, Figure, Table |
| Name | file-name | Value Object | string | The logical file name |
| Path | file-path | Value Object | string | The logical file path |
| Location | location | Value Object | string | The physical location of the file within the package. This value is relative to the base directory of the bag. |
| Canonical Location | canonical-location | Value Object (xsd:anyURI) | string | The canonical location resolving to the bytes of this file |
| Checksums | checksums | Value Object | Array of string | Checksums of the file content, in the form \&lt;algorithm\&gt;:\&lt;value\&gt; |
| Media Type | media-type | Value object | string | The IANA media type of the file (e.g. application/pdf) |
| Size | size-bytes | Value object | number | The length of this File in bytes |


| **[`Journal`](#Journal)** |
| --- |
| Metadata describing a Journal in print and/or electronic form |

| **Conceptual Field Name** | **JSON-LD Field Name** | **JSON-LD Type** | **JSON Type** | **Description** |
| --- | --- | --- | --- | --- |
| Journal Identifier | @id | IRI | string | An identifier supplied by the agent creating the package. The identifier is an opaque IRI that uniquely identifies this Journal entity within the resource model of the package. |
| Journal Entity Type | @type | IRI | string | A fixed value: &quot;Journal&quot; |
| NLM Journal ID | journal-id-nlm | Value Object | string | The NLM Journal ID for this Journal. |
| NLM Title Authority Abbreviation | journal-id-nlmta | Value Object | string | The NLM title authority abbreviation for this Journal. |
| Identifiers | identifiers | Value Object | Array of string | Arbitrary identifiers that are not otherwise defined in the data model. |
| Title | journal-title | Value Object | string | The Journal title. |
| Electronic ISSN | issn-electronic | Value Object | string | The ISSN of the electronic version of this Journal. |
| Print ISSN | issn-print | Value Object | string | The ISSN of the print version of this Journal. |
| Linking ISSN | issn-linking | Value Object | string | The Linking ISSN for this Journal. |
| Publisher Name | publisher-name | Value Object | string | The name of the Journal publisher. |


| **[`Organization`](#Organization)** |
| --- |
| Represents an organization related to the research or funding of the Article |

| **Conceptual Field Name** | **JSON-LD Field Name** | **JSON-LD Field Type** | **JSON Type** | **Description** |
| --- | --- | --- | --- | --- |
| Organization Identifier | @id | IRI | string | An identifier supplied by the agent creating the package. The identifier is an opaque IRI that uniquely identifies this Organization entity within the resource model of the package. |
| Organization Entity Type | @type | IRI | string | A fixed value: &quot;Organization&quot; |
| Identifiers | identifiers | Value Object | Array of string | Arbitrary identifiers that are not otherwise defined in the data model. |
| Name | organization-name | Value Object | string | The name of this Organization |
| SciVal Organization ID | scivalId | IRI | string | The Elsevier SciVal identifier for this Organization |
| RoR ID | rorId | IRI | string | Resource Organization Registry Identifier for this Organization |
| GRID ID | gridId | IRI | string | Global Research Identifier for this Organization |
| ISNI ID | isniId | IRI | string | International Standard Name Identifier for this Organization |
| Crossref ID | crossrefId | IRI | string | CrossRef ID for this Organization |
| NIH Institution Profile File | ipf | Value Object | string | The NIH IPF number for this Organization |
| DUNS Number | duns | Value Object | string | The Dun &amp; Bradstreet number for this Organization |
| Geographic Coordinates | geo-location | IRI | string | The VCard Geo URI for this Organization (e.g. &quot;geo:39.3299054,-76.6227064&quot;) |
| Street Address | street-address | Value Object | string | The street address of this Organization |
| Locality (i.e. City) | locality | Value Object | string | The locality (i.e. City) of this Organization |
| Region (i.e. State) | region | Value Object | string | The region (i.e. State) of this Organization |
| Country | country-name | Value Object | string | The country of this Organization |
| Postal Code | postal-code | Value Object | string | The postal code (i.e. zip code) of this Organization |


| **[`Person`](#Person)** |
| --- |
| Accounts for people and their roles |

| **Conceptual Field** | **JSON-LD Field Name** | **JSON-LD Field Type** | **JSON Type** | **Description** |
| --- | --- | --- | --- | --- |
| Person Identifier | @id | IRI | string | An identifier supplied by the agent creating the package. The identifier is an opaque IRI that uniquely identifies this Person entity within the resource model of the package. |
| Person Entity Type | @type | IRI | string | A fixed value: &quot;Person&quot; |
| Identifiers | identifiers | Value Object | Array of string | Arbitrary identifiers that are not otherwise defined in the data model. |
| First Name | given-name | Value Object | string | The first, or given, name of this Person |
| Last Name | family-name | Value Object | string | The last name of this Person |
| Affiliation (an `Organization`) | affiliation | IRI or Node Object | String or object | The Organization this Person is affiliated with. |
| Phone | phone | Value Object | string | The phone number of this Person |
| Email | email | Value Object | string | The primary email address of this Person |
| ORCID | orcid | IRI | string | The ORCID of this Person encoded as an IRI |


| **[`Publication`](#Publication)** |
| --- |
| Represents the Article in the context of its publication |

| **Conceptual Field** | **JSON-LD Field Name** | **JSON-LD Field Type** | **JSON Type** | **Description** |
| --- | --- | --- | --- | --- |
| Publication Identifier | @id | IRI | string | An identifier supplied by the agent creating the package. The identifier is an opaque IRI that uniquely identifies this Publication entity within the resource model of the package. |
| Publication Entity Type | @type | IRI | string | A fixed value: &quot;Publication&quot; |
| Identifiers | identifiers | Value Object | Array of string | Arbitrary identifiers that are not otherwise defined in the data model. |
| Volume | volume | Value Object | string | The volume of the Journal this Publication appears in. |
| Issue | issue | Value Object | string | The issue of the Journal this Publication appears in. |
| Page Start | page-start | Value Object | string | The starting page of this Publication in the Journal |
| Page End | page-end | Value Object | string | The ending page of this Publication in the Journal |
| Electronic Publication Date | publication-date-electronic | Value Object (xsd:dateTime) | string | The date this Publication was made available electronically |
| Print Publication Date | publication-date-print | Value Object (xsd:dateTime) | string | The date this Publication was made available in print |
| Journal | journal | IRI or Node Object | String or object | The Journal this Publication was published in |


| **[`Submission`](#Submission)** |
| --- |
| Represents a submission to an agency, containing the Article, Data Files, Award information, people involved in the submission process, and any agreements or contracts signed as part of the submission. |

| **Conceptual Field** | **JSON-LD Field Name** | **JSON-LD Field Type** | **JSON Type** | **Description** |
| --- | --- | --- | --- | --- |
| Submission Identifier | @id | IRI | string | An identifier supplied by the agent creating the package. The identifier is an opaque IRI that uniquely identifies this Submission entity within the resource model of the package. |
| Submission Entity Type | @type | IRI | string | A fixed value: &quot;Submission&quot; |
| Identifiers | identifiers | Value Object | Array of string | Arbitrary identifiers that are not otherwise defined in the data model. |
| Correlation Identifier | |
 | string | An identifier supplied by the sender of the package used for the express purpose of correlating information present in the model with agency records. For example, this would be an identifier suitable for PAC-M exports. |
| Article (an `Article`) | article | IRI or Node Object | String or object | The Article describes the intellectual (i.e. custodial) content of the Submission. A Submission must contain exactly one Article. |
| Award (an `Award`) | awards | IRI or Node Object | Array of string or object | Awards from funding agencies that support the research manifested in the Submission. |
| Custodial Contact (a `Person`) | custodial-contact | IRI or Node Object | String or object | The administrative contact for custodial content of this submission. This individual would complete the submission in an agency system, or address mistakes made in the content of the submission (e.g. errors in the article or supporting data). The _Custodial_ Contact and the _Submitter_ will often be the same individual, but may be an individual who has prepared the Submission on behalf of the researcher. As opposed to the _Infrastructure_ Contact, who is the contact for the IT infrastructure that created the Submission. |
| Submitter (a `Person`) | submitter | IRI or Node Object | String or object | The individual responsible for performing the submission; typically the same individual who is the signatory on any Agreements present in the Submission. |
| Agreements (each is an `Agreement`) | agreements | IRI or Node Object | Array of string or object | Agreements signed as part of the submission process or workflow. |
| Creation Date | created-date | Value Object (xsd:dateTime) | string | The date and time this Submission was created. |
| Description | submission-description | Value Object | string | A brief, human-readable, description of the Submission and its provenance. |
| Infrastructure Contact (a `Person`) | infrastructure-contact | IRI or Node Object | String or object | The person or agent responsible for creating the bag and the resource model (e.g. a system administrator or software program). As opposed to the _Custodial_ Contact who is responsible for the intellectual content contained within the Submission, and the _Submitter_ who is responsible for performing the Submission. |