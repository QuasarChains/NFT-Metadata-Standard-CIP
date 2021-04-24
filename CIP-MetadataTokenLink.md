---
CIP:
Title: Metadata Token Link
Authors: Alessandro Konrad <alessandro.konrad@live.de>, Smaug <smaug@pool.pm>
Comments-URI:
Status: Draft
Type: Informational
Created: 2021-04-24
Post-History: https://forum.cardano.org/t/cip-nft-metadata-standard/45687 and https://github.com/cardano-foundation/CIPs/pull/85#discussion_r610473096
License: CC-BY-4.0
---

## Abstract

This proposal defines, how to create a unique link between Native Tokens and on-chain metadata.

## Motivation

Tokens on Cardano are a part of the ledger. Unlike on Ethereum, where metadata can be attached to a token through a smart contract, this isn't possible on Cardano because tokens are native and Cardano uses a UTxO ledger, which makes it hard to directly attach metadata to a token.
So the link to the metadata needs to be established differently.
Cardano has the ability to send metadata in a transaction, that's the way we can create a link between a token and the metadata. To make the link unique, the metadata should be appended to the same transaction, where the token forge happens:

> Given a token in a EUTXOma ledger, we can ask “where did this token come from?” Since tokens
> are always created in specific forging operations, we can always trace them back through their
> transaction graph to their origin.

(Section 4.1 in the paper: https://hydra.iohk.io/build/5400786/download/1/eutxoma.pdf)

## Considerations

That being said, we have unique metadata link to a token and can always prove that with 100% certainty. No one else can manipulate the link except if the policy allows it to ([update mechanism](#update-metadata-link-for-a-specific-token)).

## Specification

To implement the following structure a **`metadatum_label`** needs to be chosen ([CIP10](https://github.com/cardano-foundation/CIPs/blob/master/CIP-0010/CIP-0010.md)).

### Structure

The structure allows for multiple token mints, also with different policies, in a single transaction.

```
{
  "<metadatum_label>": {
    "<policy_id>": {
      "<asset_name>": {
       ...token_metadata
      }
    }
  }
}
```

Example with multiple tokens:

```
{
  "<metadatum_label>": {
    "<policy_id>": {
      "<asset_name>": {
       ...token_metadata
      },
       "<another_asset_name>": {
       ...token_metadata
      }
    },
     "<another_policy_id>": {
      "<asset_name>": {
       ...token_metadata
      }
    }
  }
}
```

This also makes this structure backward compatible with the most common format currently used in the blockchain.

### Retrieve valid metadata for a specific token

As mentioned above this metadata structure allows to have either one token or multiple tokens with also different policies in a single mint transaction. A third party tool can then fetch the token metadata seamlessly. It doesn't matter if the metadata includes just one token or multiple. The proceedure for the third party is always the same:

1. Find the latest mint transaction with the **`metadatum_label`** in the metadata of the specific token
2. Lookup the **`metadatum_label`** key
3. Lookup the Policy Id of the token
4. Lookup the Asset name of the token
5. You end up with the correct metadata for the token

### Update metadata link for a specific token

Using the latest mint transaction with the **`metadatum_label`** as valid metadata for a token allows to update the metadata link of this token. As soon as a new mint transaction is occurring including metadata with the **`metadatum_label`**, the metadata link is considered updated and the new metadata should be used. This is only possible if the policy allows to mint or burn the same token again.

## References

- CIP about reserved labels: https://github.com/cardano-foundation/CIPs/blob/master/CIP-0010/CIP-0010.md

## Copyright

This CIP is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode).
