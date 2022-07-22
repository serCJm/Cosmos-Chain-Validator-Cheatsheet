# Cosmos-Validator-Cheatsheet
A list of common CLI commands for managing cosmos chains
## Server Setup
Check used ports. Note, Cosmos chains commanly use ports 26656 (rpc) and 1317 (rest). Also, make sure chain ports are open
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
