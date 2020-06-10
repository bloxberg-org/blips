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

A draft for a standardized peer review registry on the Bloxberg Blockchain.

## Standard Peer Review registries
Currently, our reference implementation peer-review-app is registering many fields of a peer review such as the review author, or the ID of the reviewed article.

This standard should specify which fields to register in every record, the format and standards those fields should adopt, which are mandatory among others.


### Peer Review Records' fields
> [name=Paulo] What is the `id` field and why do we need it ?

| Field | data type | Format/s | Mandatory |  Comments|
| -------- | ---- | ---- | -------- |----|
|     id | string  | IPFS Hash??   | Yes     | The id should be unique. How to make sure reviews are unique? See [Zooko's triangle](https://en.wikipedia.org/wiki/Zooko%27s_triangle) for a relevant discussion on the compromises between decentralization, human usability and unique identifiers   |
|     timestamp | uint32     | unix timestamp     |  Yes    |
|     author | address     |      |  Yes    |
|     journalId | string     |   ISSN    |  Yes    |
|     publisher | string     |     Name?   |  Yes    |
|     manuscriptId | string     |  DOI   |  Yes    |
|     manuscriptHash | string     |  IPFS   |  Yes    |
|     recommendation | enum (**TODO**: See other standards such as Orcid, Publons...)    |     |  Yes    |
|     url | string     |     |  Yes    | The url should give public access to the content of the review. Alternativelly, it can be a IPFS address so the content is served from a decentralized network
|     endorsers | address[]     |     |  Yes    |
|     endorsersMap | mapping(address => bool)     |     |  Yes    |


- [ ] **TODO** Decide which field could be private. For instance, the review content could be publicly shared or not depending on the reviewer's preferences.
### Existing standards (industry / de-facto / formalized)

- [ ] **TODO** Write a collection of existing standards: Crossref, Orcid, Publons, https://casrai.org/ (ORCid uses casrai, they are preparing a ISO standard on that standard proposal), etc.

- Orcid: https://support.orcid.org/hc/en-us/articles/360006971333-Peer-Review
- Crossref: https://www.crossref.org/education/content-registration/recommendations-by-content-type/peer-reviews/


## Functions and Permissions


| Function name | description | parameters | permission |
| -------- | -------- | -------- |  -------- |
| addReview | add a review to the system     | author, journalId, publisher, manuscriptId, ManuscriptHash, recommendation, url    | reviewers can add reviews to their profile. **TODO:** consider other authorized actors such as journals or review registering services (Publons, Orcid,...) |
- [ ] **TODO** finish previous function specification table

## Decentralized Endorsements for Review Validity
We should allow other reviewers and third parties to endorse for the validity of reviews registered at the system.

Different actors can have different strategies to verify a review. These actors can register their addresses as endorsers, and register a URL explaining how they validate the reviews. For instance, Bloxberg peer-review-app can verify at their servers that a review comes from Publons, and have an address that endorses after this verification is done.

### How to bring trust to public endorsers
As a decentralized application where we would allow different endorsers. Users, applications and other actors should decide which endorsers to trust. A system where users could vote up or down specific endorser's addresses could help to build trust in such addresses. Also, we could use the more advanced Token Curated Registries as a decentralized reputation system for endorses identities.
