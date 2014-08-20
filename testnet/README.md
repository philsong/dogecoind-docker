## Build

    docker build -t dogecoind:1.8-dev .


## Instantiate Data Container

    docker run -d --name=dogecoind-testnet-data dogecoind:1.8-dev bash


## Run Container

    docker run -it --name=dogecoind-testnet --volumes-from=dogecoind-testnet-data dogecoind:1.8-dev bash


## Run Process

    dogecoind -addnode=54.237.28.96 -addnode=107.170.14.48 -addnode=54.74.34.153 -addnode=178.201.149.20 -addnode=54.217.8.3 -disablesafemode


## Debug

    docker run -it --rm --volumes-from=dogecoind-testnet-data dogecoind:1.8-dev bash


## Test

    curl --user user:pass --data-binary '{"jsonrpc": "1.0", "id":"0", "method": "getinfo", "params": [] }' -H 'content-type: text/plain;' http://172.17.0.200:44555/
    curl --user user:pass --data-binary '{"jsonrpc": "1.0", "id":"0", "method": "getblockcount", "params": [] }' -H 'content-type: text/plain;' http://172.17.0.200:44555/

    curl -s https://chain.so/api/v2/get_info/DOGETEST | json data | json blocks

