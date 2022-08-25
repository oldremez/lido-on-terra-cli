# lido-on-terra-cli

---

Warnings:
1. Use at your risk, responsibility for damages (if any) to anyone
resulting from the use of this manual rest entirely with the user. The author
is not responsible for any damage that its use could cause.
2. Because of Terra v2 launch there is still some mess in usage of Luna and Lunac names, especcially with stLuna and bLuna. More correct usage is of course stLunac and bLunac but its on chain names are stLuna and bLuna. Since current manual is focused only on Terra Classic (also called Terra v1), names Luna/Lunac, stLuna/stLUNAC, bLuna/bLUNAC are interchangable.

---

## Prerequisite

To proceed using this manual, one need to:

1. Install terrad
2. Congigure one's keys via keychain

## How to interact with Lido contracts via CLI

This section covers all the operations that used to be provided by terra.lido.fi UI widget. If there is any operation that is not covered in the manual, feel free to create an issue.

The manual doesn't cover any operations besides actual Lido-related things, such as bonding, unbonding, converting and so on. Other related operations, such as trading bLuna/stLuna on Astroport and other stuff that is available with appropriate interfaces.

### Know your stLUNAC/bLUNAC balance

Because all the operations are made with ustluna or ubluna (`u` here stands for `μ` sign wich means micro-stLUNAC and micro-bLUNAC; so, 1000000 of ustluna is 1 stluna and so on) you might be interested into knowing your bLUNAC/stLUNAC balances in ustluna or ubluna. This commands are also good for checking if your configuration works well.

To do so, you can use the following commands:

For stLUNAC

```
terrad query wasm contract-store \
    terra1yg3j2s986nyp5z7r2lvt0hx3r0lnd7kwvwwtsc \
    '{"balance":{"address":"terra1gxlpqnxszu6ltfa69ska8t3xaz3auxfkfayvhw"}}' \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5
```

For bLUNAC

```
terrad query wasm contract-store \
    terra1kc87mu460fwkqte29rquh4hc20m54fxwtsx7gp \
    '{"balance":{"address":"terra1gxlpqnxszu6ltfa69ska8t3xaz3auxfkfayvhw"}}' \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5
```

### Bond LUNAC to stLUNAC or bLUNAC

bLuna

`terrad make awesome tx`

{
  "bond": {}
}

stLuna

{
  "bond_for_stluna": {}
}

### Unbond stLUNAC or bLUNAC

Bla-bla-bla

{
  "unbond": {}
}

`terrad make awesome tx`

### Claim rewards

Bla-bla-bla

`terrad make awesome tx`

{
  "claim_rewards": {
    "recipient": "terra1..." 
  }
}
terra17yap3mhph35pcwvhza38c2lkj7gzywzy05h7l0

### Withdraw unbonded LUNAC

Check amount of withdraw

```
terrad tx wasm execute \
    terra1mtwph2juhj0rvjz7dy92gvl6xvukaxu8rfv8ts \
    '{"withdraw_unbonded": {}}' \
    []
```

Bla-bla-bla

`terrad make awesome tx`

#### Unbond wen???

While doing an unbond you might be interested in when your rewards can be claimed. To do so, you need to execute several things:

`terrad make awesome tx`

### Convert stLUNAC to bLUNAC or vice-verca

Bla-bla-bla

`terrad make awesome tx`

{
  "convert": {}
}

## Update global index

That operation is needed to autocompaund rewards

{
  "update_global_index": {
  }
}

## Parameters and how to check them

| Name | Value | How to check |
| --- | --- | --- |
| Hub address | terra1mtwph2juhj0rvjz7dy92gvl6xvukaxu8rfv8ts | https://docs.terra.lido.fi/introduction/deployed-contracts#mainnet |
| Public RPC node | http://public-node.terra.dev:26657 | 
| Chain ID | columbus-5
