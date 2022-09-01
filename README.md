# lido-on-terra-cli

---

Warnings:
1. Use at your risk, responsibility for damages (if any) to anyone
resulting from the use of this manual rest entirely with the user. The author
is not responsible for any damage that its use could cause.
2. Because of Terra v2 launch there is still some mess in usage of Luna and
Lunac names, especcially with stLuna and bLuna. More correct usage is of course
stLunac and bLunac but its on chain names are stLuna and bLuna. Since current
manual is focused only on Terra Classic (also called Terra v1), names
Luna/Lunac, stLuna/stLUNAC, bLuna/bLUNAC are interchangable.

---

## Prerequisite

To proceed using this manual, one need to:

1. Install terrad (please follow the official manual
([1])[https://docs.terra.money/docs/develop/terrad/install-terrad.html]
([2])[https://docs.terra.money/docs/develop/terrad/terrad-mac.html])
2. (Congigure one's keys via keychain)
[https://docs.terra.money/docs/develop/terrad/subcommands.html#keys-add]

## How to interact with Lido contracts via CLI

This section covers all the operations that used to be provided by terra.lido.fi
UI widget. If there is any operation that is not covered in the manual, feel
free to create an issue.

The manual doesn't cover any operations besides actual Lido-related things, such
as bonding, unbonding, converting and so on. Other related operations, such as
trading bLuna/stLuna on Astroport and other stuff that is available with
appropriate interfaces.

After any transaction made via CLI you'll receive an ouput with transaction hash
(e.g. `txhash: BDB45045BCA14D2B696E2C7A6D3FCEFB27E61669947E72A198CB66BAE5ED0AFA`
) that you can check via (Finder)[https://finder.terra.money/classic].
Transaction might be not found because of several reasons:

1. Transaction still not processed
2. Transaction still not indexed
3. Transaction failed on client side and wasn't included in block

In cases 1 and 2 you should wait (1 minute should be more than enough). In case
3 please analize the output to figure out possible reasons.

### Know your stLUNAC/bLUNAC balance

Because all the operations are made with ustluna or ubluna (`u` here stands for
`μ` sign wich means micro-stLUNAC and micro-bLUNAC; so, 1000000 of ustluna is 1
stluna and so on) you might be interested into knowing your bLUNAC/stLUNAC
balances in ustluna or ubluna. 

E.g., if your bal

To do so, you can use the following commands:

For stLUNAC

```
terrad query wasm contract-store \
    terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    '{"balance":{"address":"terra1gxlpqnxszu6ltfa69ska8t3xaz3auxfkfayvhw"}}' \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5
```

For bLUNAC

```
terrad query wasm contract-store \
    terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    '{"balance":{"address":"terra1gxlpqnxszu6ltfa69ska8t3xaz3auxfkfayvhw"}}' \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5
```

For LUNC/USTC

```
terrad query bank balances \
    terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5
```

### Bond LUNAC to stLUNAC or bLUNAC

bLuna

```
terrad tx wasm execute \
    terra1mtwph2juhj0rvjz7dy92gvl6xvukaxu8rfv8ts \
    '{"bond":{}}' \
    1uluna \
    --from=terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5 \
    --fees=6000000uusd \
    --gas=30000000 \
    -b block
```

stLuna

```
terrad tx wasm execute \
    terra1mtwph2juhj0rvjz7dy92gvl6xvukaxu8rfv8ts \
    '{"bond_for_stluna":{}}' \
    1uluna \
    --from=terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5 \
    --fees=6000000uusd \
    --gas=30000000 \
    -b block
```

### Unbond stLUNAC or bLUNAC

WARNING:
Unbonding doesn't happen instantly. If you want to convert your bLUNAC/stLUNAC
to LUNAC instantly you might use
(Astroport)[https://classic.astroport.fi/swap?from=terra1yg3j2s986nyp5z7r2lvt0hx3r0lnd7kwvwwtsc&to=uluna]
for it.

To unbond your tokens you need to send them to the Hub and specify which action
you wish to do with them (unbond or convert). To unbond stLUNAC use the
following command:

```
terrad tx wasm execute \
    terra1yg3j2s986nyp5z7r2lvt0hx3r0lnd7kwvwwtsc \
    '{"send": {"amount": "1","contract": "terra1mtwph2juhj0rvjz7dy92gvl6xvukaxu8rfv8ts","msg": "eyJ1bmJvbmQiOnt9fQ=="}}' \
    [] \
    --from=terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5 \
    --fees=6000000uusd \
    --gas=30000000 \
    -b block
```

For bLuna:

```
terrad tx wasm execute \
    terra1kc87mu460fwkqte29rquh4hc20m54fxwtsx7gp \
    '{"send": {"amount": "1","contract": "terra1mtwph2juhj0rvjz7dy92gvl6xvukaxu8rfv8ts","msg": "eyJ1bmJvbmQiOnt9fQ=="}}' \
    [] \
    --from=terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5 \
    --fees=6000000uusd \
    --gas=30000000 \
    -b block
```

### Claim rewards

To claim rewards you can use 

```
terrad tx wasm execute \
    terra17yap3mhph35pcwvhza38c2lkj7gzywzy05h7l0 \
    '{"claim_rewards":{"recipient": "terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla"}}' \
    [] \
    --from=terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5 \
    --fees=6000000uusd \
    --gas=30000000 \
    -b block
```

### Withdraw unbonded LUNAC

Check amount to withdraw

```
terrad ????????????????????????????
```

Actual withdraw

```
terrad tx wasm execute \
    terra17yap3mhph35pcwvhza38c2lkj7gzywzy05h7l0 \
    '{"withdraw_unbonded": {}}' \
    [] \
    --from=terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5 \
    --fees=6000000uusd \
    --gas=30000000 \
    -b block
```

#### Unbond wen???

While doing an unbond you might be interested in when your rewards can be
claimed. To do so, you need to execute several things:

`terrad make awesome tx`

### Convert stLUNAC to bLUNAC or vice-verca

Bla-bla-bla

```
terrad tx wasm execute \
    terra1yg3j2s986nyp5z7r2lvt0hx3r0lnd7kwvwwtsc \
    '{"send": {"amount": "1","contract": "terra1mtwph2juhj0rvjz7dy92gvl6xvukaxu8rfv8ts","msg": ""?????????????=="}}' \
    [] \
    --from=terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5 \
    --fees=6000000uusd \
    --gas=30000000 \
    -b block
```

For bLuna:

```
terrad tx wasm execute \
    terra1kc87mu460fwkqte29rquh4hc20m54fxwtsx7gp \
    '{"send": {"amount": "1","contract": "terra1mtwph2juhj0rvjz7dy92gvl6xvukaxu8rfv8ts","msg": "?????????????=="}}' \
    [] \
    --from=terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5 \
    --fees=6000000uusd \
    --gas=30000000 \
    -b block
```

{
  "convert": {}
}

## Update global index

That operation is needed to autocompaund rewards

```
terrad tx wasm execute \
    terra1kc87mu460fwkqte29rquh4hc20m54fxwtsx7gp \
    '{"update_global_index": {}}' \
    [] \
    --from=terra1ucxftqm3hx6kxq9den984lypuwdq3uknft4kla \
    --node="http://public-node.terra.dev:26657" \
    --chain-id=columbus-5 \
    --fees=6000000uusd \
    --gas=30000000 \
    -b block
```


## Parameters and how to check them

| Name | Value | How to check |
| --- | --- | --- |
| Hub address | terra1mtwph2juhj0rvjz7dy92gvl6xvukaxu8rfv8ts | https://docs.terra.lido.fi/introduction/deployed-contracts#mainnet |
| Public RPC node | http://public-node.terra.dev:26657 | 
| Chain ID | columbus-5
