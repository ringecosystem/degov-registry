# Registry Contribution Guide

This document explains how to add or update DAO entries in this repository.
The source of truth is always the repository itself:

- `config.yml`
- `daos/*.yml`
- `schemas/config.schema.json`
- `schemas/dao.schema.json`

If an example and a schema ever disagree, follow the schema first and then
update the example.

## Collaboration paths

External contributors can work with the registry in two ways:

1. GitHub issue
   Use the issue template when you want maintainers to help prepare the files.
   This is the best option when you have the DAO details but do not want to edit
   the repository yourself.
2. GitHub pull request
   Open a PR when you can prepare the YAML files directly. Follow the examples
   and validation rules in this guide.

## Recommended contribution flow

For a brand new DAO, use this lifecycle:

1. Create `daos/<dao-code>.yml`.
2. Add an entry to the correct network list in `config.yml`.
3. Set `state: draft` while the integration is being synced or reviewed.
4. Change the same entry to `state: active` after the DAO is ready for normal
   use in DeGov Square.

Notes:

- Some existing entries do not write `state` explicitly.
- For new contributions, prefer writing `state` explicitly so reviewers can see
  the lifecycle at a glance.
- The schema also allows `inactive` for entries that should stay tracked but
  should not be treated as active.

## Files you will usually touch

### `config.yml`

This file groups DAO entries by network.

Example:

```yml
base:
  - config: daos/example-dao.yml
    state: draft
    domains:
      - gov.example.org
    features:
      - fulfill
```

Field reference:

| Field | Required | Meaning |
| --- | --- | --- |
| `config` | Yes | Path to a local file under `daos/` or an external HTTP/HTTPS config URL. |
| `state` | No | Entry status. Use `draft` for new onboarding, `active` after sync, and `inactive` for a non-active entry. |
| `tags` | No | Free-form tags such as `demo` or other review-friendly labels. |
| `domains` | No | Domains associated with the DAO site. |
| `features` | No | Optional feature flags for this entry. Current examples use `fulfill`. |

Network keys such as `darwinia`, `ethereum`, `base`, `arbitrum`, `optimism`,
and `lisk` are the top-level groups. Add the DAO under the network where the
governor lives.

### `daos/<dao-code>.yml`

This file contains the detailed DAO configuration consumed by DeGov Square.

Minimal example:

```yml
name: Example DAO
code: example-dao
logo: https://example.org/logo.png
siteUrl: https://gov.example.org
description: Example DAO description.

wallet:
  walletConnectProjectId: example-walletconnect-project-id

chain:
  id: 8453
  name: Base
  rpcs:
    - https://base-rpc.publicnode.com
  explorers:
    - https://basescan.org
  nativeToken:
    symbol: ETH
    decimals: 18

indexer:
  endpoint: https://indexer.example.org/graphql
  startBlock: 123456

contracts:
  governor: "0x0000000000000000000000000000000000000001"
  governorToken:
    address: "0x0000000000000000000000000000000000000002"
    standard: ERC20
```

## DAO config field reference

### Top-level fields

| Field | Required | Meaning |
| --- | --- | --- |
| `name` | Yes | DAO display name shown in the UI. |
| `code` | Yes | Stable unique identifier for the DAO. Use letters, numbers, `_`, or `-`. |
| `logo` | Yes | DAO logo URL. |
| `siteUrl` | Yes | DAO governance site URL. |
| `offChainDiscussionUrl` | No | Forum, GitHub Discussions, or other off-chain discussion URL. |
| `editLink` | No | Link to the DAO config file in GitHub, usually the `blob/main/daos/<file>.yml` URL. |
| `description` | Yes | DAO description shown by the app. |
| `links` | No | Social and external links. |
| `theme` | No | Branding assets such as theme logos and banners. |
| `wallet` | Yes | Wallet connection settings. |
| `chain` | Yes | Network metadata and chain access details. |
| `indexer` | Yes | GraphQL indexer settings. |
| `contracts` | Yes | Governor and governance token contracts. |
| `treasuryAssets` | No | Treasury token/NFT assets tracked for the DAO, or `null`. |
| `safes` | No | Safe multisig wallets associated with the DAO. |
| `apps` | No | Related tools or companion applications. |
| `analysis` | No | Analytics configuration. |

### `links`

| Field | Required | Meaning |
| --- | --- | --- |
| `links.coingecko` | No | CoinGecko project page. |
| `links.website` | No | Main website. |
| `links.twitter` | No | X/Twitter URL. |
| `links.discord` | No | Discord invite or community URL. |
| `links.telegram` | No | Telegram URL. |
| `links.github` | No | GitHub organization or repository URL. |
| `links.email` | No | Public contact email. |

### `theme`

| Field | Required | Meaning |
| --- | --- | --- |
| `theme.logoDark` | No | Logo for dark mode. |
| `theme.logoLight` | No | Logo for light mode. |
| `theme.banner` | No | Desktop banner image. |
| `theme.bannerMobile` | No | Mobile banner image. |
| `theme.faqs` | No | FAQ entries shown on the dashboard. |
| `theme.faqs[].question` | Yes | FAQ question text. |
| `theme.faqs[].answer` | Yes | FAQ answer target. The schema expects a URI, so use a documentation or landing-page link. |

### `wallet`

| Field | Required | Meaning |
| --- | --- | --- |
| `wallet.walletConnectProjectId` | Yes | WalletConnect project ID used by the DAO app. |

### `chain`

| Field | Required | Meaning |
| --- | --- | --- |
| `chain.id` | Yes | Numeric chain ID. |
| `chain.name` | Yes | Human-readable chain name. |
| `chain.logo` | No | Chain logo URL. |
| `chain.rpcs` | Yes | RPC endpoint list. At least one entry is required. |
| `chain.explorers` | Yes | Block explorer URL list. At least one entry is required. |
| `chain.contracts` | No | Optional chain-level contracts. |
| `chain.contracts.multicall3.address` | Yes, if `multicall3` is present | Multicall3 address. |
| `chain.contracts.multicall3.blockCreated` | Yes, if `multicall3` is present | Deployment block for Multicall3. |
| `chain.nativeToken` | Yes | Native token metadata. |
| `chain.nativeToken.symbol` | Yes | Native token symbol. |
| `chain.nativeToken.priceId` | No | CoinGecko price ID for the native token. |
| `chain.nativeToken.decimals` | Yes | Native token decimals. |
| `chain.nativeToken.logo` | No | Native token logo URL. |

### `indexer`

| Field | Required | Meaning |
| --- | --- | --- |
| `indexer.endpoint` | Yes | GraphQL endpoint used by DeGov Square. |
| `indexer.startBlock` | Yes | First block that should be indexed. |
| `indexer.rpc` | No | Optional RPC endpoint used by the indexer. |
| `indexer.gateway` | No | Optional archive gateway URL. |

### `contracts`

| Field | Required | Meaning |
| --- | --- | --- |
| `contracts.governor` | Yes | Governor contract address. |
| `contracts.governorToken.address` | Yes | Governance token contract address. |
| `contracts.governorToken.standard` | Yes | Governance token standard: `ERC20`, `ERC721`, or `ERC1155`. |
| `contracts.timeLock` | No | Timelock contract address when the DAO uses one. |

### `treasuryAssets`

| Field | Required | Meaning |
| --- | --- | --- |
| `treasuryAssets` | No | Array of treasury assets, or `null`. |
| `treasuryAssets[].name` | Yes | Asset name. |
| `treasuryAssets[].contract` | Yes | Asset contract address. |
| `treasuryAssets[].standard` | Yes | Token standard: `ERC20`, `ERC721`, or `ERC1155`. |
| `treasuryAssets[].priceId` | No | CoinGecko price ID for the asset. |
| `treasuryAssets[].logo` | No | Asset logo URL. |

### `safes`

| Field | Required | Meaning |
| --- | --- | --- |
| `safes[].name` | Yes | Safe name or description. |
| `safes[].chainId` | Yes | Chain ID where the Safe is deployed. |
| `safes[].link` | Yes | Safe URL. |

### `apps`

| Field | Required | Meaning |
| --- | --- | --- |
| `apps[].name` | Yes | Application name. |
| `apps[].description` | Yes | What the app does. |
| `apps[].icon` | No | App icon URL. |
| `apps[].link` | Yes | App URL. |
| `apps[].params` | No | App-specific parameters. The schema allows any object shape here. |

### `analysis`

| Field | Required | Meaning |
| --- | --- | --- |
| `analysis.ga.tag` | Yes, if `analysis.ga` is present | Google Analytics 4 measurement ID in `G-XXXXXXXXXX` format. |

## PR checklist

Before opening a PR, make sure you have done all of the following:

1. Added or updated the DAO file under `daos/`.
2. Added or updated the matching entry in `config.yml`.
3. Used `state: draft` for new onboarding work.
4. Checked that every required schema field is present.
5. Reused existing naming and formatting patterns from nearby DAO files.
6. Confirmed every URL and address is final.

## Issue checklist

If you are opening an issue instead of a PR, include at least:

1. DAO name and code.
2. Network name and chain ID.
3. Governance site URL and discussion URL.
4. Governor, governance token, and timelock addresses.
5. RPC, explorer, and indexer information.
6. Branding assets such as logos and banners if available.

Maintainers can use that information to prepare the YAML files for you.
