---
navigation_weight: 10
---
# BAGEND Specification Overview

BagIt is an IETF informational document, specifying a methodology for organizing a group of files and related metadata, typically encapsulating them as a single zip or tar file.  Attractive features of BagIt include a structured layout of the files being packaged, a built-in mechanism for ensuring the integrity of the content, and the ability to tailor the metadata contained within the package using BagIt Profiles.  These features, combined with the availability of tools and software that create and process bags, make it an attractive mechanism for depositing research articles and data to federal agency systems.

BagIt profiles are used to constrain or expand upon the metadata contained in the bag.  This specification defines a minimal profile and two novel tags which locate and identify the BAGEND Resource Model within the package.  A bag may conform to multiple profiles; as long as the profiles do not present conflicting requirements, the bag-info.txt file may define any number of profiles as needs dictate.  In order for a bag to conform to the BAGEND specification, it must advertise and conform to the BAGEND profile by specifying BagIt-Profile-Identifier in bag-info.txt with a value of http://bagend.io/bagit-profile/0.1.

The minimal profile defined by the BAGEND specification allows for maximum flexibility, because it is unlikely to conflict with or exclude existing BagIt profiles.  Individual federal agencies may express unique requirements by crafting a profile on a per-agency basis.  Alternately, agencies may agree upon common requirements, which could be codified in a single, federal profile for research data.  Or, agencies may adopt an existing profile from organizations such as APTrust or Beyond the Repository.  This specification does not wish to be prescriptive, but adopting an existing profile may be a reasonable path forward for expressing bag metadata requirements as various agencies investigate, interact, and adopt BAGEND bags.

The BAGEND profile can be found here: <TODO LINK>.  Example usage of the BagEnd-Submission-File and BagEnd-Submission-Resource tag can be seen below in a hypothetical bag.
