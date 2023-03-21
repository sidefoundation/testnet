# Installation

## Build and Start blockchains

- Prepare two servers, (4core, 8G, 100G disk) 
- Download the codes on the servers
```sh
git clone https://github.com/sidefoundation/sidechain.git
cd sidechain && make install
sudo cp ~/go/bin/sidechaind /usr/local/bin
```

- Start the node #1
```sh
ignite c serve -r 
```
- Start the node #2
```sh
ignite -c ./config2.yml c serve -r 
```

# Relayer

- Install go relayer:
```sh
$ git clone https://github.com/cosmos/relayer.git
$ cd relayer && git checkout v2.1.0
$ make install
```
for more details: https://github.com/cosmos/relayer

- Create chain config `side1.json`:
```json
{
  "type": "cosmos",
  "value": {
    "key": "default",
    "chain-id": "sidechain_7070-1",
    "rpc-addr": "https://rpc.testnet.side.one:443",
    "account-prefix": "side",
    "keyring-backend": "test",
    "gas-adjustment": 1.2,
    "gas-prices": "0.01aside",
    "debug": true,
    "timeout": "20s",
    "output-format": "json",
    "sign-mode": "direct"
  }
}
```

- Create chain config `side2.json`:
```json
{
  "type": "cosmos",
  "value": {
    "key": "default",
    "chain-id": "sidechain_7070-2",
    "rpc-addr": "http://rpc2.testnet.side.one:443",
    "account-prefix": "side",
    "keyring-backend": "test",
    "gas-adjustment": 1.2,
    "gas-prices": "0.01aside",
    "debug": true,
    "timeout": "20s",
    "output-format": "json",
    "sign-mode": "direct"
  }
}

```

- Add chains
```sh 
rly chains add --file side1.json side-1
rly chains add --file side1.json side-2
```

- New Paths
```sh
rly paths new sidechain_7070-1 sidechain_7070-2 side
rly tx link side
rly tx channel side --src-port swap --dst-port swap --order unordered --version ics100-1
```

- Start rely
```sh
rly start side
```

# Test
- Make swap on node #1:
```sh
 sidechaind tx ibc-swap make channel-1 100aside side14d92qgn6dtygycaehu6dqu2uu7t8ge36te00rf 200bside --from alice
 ```
- Take swap on node #2
```sh
sidechaind tx ibc-swap take 9ADB7BE23AD24C1C28364B8FBB265F9C148ED25AC0EB7556B9A610DAA20A1B71 200bside side14d92qgn6dtygycaehu6dqu2uu7t8ge36te00rf --from david --packet-timeout-height 1-1000 --packet-timeout-timestamp 0
```

- Query orders
```sh
sidechaind q ibc-swap orders
```









