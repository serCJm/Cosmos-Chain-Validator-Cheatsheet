# Cosmos-Validator-Cheatsheet
A list of common CLI commands for managing cosmos chains

## General
<ul>
  <li>There are two types of unit denomination in Cosmos chains. Standard unit (i.e. 1 ATOM) and micro unit, the smallets unit (i.e. 1 uATOM). 1 standard unit = 1,000,000 micro units.</li>  
</ul>  

## Server Setup
Check used ports. Note, Cosmos chains commanly use ports 26657 (rpc) and 1317 (rest). Also, make sure chain ports are open
```
sudo ss -tulwn | grep LISTEN
sudo ufw status verbose
```
Install GO
```
ver="1.18.1" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

## Installation and Init
Download and install binaries
```
git clone <chain-repo-link>
cd <chain-folder-name>
git checkout <proper-branch>
make install

<chain-process> version --long | head
```
Init
```
<chain-process> init <moniker> --chain-id <proper-chain>

# set default chain-id
<chain-process> config chain-id <proper-chain>
```
Create or recover a wallet
```
# add --recover flag to recover a wallet
<chain-process> keys add <wallet-name>
```
For genesis validators only
```
<chain-process> add-genesis-account <wallet-name> 10000000<micro-unit>

<chain-process gentx <wallet-name> 10000000<micro-unit> \
--chain-id <proper-chain> \
--commission-rate=0.05 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.05 \
--pubkey $(<chain-process> tendermint show-validator) \
--moniker <"moniker">

cat ~/.<chain>/config/gentx/gentx-*
```

Download Genesis
```
wget -O $HOME/.<chain>/config/genesis.json "<genesis-link>"
```
Check validator state
```
cd && cat .<chain>/data/priv_validator_state.json
#{
#  "height": "0",
#  "round": 0,
#  "step": 0
#}

# otherwise
<chain-process> tendermint unsafe-reset-all --home $HOME/.<chain>
```
Download addrbook
```
wget -O $HOME/.<chain>/config/addrbook.json "<addrbook-link"
```
Create Service File
```
sudo tee /etc/systemd/system/<chain-process>.service > /dev/null <<EOF
[Unit]
Description=seid
After=network-online.target

[Service]
User=$USER
ExecStart=$(which <chain>) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload && \
systemctl enable <chain-process> && \
systemctl restart <chain-process> && journalctl -u <chain-process> -f -o cat
```

## Key Management
Display keys
```
<chain-process> keys list
```
Display account key
```
<chain-process> keys show <wallet-name> --bech acc
```
Display validator key
```
<chain-process> keys show <wallet-name> --bech val
```
Display consensus key
```
<chain-process> keys show <wallet-name> --bech cons
```
Query account info
```
<chain-process> q auth account $(<chain-process> keys show <wallet-name> -a) -o text
```
Delete Wallet
```
<chain-process> keys delete <wallet-name>
```

## Voting
See proposals
```
<chain-process> q gov proposals
```
See specific proposal
```
<chain-process> q gov proposal <proposal-id>
```
See voting results by address
```
<chain-process> q gov proposals --voter <ADDRESS>
```
Vote for a proposal
```
<chain-process> tx gov vote 1 yes --from <name_wallet> --fees 5550<micro-fee>
```
Make a deposit on a proposal
```
<chain-process> tx gov deposit 1 1000000<micro-amount> --from <name_wallet> --fees 5550<micro-fee>
```
