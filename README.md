## What is Bitd?

Bitd is a scraper for Bitcore that fetches transaction data from the blockchain and stores it in a MongoDB database. You need to install this to set up bitserve and run a bitdb node. As the software matures synchronization speed will get better, but for now expect a day or two from the default checkpoint block depending on your hardware.

### Hardware Requirements
If you intend to run a full un-pruned bitdb node, it is recommended that your server have at the very minimum **4 gigabytes of ram** or more available at all times, and that you have enough storage space to house the whole blockchain three times over. If this is not the case, you may look into changing the [core_from](https://github.com/dalijolijo/bitd-btx/blob/master/.env.example#L15) enviorment variable to a more recent blockheight.

## Installation

### Prerequisite
First you need to do the following:
1. Install Bitcore node software on your server
2. Install the latest versions of NPM and Nodejs on your server
2. Install MongoDB on your server

Before sync, set up your bitcore.conf file to meet the requirements set by bitd. On Ubuntu this can be found in $HOME/.bitcore/bitcore.conf

A sample configuration file can be found below:
```
# location to store blockchain and other data -- if you
# don't know what to do here, just remove the below line
datadir=/data/bitcore
dbcache=4000

# Must set txindex=1 so Bitcore keeps the full index
txindex=1

# [rpc]
# Accept command line and JSON-RPC commands.
server=1
# Choose a strong password here
rpcuser=root
rpcpassword=bitcore

# If you want to allow remote JSON-RPC access
rpcallowip=0.0.0.0/0

# [wallet]
disablewallet=1

# [ZeroMQ]
# ZeroMQ messages power the realtime BitDB crawler
# so it's important to set the endpoint
zmqpubrawtx=tcp://127.0.0.1:28556
zmqpubhashblock=tcp://127.0.0.1:28555

# BitDB makes heavy use of JSON-RPC so it's set to a higher number
rpcworkqueue=512
```

### Setting up Bitd

Clone this repository:
```
git clone https://github.com/dalijoloji/bitd-btx.git && cd bitd-btx
```

Install dependencies:
```
npm install
```

Configure bitd:
```
cp .env.example .env
$(EDITOR) .env
# note, you should only have to change rpc_user and rpc_pass normally
```

Start bitd:
```
npm start --max_old_space_size=4096
```

### Running as a daemon

Install PM2 using NPM
```
npm install pm2 -g
```

CD to install location and run bitd
```
pm2 start index.js --node-args="--max_old_space_size=4096"
```

### Troubleshooting

#### BitD crashing on bigger blocks

If it crashes, it's more than likely node is running out of memory, so try gradually increasing the max old space size.
```
pm2 start index.js --node-args="--max_old_space_size=8192"
```

## Credits

2018 Unwriter

2018 Fountainhead-Cash Developers

2019 LIMXTEX Developers
