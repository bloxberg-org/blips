# Research Object Certification

| BLIP:     | 0001                                                         |
| -------- | ------------------------------------------------------------ |
| Title:   | Research Object Certification                                      |
| Owner(s):  | James Lawton                                                           |
| Author(s):  | James Lawton                                                           |
| Status:  | ![Draft](http://rfc.unprotocols.org/spec:2/COSS/draft.svg) |
| Created: | 2020-02-18                                                   |
| License: | BSD-2-Clause                                                 |

## Motivation
Certifying that specific data or documents are generated, modified, analyzed, or processed at a specific time is valuable for the scientific community in order to protect intellectual property and incentivize data sharing. A common methodology for this is trusted timestamping which is used to ensure the creation and modification time of arbitrary data or a document. This requires a trusted authority to validate the data integrity by digitally signing upon receipt of a hash derived from the data.

In addition, as the research process can be considered a workflow - there is a large benefit to include measurements of how data has changed through the research process, for instance between initial acquisition and then preprocessing. Thus, by concatenating multiple generated certificates, the workflow of a research process can be certified and validated at a later point in time.

Our aim is to standardize the research data certification with respect to what metadata should be included to identify the data, the process of research certificate generation, and the subsequent validation of an issued research certificate.

## Specification
Each certified piece of data is minted as a non-fungible (*Transferable ERC721 compliant or non-transferable) token. Each contract MUST include the ERC721Metadata standard augmented with an additional field that contains the hash of the data object. This is in order to ensure that the dataURI hosted offchain can be resolved to an onchain transaction. The ERC721Metadata is necessary to include the information for future identification of the data object.

```solidity
/// @title Research Object Metadata Extension 
/// @dev See https://blips.bloxberg.org/blips/
///  Note: the ERC-165 identifier for this interface is 0x5b5e139f.
interface objectMetadata /* is ERC721 */ {
    /// @notice A descriptive name for a collection of NFTs in this contract
    function name() external view returns (string _name);

    /// @notice An abbreviated name for NFTs in this contract
    function symbol() external view returns (string _symbol);

    /// @notice A distinct Uniform Resource Identifier (URI) for a given asset.
    /// @dev Throws if `_tokenId` is not a valid NFT. URIs are defined in RFC
    ///  3986. The URI may point to a JSON file that conforms to the "BLIPS-0001
    ///  Metadata JSON Schema".
    function tokenURI(uint256 _tokenId) external view returns (string);

    /// @notice Algorithmic Identifier generated from the content and 
    /// metadata from the data to be certified. Currently utilizing the ISCC
    /// standard
    function hashIdentifier(uint256 _tokenId) external view returns (string);
}
```

## Metadata JSON Schema
 Each owner of the token is able to augment the tokenURI field to a hosting location of their choosing. Similar to DOI, it is the responsibility of each token owner to update the token URI. The URI may point to a JSON file that conforms to the EIP-1047 Metadata JSON Schema augmented with specific characteristics desirable for use with respect to scientific applications and to ensure compatibility across applications.

```json
{
    "title": "Research Object Metadata",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "required": false,
            "description": "Identifies the asset to which this NFT represents"
        },
        "description": {
            "type": "string",
            "required": false,
            "description": "Describes the asset to which this NFT represents"
        },
        "image": {
            "type": "string",
            "required": false,
            "description": "A URI pointing to a resource with mime type image/* representing the asset to which this NFT represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive."
        },
        "claimant": {
            "author(s)": {
                "type": "array",
                "required": true,
                "description": "Array of the contributors and/or authors of the data associated with the corresponding ISCC."
            },
            "organization(s)": {
                "type": "array",
                "required": false,
                "description": "Array of organizations that are stakeholders in the certified data."
            }
        },
        "iscc": {
            "type": "string",
            "required": true,
            "description": "ISCC is an algorithmic identifier generated from the content itself. This currently includes metadata similarity, normalized content similarity, encoded data similarity, and exact data integrity."
        },
        "certificate": {
            "type": "string",
            "required": false,
            "description": "a URI pointing to the corresponding data certificate in PDF format. The certificate can be utilized to prove via certified timestamping the data certification time."
        },
        "external_url": {
            "type": "string",
            "required": false,
            "description": "a URI pointing to the corresponding data file where the data can be downloaded. If provided, the data at the URI should resolve to the same ISCC listed in the metadata."
        }
        ,
        "timestamp": {
            "type": "string",
            "required": true,
            "description": "UNIX timestamp that corresponds to the block confirmation time when the research data object token was minted."
        }
    }
}
```

The research data schema offers flexibility in what specific metadata fields are required to be included. This is for several reasons: 1. Data generated at differ stages of the research cycle, it may not be possible to provide this information. 2. To ensure the protection of sensitive and confidential data that is acquired, analyzed, or processed. However, the required field ISCC, is required in order to ensure subsequent validation of data at a later point in time.

In addition, due to the algorithmic design of [ISCC](https://iscc.codes/), it is possible to see a similarity matching of how data has been modified during the research workflow.

## Validation

The hash identifier contained in the metadata extension for a given tokenID must identically match the data object it is referencing. The data object can be either publicly or privately available to concerned parties. Furthermore, by referencing the UNIX time of the block confirmation that included the token mint transaction, a timestamp corresponding to the research object can be verified.  The data certificate serves as a user-friendly method to provide evidence of research object certification. The certificate must include the hash code of the data object, an external_url of the data object that corresponds to the hash, and timestamp of the block when the token was minted.
