# EVM-based Chains

The source data is in _data/chains. Each chain has its own file with the filename being the [CAIP-2](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) representation as name and `.json` as extension.

## Example

```json
{
  "name": "Ethereum Mainnet",
  "chain": "ETH",
  "rpc": [
    "https://mainnet.infura.io/v3/${INFURA_API_KEY}",
    "https://api.mycryptoapi.com/eth"
  ],
  "faucets": [],
  "nativeCurrency": {
    "name": "Ether",
    "symbol": "ETH",
    "decimals": 18
  },
  "features": [{ "name": "EIP155" }, { "name": "EIP1559" }],
  "infoURL": "https://ethereum.org",
  "shortName": "eth",
  "chainId": 1,
  "networkId": 1,
  "icon": "ethereum",
  "explorers": [{
    "name": "etherscan",
    "url": "https://etherscan.io",
    "icon": "etherscan",
    "standard": "EIP3091"
  }]
}
```

when an icon is used in either the network or an explorer there must be a json in _data/icons with the name used (e.g. in the above example there must be a `ethereum.json` and a `etherscan.json` in there) - the icon jsons look like this:

```json

[
    {
      "url": "ipfs://QmdwQDr6vmBtXmK2TmknkEuZNoaDqTasFdZdu3DRw8b2wt",
      "width": 1000,
      "height": 1628,
      "format": "png"
    }
]

```

where:
 * the URL must be an IPFS url that is publicly resolvable
 * width and height are positive integers
 * format is either "png", "jpg" or "svg"

If the chain is an L2 or a shard of another chain you can link it to the parent chain like this:


```json
{
  ...
  "parent": {
   "type" : "L2",
   "chain": "eip155-1",
   "bridges": [ {"url":"https://bridge.arbitrum.io"} ]
  }
}
```

where you need to specify type 2 and the reference to an existing parent. The field about bridges is optional.

You can add a `status` field e.g. to deprecate (via status `deprecated`) a chain (a chain should never be deleted as this would open the door to replay attacks)
Other options for `status` are `active` (default) or `incubating`

## Aggregation

There are also aggregated json files with all chains automatically assembled:
 * https://chainid.network/chains.json
 * https://chainid.network/chains_mini.json (miniaturized - fewer fields for smaller filesize)

## Constraints

 * the shortName and name MUST be unique - see e.g. EIP-3770 on why
 * if referencing a parent chain - the chain MUST exist in the repo
 * if using a IPFS CID for the icon - the CID MUST be retrievable via `ipfs get` - not only through some gateway (means please do not use pinata for now)
 * for more constraints you can look into the CI

## Collision management

 We cannot allow more than one chain with the same chainID - this would open the door to replay attacks.
 The first pull request gets the chainID assigned. When creating a chain we can expect that you read EIP155 which states this repo.
 All pull requests trying to replace a chainID because they think their chain is better than the other will be closed.
 The only way to get a chain reassigned is when the old chain gets deprecated. This can e.g. be used for testnets that are short-lived. But then you will get the redFlag "reusedChaiID" that should be displayed in clients to warn them about the dangers here.

## PR verification

Before submitting a PR, please verify that checks pass with:

```bash
$ ./gradlew run

BUILD SUCCESSFUL in 7s
9 actionable tasks: 9 executed
```

## Usages
### Wallets
 * [WallETH](https://walleth.org)
 * [TREZOR](https://trezor.io)
 * [Minerva Wallet](https://minerva.digital)

### Explorers
 * [Otterscan](https://otterscan.io)

### EIPs
 * EIP-155
 * EIP-3014
 * EIP-3770
 * EIP-4527

### Listing sites
 * [chainid.network](https://chainid.network) / [chainlist.wtf](https://chainlist.wtf)
 * [chainlist.org](https://chainlist.org)
 * [networks.vercel.app](https://networks.vercel.app)
 * [eth-chains](https://github.com/taylorjdawson/eth-chains)
 * [EVM-BOX](https://github.com/izayl/evm-box)
 * [chaindirectory.xyz](https://www.chaindirectory.xyz)
 * [chain-list.org](https://chain-list.org)
 * [chainlist.network](https://chainlist.network/)
 * [evmchainlist.org](https://evmchainlist.org)
 * [chainlist.info](https://chainlist.info)
 * [chainmap.io](https://chainmap.io)
 * [chainz.me](https://chainz.me)
 * [Chainlink docs](https://docs.chain.link/)
 * [Wagmi compatible chain configurations](https://spenhouet.com/chains)
 * [evmchain.info](https://evmchain.info)

### Other
 * [FaucETH](https://github.com/komputing/FaucETH)
 * [Sourcify playground](https://playground.sourcify.dev)


 * Your project - contact us to add it here!
