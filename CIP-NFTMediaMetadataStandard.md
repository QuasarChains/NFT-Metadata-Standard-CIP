---
CIP:
Title: NFT Media Metadata Standard
Authors: Alessandro Konrad <alessandro.konrad@live.de>, Smaug <smaug@pool.pm>
Comments-URI:
Status: Draft
Type: Informational
Created: 2021-04-08
Post-History: https://forum.cardano.org/t/cip-nft-metadata-standard/45687 and https://www.reddit.com/r/CardanoDevelopers/comments/mkhlv8/nft_metadata_standard/
License: CC-BY-4.0
---

## Abstract

This proposal defines an NFT Media Metadata Standard for Native Tokens.

## Motivation

## Specification

This is the registered `transaction_metadatum_label` value

| transaction_metadatum_label | description  |
| --------------------------- | ------------ |
| 721                         | NFT Metadata |

### Structure

```
{
  "721": {
    "<policy_id>": {
      "<asset_name>": {
        "name": "<name>",

        "image": "<uri | array>",
        "mime": "<mime_type>",

        "description": "<description>"

        "files": [{
          "name": "<name>",
          "mime": "<mime_type>",
          "src": "<uri | array>",
          <other_properties>
        }],

        <other properties>
      }
    }
    "version": "1.0"
  }
}
```

#### CDDL

```
{
  721: {
    <policy_id>: {
      <asset_name>: {
        name: tstr,

        image: uri, [tstr] //
        ? mime: tstr,

        ? description: tstr,

        ? files: [file]

      }
    }
  }
}

file = {
  name: tstr,
  ? mime: tstr,
  src: uri, [tstr] //
}
```

The **`image`** and **`name`** property are marked as required. **`image`** should be an URI pointing to a resource with mime type `image/*` used as thumbnail or as actual link if the NFT is an image (ideally <= 1MB).

The **`description`** property is optional.

The **`mime`** and **`src`** properties are optional. If **`mime`** is defined, **`src`** will be an URI pointing to a resource of this mime type. If the mime type is `image/*`, **`src`** points to the same image in an higher resolution.

The **`version`** property is also optional. If not specified the version is "1.0". It will become mandatory in future versions if needed.

This structure really just defines the basis. New properties and standards can be defined later on for varies uses cases. That's why there is an "other properties" tag.

This also makes this structure backward compatible with the most common format currently used in the blockchain.

## References

- Mime type: https://tools.ietf.org/html/rfc6838.
- CIP about reserved labels: https://github.com/cardano-foundation/CIPs/blob/master/CIP-0010/CIP-0010.md
- EIP-721: https://eips.ethereum.org/EIPS/eip-721
- URI: https://tools.ietf.org/html/rfc3986, https://tools.ietf.org/html/rfc2397

## Copyright

This CIP is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode).
