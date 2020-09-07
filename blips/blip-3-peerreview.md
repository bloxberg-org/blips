This BLIP is a draft: the collaborative document we use to write it is https://hackmd.io/QtoxlUimQNSJByIRKm0IwQ?edit

# Peer Review

| BLIP:     | 3                                                        |
| -------- | ------------------------------------------------------------ |
| Title:   | Peer Review                                      |
| Owner(s):  |                                                            |
| Author(s):  | Antonio Tenorio Fornés (antonio@decentralized.science), Kaan Uzdogan (uzdogan@mpdl.mpg.de), Paulo Colombo (pcolombo@ucm.es), Elena Pérez Tirador (elena@decentralized.science), Kevin Wittek (wittek@internet-sicherheit.de), Sebastian Müller (sebastian.mueller@flexdapps.com), James Lawton (lawton@mpdl.mpg.de)                       |
| Status:  | ![Raw](http://rfc.unprotocols.org/spec:2/COSS/raw.svg) |
| Created: | 2020-06-09                                                   |
| License: | BSD-2-Clause                                                 |

## Motivation

This document proposes an standard for the registration of peer reviews on the Bloxberg's blockchain. At bloxberg's bloeckchain ecosystem, there are currently several applications and projects that register peer reviews, such as Bloxberg's [peer-review-app](https://github.com/bloxberg-org/peer-review-app), [Decentralized Science](https://decentralized.science), or [peerMiles](peermiles-project.kmi.open.ac.uk/) project. An agreement of how to record these peer reviews would benefit Bloxberg's applications ecosystem, enabling interoperability among them.

### List of blochain projects registering peer reviews
Following, we list the blockchain projects that could benefit from this standard

#### Projects actively contributing to the definition of the standard
 - Bloxberg's [peer-review-app](https://github.com/bloxberg-org/peer-review-app)
 - [Decentralized Science](https://decentralized.science)
 - [peerMiles](https://peermiles-project.kmi.open.ac.uk/)

#### Other active blockchain projects with interest in peer reviewing
 - [Orvium](https://orvium.io/)

## Standard Peer Review registries
Currently, our reference implementation peer-review-app is registering many fields of a peer review such as the review author, or the ID of the reviewed article.

This standard should specify which fields to register in every record, the format and standards those fields should adopt, which are mandatory among others.


### Peer Review Records' fields
> [name=Paulo] What is the `id` field and why do we need it ?

| Field | data type | Format/s | Mandatory |  Comments|
| -------- | ---- | ---- | -------- |----|
|     id | string  | IPFS Hash??   | Yes     | The id should be unique. How to make sure reviews are unique? See [Zooko's triangle](https://en.wikipedia.org/wiki/Zooko%27s_triangle) for a relevant discussion on the compromises between decentralization, human usability and unique identifiers   |
| doi | | | |
|     timestamp | uint32     | unix timestamp     |  Yes    |
|     author | address     |      |  Yes    | |
|     journalId | string     |   ISSN    |  Yes    |
|     publisher | string     |     Name?   |  Yes    |
|     manuscriptId | string     |  DOI   |  Yes    | See isReviewOf relationship of Crossref.
|     manuscriptHash | string     |  IPFS   |  Yes    |
|     recommendation | enum (**TODO**: See other standards such as Orcid, Publons...)    |     |  Yes    |
|     url | string     |     |  Yes    | The url should give public access to the content of the review. Alternativelly, it can be a IPFS address so the content is served from a decentralized network
|     endorsers | address[]     |     |  Yes    |
|     endorsersMap | mapping(address => bool)     |     |  Yes    |
| stage  | enum | Values: pre-publication, post-publication | Optional | Defined at [Crossref peer review stage schema](https://data.crossref.org/reports/help/schema_doc/4.4.2/NO_NAMESPACE.html#peer_review_stage)|
| type | enum | Values: referee-report, 	editor-report, 	author-comment, community-comment, manuscript,	aggregate, recommendation | Optional | Defined at [Crossref peer review type schema](https://data.crossref.org/reports/help/schema_doc/4.4.2/NO_NAMESPACE.html#peer_review_type) |
| revision-round | int |  | Yes |Defined at [Crossref peer review revision-round schema](https://data.crossref.org/reports/help/schema_doc/4.4.2/NO_NAMESPACE.html#peer_review_type) |
| competing_interest_statement | string | | |
| institution |
| titles |
| license_data |


- [ ] **TODO** Decide which field could be private. For instance, the review content could be publicly shared or not depending on the reviewer's preferences.
### Existing standards (industry / de-facto / formalized)

- [ ] **TODO** Write a collection of existing standards: Crossref, Orcid, Publons, https://casrai.org/ (ORCid uses casrai, they are preparing a ISO standard on that standard proposal), etc.

- Orcid: https://support.orcid.org/hc/en-us/articles/360006971333-Peer-Review
- Crossref: https://www.crossref.org/education/content-registration/recommendations-by-content-type/peer-reviews/
  - [CrossRef UML visalization](https://github.com/semuelle/crossref-java) by @semuelle 


## Functions and Permissions


| Function name | description | parameters | permission | Comment |
| -------- | -------- | -------- |  -------- |----|
| addReview | add a review to the system     | author, journalId, publisher, manuscriptId, ManuscriptHash, recommendation, url    | reviewers can add reviews to their profile. **TODO:** consider other authorized actors such as journals or review registering services (Publons, Orcid,...) |
| addMultipleReviews | add multiple reviews to the system | ids, journalIds, publishers, manuscriptIds, manuscriptHashes, timestamps, recommendations, urls | reviewers can add multiple reviews to their profile at once |
| deleteReview | delete a review from the system | id | reviews can be deleted by the author only |
| endorseReview | endorse a review | address, id | reviews can be endorse by others only |
| addEndorser | add an endorser address | address, name? | endorser address | How should endorsers share with the network how they verify reviews? different approaches from different actors should be supported. | Should the endorser be the `msg.sender` or do we pass it as a parameter ? |

## Decentralized Endorsements for Review Validity
We should allow other reviewers and third parties to endorse for the validity of reviews registered at the system.

Different actors can have different strategies to verify a review. These actors can register their addresses as endorsers, and register a URL explaining how they validate the reviews. For instance, Bloxberg peer-review-app can verify at their servers that a review comes from Publons, and have an address that endorses after this verification is done.

### How to bring trust to public endorsers
As a decentralized application where we would allow different endorsers. Users, applications and other actors should decide which endorsers to trust. A system where users could vote up or down specific endorser's addresses could help to build trust in such addresses. Also, we could use the more advanced Token Curated Registries as a decentralized reputation system for endorses identities.

