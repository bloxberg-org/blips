# Research Object Certification

| BLIP:     | 2                                                         |
| -------- | ------------------------------------------------------------ |
| Title:   | Research Object Certification                                      |
| Owner(s):  | James Lawton                                                           |
| Author(s):  | James Lawton                                                           |
| Status:  | ![Draft] |
| Created: | 2020-02-18                                                   |
| License: | BSD-2-Clause                                                 |

## Motivation
Certifying that specific data or documents are generated, modified, analyzed, or processed at a specific time is valuable for the scientific community in order to protect intellectual property and incentivize data sharing. A common methodology for this is trusted timestamping which is used to ensure the creation and modification time of arbitrary data or a document. This requires a trusted authority to validate the data integrity by digitally signing upon receipt of a hash derived from the data.

In addition, as the research process can be considered a workflow - there is a large benefit to include measurements of how data has changed through the research process, for instance between initial acquisition and then preprocessing. Thus, by concatenating multiple generated certificates, the workflow of a research process can be certified and validated at a later point in time.

Our aim is to standardize the research data certification with respect to what metadata should be included to identify and certify the data, the process of research certificate generation, and the subsequent verification of an issued research certificate.

## Specification
Each desired batch of data objects or singular object is minted as a non-fungible ERC721 token. Each contract MUST include the ERC721Metadata standard augmented with an additional field that contains a unique, cryptographic hash of the bloxberg Research Object Certificate(s) included in the minting transaction. This process ensures that the research object certificate(s) can be resolved to an onchain transaction, thereby guaranteeing data integrity and provenance without disclosing sensitive information, if desired.

```solidity
/// @title Research Object Certificate ERC721Metadata
/// @dev See https://blips.bloxberg.org/blips/
///  Note: the ERC-165 identifier for this interface is 0x5b5e139f.
interface objectMetadata is ERC721Metadata  {
    /// @notice A descriptive name for a collection of NFTs in this contract
    function name() external view returns (string _name);

    /// @notice An abbreviated name for NFTs in this contract
    function symbol() external view returns (string _symbol);

    /// @notice A distinct Uniform Resource Identifier (URI) for a given asset.
    /// @dev Throws if `_tokenId` is not a valid NFT. URIs are defined in RFC
    ///  3986. The URI may point to a JSON-LD file that conforms to the "bloxberg Research Object Certificate JSON-LD Schema".
    function tokenURI(uint256 _tokenId) external view returns (string);

    /// @notice Token Hash is an algorithmic identifier generated from bloxberg Research Object Certificate JSON-LD file(s) 
    /// that MUST include a unique hash of the file byte content and metadata of the Research Object Certificate(s) to be certified.
    function tokenHash(uint256 _tokenId) external view returns (string);
}
```

## bloxberg Research Object Certificate JSON-LD Schema

For each transaction that certifies a single or batch of research objects, a corresponding Research Object Certificate [JSON-LD](https://www.w3.org/TR/json-ld/) file MUST generated.
The Research Object Certificate MUST conform to the [Verifiable Credentials](https://www.w3.org/TR/vc-data-model/) standard.
Each owner of the token is able to augment the tokenURI field to a hosting location of their choosing that can resolve to a single or batch of certificates. Similar to DOI, it is the responsibility of each token owner to update the token URI.

```json
{
    // Relevant JSON-LD context links in order to validate Verifiable Credentials according to their spec.
    "@context": ["https://www.w3.org/2018/credentials/v1", "https://w3id.org/blockcerts/schema/3.0-alpha/context.json"], 
    // UUID generated id corresponding to the certificate - OPTIONAL.
    "id": "urn:uuid:94edcd1e-2a1d-43af-9f17-ef97816954b1",
    // Credential types which declare what data to expect in the certificate.
    "type": ["VerifiableCredential", "BlockcertsCredential"],
    // Entity that issued the credential, also could be characterized as a DID.
    "issuer": "https://raw.githubusercontent.com/bloxberg-org/issuer_json/master/issuer.json",
    // Date & Time that the credential was issued.
    "issuanceDate": "2020-09-10T13:05:11.996623+00:00", 
    // claim about recipient of the certificate.
    "credentialSubject": {
        // identifier for the subject of the certificate.
        "id": "einstein@mpg.de", 
        // assertion about the subject of the certificate.
        "alumniOf": {
            "id": "https://bloxberg.org"
        }
    }, 
    // Html to render when certificate is verified - OPTIONAL.
    "displayHtml": "<h1>bloxberg Certificate</h1><h2>This bloxberg certificate serves as a proof of existence that the data corresponding to the SHA256 Hash were transacted on the bloxberg blockchain at the issued time.</h2>", 
    // Cryptographic hash that is derived from the research object to certify. The exact hashing algorithm can be generalized, but must uniquely identify a file such as SHA256, SHA-3, or ISCC.
    "hash": "0x0e4ded5319861c8daac00d425c53a16bd180a7d01a340a0e00f7dede40d2c9f6", 
    // Crytographic hashing mechanism used to derive value in *hash*.
    "hashType": "SHA256",
    // Digital proof that ensures tamper-resistance.
    "proof": {
        // Cryptographic signature suite used to generate the signature.
        "type": "MerkleProof2019", 
        // Date the signature was created.
        "created": "2020-09-10T13:05:15.582085", 
        // Digital signature value derived from the cryptographic signature suite.
        "proofValue": "z2LuLBVSfnVzaQtvzwDVNGE9aeuRToPVTvSHcbXMwzwS7ngfWtkBdbgfGaKJFM8W3GHN7MeAQ3zwt7dfESxWiY7Y4M3FxHg9pefhXggXgZPBYkZo9RUXMEkyu8xaxEoF8t6jqeMGARMZortEkgfCCTJMLGsfMfMXPcam4chnQwjhkTnmcZhRjoFUg13NZLwjsWYG961uv4inAiWHjBwM52kkv6vSD8EyTgXFjfooChsRXFiN4VykwPcUWBMRkuinHNwvrewx8dTPjijxdFAn1zDKJdUGn3erbVgV7VhMBbfmv7RQStgKbA1D6FvQNAVwsbW25NEEQ1mnGsBXDFH2EC1coFwRQTLTTDpiEjdKh4tRqk5kTycmpk1c1Zihm4d4URUMybAw1NmG4Hi12JKqZr", 
        // purpose of the proof.
        "proofPurpose": "assertionMethod", 
        // identifier of the bloxberg public key that can verify the signature. 
        "verificationMethod": "ecdsa-koblitz-pubkey:0xD748BF41264b906093460923169643f45BDbC32e"
    },
    // Generalized metadata field that can contain additional data to describe the certificate - OPTIONAL
    "@metadata": { 
        "researchObjectName": "NeuronalImpulsePatient12.csv" 
      }
}
```
The bloxberg research object certificate offers flexibility in what specific metadata fields could be included in the metadata field. This is to account for the breadth of scientific disciplines, privacy or data security requirements, and different stages of the research workflow.

## Verification of Certificates
The unique hash identifier encoded in the *proofValue* calculated from the corresponding proof mechanism listed in *type* must identically match 

Steps to Verify Certificate:

<ol>
<li>Transaction ID is validated against the bloxberg blockchain via a query. Transaction ID is obtained by decoding (if necessary) the proof value to obtain the anchoring information for the transaction.</li>
<li> The local hash of the single or batch research object certification is computed from the certificate *proofValue* and determined whether it is valid.</li>
<li> The remote hash stored in the variable *tokenHash* of the corresponding transaction ID is checked whether it is valid.</li>
<li> Issuer keys are parsed to ensure that the DID or issuer profile is correctly defined and issued the relevant certificate.</li>
<li> The remote and local hash are compared to confirm correctness.</li>
</ol>


These steps ensure that the certificate is valid and secured on the bloxberg blockchain on the issuanceDate. Additional verification steps can be taken to ensure data integrity of any individual research object secured in a batch or individually:
<ol>
<li>Compute cryptographic hash of certified research object according to function listed in variable *hashType*.</li>
<li>Compare computed value with value secured in research object certificate and ensure that they are identical.</li>
</ol>

## References
<ol>
<li>Verifiable Credentials Data Model 1.0. https://www.w3.org/TR/vc-data-model/.</li>
<li> Merkle Proof Signature Suite 2019. https://w3c-ccg.github.io/lds-merkle-proof-2019/.</li>
<li>International Standard Content Code. https://iscc.codes.</li>
<li>ERC-721 Non-Fungible Token Standardhttps://eips.ethereum.org/EIPS/eip-721</li>
</ol>
