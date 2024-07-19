![image](https://github.com/user-attachments/assets/3ca72387-514b-48d4-9bea-1917cbaa241c)


Penulis: [Naufal](https://x.com/0xfal)

> [!NOTE]
> **WHAT IS Fuel?**\
> ...

# Tutorial Fuel Final Testnet Node

## Dependencies

### Install Fuel toolchain:
```
curl https://install.fuel.network | sh
```
## Run Node

### Getting a Sepolia (Ethereum Testnet) API Key
An API key from any RPC provider that supports the Sepolia network will work. Relayers will help listen to events from the Ethereum network. We recommend either [Infura](https://www.infura.io/) or [Alchemy](https://www.alchemy.com/).

The endpoints should look like the following:

**Infura**
```
https://sepolia.infura.io/v3/{YOUR_API_KEY}
```
**Alchemy**
```
https://eth-sepolia.g.alchemy.com/v2/{YOUR_API_KEY}
```

### Generating a P2P Key

Do not share or lose this private key! Press any key to complete.
```
fuel-core-keygen new --key-type peering
```

### Chain Configuration

```
mkdir .fueld
git clone https://github.com/FuelLabs/chain-configuration chain-configuration && cp -r $HOME/chain-configuration/ignition/* $HOME/.fueld/
```

### Running a Local Node

Change `<YOUR_KEYPAIR_SECRET>` and `<YOUR_ETHEREUM_SEPOLIA_RPC>` to yours.
```
sudo tee /etc/systemd/system/fueld.service > /dev/null << EOF
[Unit]
Description=Fuel final testnet node guide by ZuperHunt
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which fuel-core run) \
--service-name=zuperfuel \
--keypair <YOUR_KEYPAIR_SECRET> \
--relayer <YOUR_ETHEREUM_SEPOLIA_RPC> \
--ip=0.0.0.0 --port=4000 --peering-port=30333 \
--db-path=$HOME/.fueld \
--snapshot $HOME/.fueld \
--utxo-validation --poa-instant false --enable-p2p \
--reserved-nodes /dns4/p2p-testnet.fuel.network/tcp/30333/p2p/16Uiu2HAmDxoChB7AheKNvCVpD4PHJwuDGn8rifMBEHmEynGHvHrf \
--sync-header-batch-size 100 \
--enable-relayer \
--relayer-v2-listening-contracts=0x01855B78C1f8868DE70e84507ec735983bf262dA \
--relayer-da-deploy-height=5827607 \
--relayer-log-page-size=500 \
--sync-block-stream-buffer-size 30

Restart=on-failure
RestartSec=5s
StartLimitBurst=3
StartLimitIntervalSec=10s
TimeoutStartSec=60s
TimeoutStopSec=60s
LimitNOFILE=65535
WatchdogSec=3600s
ExecStartPre=/bin/sleep 10

[Install]
WantedBy=multi-user.target 
EOF
```

The output should be like this:
![image](https://github.com/user-attachments/assets/9bc8426b-a643-4bf0-8aae-c95bb6764423)


# Help

If you have any questions, find us on:\
[ZuperHunt's Discord server](https://discord.gg/ZuperHunt)\
[ZuperHunt's X(Twitter)](https://twitter.com/ZuperHunt)

# Acknowledgements

* [Running a local Fuel node connected to Testnet using P2P](https://docs.fuel.network/guides/running-a-node/running-a-testnet-node/)
* [Fuel Node Setup Guide](https://docs.nodecattel.xyz/node-guides/quick-start/fuel-node-setup-guide)
