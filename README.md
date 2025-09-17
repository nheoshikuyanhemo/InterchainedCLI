# InterchainedCLI Help

## Download 
```
git clone https://github.com/nheoshikuyanhemo/interchained
```
```
cd Interchained
```
## Dependencies 
```
sudo apt update && sudo apt install -y software-properties-common
sudo add-apt-repository -y ppa:ubuntu-toolchain-r/test
sudo apt update
sudo apt install -y gcc-13 g++-13 \
  build-essential automake libtool pkg-config git \
  libevent-dev libzmq3-dev libssl-dev \
  libboost-all-dev libdb5.3-dev libdb5.3++-dev \
  libprotobuf-dev protobuf-compiler libqrencode-dev
```
## Export 
```
export CC=gcc-13 CXX=g++-13
export CXXFLAGS="-std=c++14 -O2 -pipe -fstack-protector-strong"
export CFLAGS="-O2 -pipe -fstack-protector-strong"
export LDFLAGS="-Wl,-O1 -Wl,--as-needed"
```
## Aiutogen
```
./autogen.sh
```
## Configuration
```
./configure \
  --enable-wallet \
  --without-gui \
  --disable-bench \
  --disable-tests \
  --with-zmq \
  --with-miniupnpc=no \
  --with-incompatible-bdb
```
## Make file
```
make -j$(nproc)
```
```
cd src
```
## Binaries & Data Directories 

Executables: interchainedd (daemon) and interchained-cli (command-line RPC client).

Default data directories:

Linux:   ```~/.interchained```

## Optional Configuration (interchained.conf)

For persistent settings, create an interchained.conf file in the data directory. Example:
```
./nano ~/.interchainned/interchained.conf
```
```
daemon=1
server=1
# Use cookie auth by default (recommended). If you must set manual RPC creds:
# rpcuser=youruser
# rpcpassword=yourstrongpassword
addnode=seed.interchained.org:17101
# Optional extras:
# txindex=1
# prune=550
```
Note: Local cookie authentication is recommended for security. Avoid exposing RPC beyond localhost unless you know what you’re doing.

## Start the Node (interchainedd)
Linux/macOS (background):
```
./interchainedd -daemon -addnode=seed.interchained.org:17101
```
## Check Sync & Network
```
./interchained-cli -getinfo
```
```
./interchained-cli getblockchaininfo
```
```
./interchained-cli getnetworkinfo
```
```
./interchained-cli getpeerinfo
```

Tip: If you see connection issues, confirm the seed addnode is set and your firewall allows outbound traffic.

# Wallet Management

## Create/Load/Info:

### Create a new wallet named "miner1"
```
./interchained-cli createwallet "miner1"
```
### Load an existing wallet
```
./interchained-cli loadwallet "miner1"
```
### List loaded wallets
```
./interchained-cli listwallets
```
### Get wallet info
```
./interchained-cli -rpcwallet=miner1 getwalletinfo
```
## Look at the list of addresses created 
```
./interchained-cli -rpcwallet="miner1" getaddressesbylabel ""
```
## Addresses & Validation:

### Get a new address (Bech32)
```
./interchained-cli -rpcwallet=miner1 getnewaddress ""
```
### Validate an address (syntax & ownership checks where applicable)
```
./interchained-cli validateaddress itc1qexampleaddress...
```
### Validate wallet integrity (if available in your build)
```
./interchained-cli -rpcwallet=miner1 validatewallet
```

## Security & Backup:

### Encrypt the wallet (requires restart & passphrase for spending)
```
./interchained-cli -rpcwallet=miner1 encryptwallet "your long passphrase"
```
### Unlock for N seconds (for sending)
```
./interchained-cli -rpcwallet=miner1 walletpassphrase "your long passphrase" 120
```
### Lock the wallet again
```
./interchained-cli -rpcwallet=miner1 walletlock
```
### Backup wallet file
```
./interchained-cli -rpcwallet=miner1 backupwallet "/path/to/backup/wallet.backup"
```

##Solo Mining Controls (CPU)
Interchained supports local CPU mining via RPC on some builds. Use these commands if your build includes the classic generate interface.

### Enable mining with N threads
```
./interchained-cli setgenerate true 4 itc1qdestaddress...
```
### Disable mining
```
./interchained-cli setgenerate false
```
### Mining status
```
./interchained-cli getmininginfo
```
Note: For pool mining, use external miners (cpuminer/SRBMiner) and point to pool stratum.

 

## Send Transactions
### Send 1.234 ITC to a Bech32 address from wallet "miner1"
```
./interchained-cli -rpcwallet=miner1 sendtoaddress itc1qdestaddress... 1.234 "payment note" "comment" false
```
### Look up a transaction by txid
```
./interchained-cli -rpcwallet=miner1 gettransaction <txid>
```
### List recent transactions
```
./interchained-cli -rpcwallet=miner1 listtransactions "*" 20 0 true
```
If using an encrypted wallet, unlock it with walletpassphrase before sending.

## cek log 
```
tail -f ~/.interchained/debug.log
```

#### close log 
CTRL + c
 

## Stop the Node
```
./interchained-cli stop
```


## Troubleshooting
interchained-cli connection refused → Ensure interchainedd is running and using the same datadir. If using non-default datadir, pass -datadir to both daemon and CLI.
Stuck sync / no peers → Confirm -addnode=seed.interchained.org:17101 is set; try adding more nodes when announced; verify network/firewall.
RPC auth issues → Prefer cookie auth (default). If using rpcuser/rpcpassword, keep them in interchained.conf and never expose RPC publicly.
Logs → Check debug.log in the data directory for detailed errors.
 
