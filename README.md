# InterchainedCLI

## Download 
```
git clone https://github.com/nheoshikuyanhemo/interchained
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
## Binaries & Data Directories 

Executables: interchainedd (daemon) and interchained-cli (command-line RPC client).

Default data directories:

Linux:   ```~/.interchained```

## Optional Configuration (interchained.conf)

For persistent settings, create an interchained.conf file in the data directory. Example:
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
##affic
