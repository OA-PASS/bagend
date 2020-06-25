---
navigation_weight: 10
---
# Specification Overview

The BAGEND specification combines [BagIt][1], the BAGEND [BagIt profile][2], and the BAGEND [resource model][4] as a means to describe and transfer research outputs between institutions and federal agencies.

This overview is intented to be non-normative, aiding the understanding of the specification itself.

## About BagIt
BagIt is an IETF informational document ([RFC 8493][1]), specifying a methodology for organizing a group of files and related metadata, typically encapsulating them as a single zip or tar file.  Attractive features of BagIt include a structured layout of the files being packaged, a built-in mechanism for ensuring the integrity of the content, and the ability to tailor the metadata contained within the package using [BagIt Profiles][5].  These features, combined with the availability of tools and software that create and process bags, make it an attractive mechanism for depositing research articles and data to federal agency systems.

## BAGEND Profile
BagIt [profiles][5] are used to constrain or expand upon the metadata contained in the bag.  This specification defines a [minimal profile][2] and two novel tags which locate and identify the BAGEND Resource Model within the package.  A bag may conform to multiple profiles; as long as the profiles do not present conflicting requirements, the `bag-info.txt` file may define any number of profiles as needs dictate.  In order for a bag to conform to the BAGEND specification, it must advertise and conform to the BAGEND profile by specifying a `BagIt-Profile-Identifier` in `bag-info.txt` with a value of `http://bagend.io/bagit-profile/0.1/`.

The minimal profile defined by the BAGEND specification allows for maximum flexibility, because it is unlikely to conflict with or exclude existing BagIt profiles.  Individual federal agencies may express unique requirements by crafting a profile on a per-agency basis.  Alternately, agencies may agree upon common requirements, which could be codified in a single, federal profile for research data.  Or, agencies may adopt an existing profile from organizations such as [APTrust][7] or [Beyond the Repository][6].  This specification does not wish to be prescriptive, but adopting an existing profile may be a reasonable path forward for expressing bag metadata requirements as various agencies investigate, interact, and adopt BAGEND bags.

Version 0.1 of the BAGEND profile can be found at: `http://bagend.io/bagit-profile/0.1/`.  Example usage of the `BagEnd-Submission-File` and `BagEnd-Submission-Resource` tag can be seen below in a hypothetical bag.

## Examples

TODO

[1]: https://tools.ietf.org/html/rfc8493
[2]: http://bagend.io/bagit-profile/0.1/
[3]: https://www.w3.org/TR/json-ld11/
[4]: /model-overview.html
[5]: https://github.com/bagit-profiles/bagit-profiles-specification
[6]: https://github.com/dpscollaborative/btr_bagit_profile
[7]: https://github.com/APTrust/bagit-profiles
