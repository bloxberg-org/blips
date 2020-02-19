# Research Data Certificates

| BLIP:     | 0001                                                         |
| -------- | ------------------------------------------------------------ |
| Title:   | Research Data Certificates                                      |
| Author(s):  | James Lawton                                                           |
| Status:  | ![Draft](http://rfc.unprotocols.org/spec:2/COSS/draft.svg) |
| Created: | 2020-02-18                                                   |
| License: | BSD-2-Clause                                                 |

## Purpose
Certifying that specific data or documents are generated, modified, analyzed, or processed at a specific time is valuable for the scientific community in order to protect intellectual property and incentivize data sharing. A common methodology for this is trusted timestamping which is used to ensure the creation and modification time of arbitrary data or a document. This requires a trusted authority to validate the data integrity by digitally signing upon receipt of a hash derived from the data.

In addition, as the research process can be considered a workflow - there is a large benefit to include measurements of how data has changed through the research process, for instance between initial acquisition and then preprocessing. Thus, by concatenating multiple generated certificates, the workflow of a research process can be certified and validated at a later point in time.

Our aim is to standardize the research certificate with respect to what metadata should be included to identify the data, the process of research certificate generation, and the subsequent validation of an issued research certificate.

## Schema
Each certified piece of data is minted as a non-fungible (*Transferable ERC721 compliant or non-transferable) token. Each contract must include the ERC721Metadata standard in order to include the various metadata necessary for future identification and locating of the data object. Each owner of the token is able to augment the tokenURI field to a hosting location of the their choosing. Similar to DOI, it is the responsibility of each token owner to update the token URI. The URI may point to a JSON file that conforms to the EIP-1047 Metadata JSON Schema augmented with specific characteristics desirable for use with respect to scientific applications.

```json
{
    "title": "Research Object Metadata",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "description": "Identifies the asset to which this NFT represents"
        },
        "description": {
            "type": "string",
            "description": "Describes the asset to which this NFT represents"
        },
        "image": {
            "type": "string",
            "description": "A URI pointing to a resource with mime type image/* representing the asset to which this NFT represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive."
        },
        "claimant": {
            "author(s)": {
                "type": "array",
                "description": "Array of the contributors and/or authors of the data associated with the corresponding ISCC."
            },
            "organization(s)": {
                "type": "array",
                "description": "Array of organizations that are stakeholders in the certified data."
            }
        },
        "ISCC": {
            "type": "string",
            "description": "ISCC is an algorithmic identifier generated from the content itself. This currently includes metadata similarity, normalized content similarity, encoded data similarity, and exact data integrity."
        },
        "certificate": {
            "type": "string",
            "description": "a URI pointing to the corresponding data certificate in PDF format. The certificate can be utilized to prove via certified timestamping the data certification time."
        },
        "external_url": {
            "type": "string",
            "description": "a URI pointing to the corresponding data file where the data can be downloaded. If provided, the data at the URI should resolve to the same ISCC listed in the metadata.
        }
    }
}
```





## Updating
ISCC - https://iscc.codes/features/



## Re-Assigning



## Validation


