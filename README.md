# Cosmos-Validator-Cheatsheet
A list of common CLI commands for managing cosmos chains
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
<chain-process> keys show <name_wallet> --bech cons
```
Query account info
```
<chain-process> q auth account $(<chain-process> keys show <name_wallet> -a) -o text
```
Delete Wallet
```
<chain-process> keys delete <name_wallet>
```
