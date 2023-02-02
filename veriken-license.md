License Metadata

Version 1.0.0

Summary
An interface for a non-fungible token license metadata. The goal is to add license agreements and corresponding metadata to non-fungible tokens.

Motivation
License capabilities enhance for NFTs (Non-Fungible Tokens) for several reasons:

- Intellectual property protection: NFTs can represent unique digital assets such as art, music, and video. By licensing these assets, creators and owners can control how they are used and ensure that they are properly compensated for their work.

- Legal compliance: NFTs are subject to laws and regulations related to intellectual property, copyright, and other forms of digital assets. Licensing helps ensure that NFTs comply with these laws and regulations.

- Revenue generation: Licensing NFTs can provide a revenue stream for creators and owners by enabling them to sell their digital assets or charge for their use.

- Transparency: Licensing provides a clear understanding of the terms and conditions of NFT ownership, usage, and distribution. This helps to avoid confusion or disputes over NFTs and ensures that everyone involved understands their rights and obligations.

Licensing is an important part of the NFT ecosystem and helps to protect the rights of both creators and NFT owners supporting the the growth and development of the overall NFT market.

This standard is an extension of NFT standard enabling token creators to set license information and provides for a standard location for consumers of NFTs, such as marketplaces, to retrieve license information from tokens. NEAR's unique storage staking approach makes it feasible to store license parameters, such as personal or commecial use, on-chain to enable smart contacts to programatically interact with the licenses. License information can be set at the contract level or on a per token basis with the license information at the token level having priority. 

Prior art: 

- NEAR Non Fungible Token Standard: #171
- NEAR Non Fungible Token Metadata Standard: #177

Interface

License Metadata Type

```
type LicenseMetadata = {
  spec: string, // required, essentially a version like "lc-2.0.0", replacing "2.0.0" with the implemented version of this spec
  title: string|null, // License agreement title  "Veriken personal license" or "Creative commons license"
  description: string|null, // free-form license description
  issuer: address // Required, NEAR account/contract, must set, may be different than minting account/contract
  license: string|null, // Required, URL to off-chain human readable license agreement,, preferably to decentralized, content-addressed storage
  license_hash: string|null, // Base64-encoded sha256 hash of content referenced by the `license` field. 
  extra: string|null, // anything extra the NFT wants to store on-chain. Can be stringified JSON of license policy rules, version, template library
  reference: string|null, // URL to an off-chain JSON file with more info.
  reference_hash: string|null // Base64-encoded sha256 hash of JSON from reference field. Required if `reference` is included.
}

```
One new functions **MUST** be supported on the NFT contract that sets the version and provides **OPTIONAL** license parameters that apply to every token:

	function nft_license_metadata(): LicenseMetadata {} 

A new license attribute **MAY** be added to each Token struct if per token licensing is desired. Per token license data takes priority over license data set at the contract level.

```
type Token = {
	token_id: string,
	owner_id: string,
	metadata: TokenMetadata,
+	license:  LicenseMetadata,
}
```

LicenseMetadata:

An implementing contract **MUST** include the following fields on-chain at the contract level **AND** at the token level **IF** per token licensing is used.

- `spec`: a string. Should be lc-1.0.0 to indicate that a NFT contract adheres to the current versions of this License metadata and the Non-Fungible Token Core and Metadata specs. This will allow consumers of the Non-Fungible Token to know if they support the features of a given contract.

An implementing contract **MUST** include the following fields on-chain at either the contract level **OR** the at the individaul token level. If both are set, the token level information takes priority. 

- `license`: URL to off-chain human readable license agreement, preferably to decentralized, content-addressed storage

An implementing contract **MAY** include the following fields on-chain at either the contract level **OR** the at the individaul token level. If both are set, the token level information takes priority. 

- `title`: License agreement title such as "Veriken personal license" or "Creative commons license"
- `description`: Free-form license description
- `issuer`: Account or contract that issued the license
- `license_hash`: Base64-encoded sha256 hash of content referenced by the `license` field. 
- `extra`: Anything extra the NFT wants to store on-chain. Can be stringified JSON of license policy rules, version, template library
- `reference`: URL to an off-chain JSON file with more info.
- `reference_hash`: Base64-encoded sha256 hash of JSON from reference field. Required if `reference` is included.