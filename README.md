# Docker recipe for [dogecoind](https://github.com/dogecoin/dogecoin)

See the global picture how this container interacts with other components to run Dogeparty:

[Global Component Overview](http://www.inkpad.io/1GMXYwxl4Q)


## Build

    docker build -t dogecoind:v1 .


## Instantiate Data Container

    docker run -d --name=dogecoind-data dogecoind:v1 bash


## Run Container

    docker run -it --name=dogecoind --volumes-from=dogecoind-data dogecoind:v1 bash


## Run Process

    dogecoind


## Debug

    docker run -it --rm --volumes-from=dogecoind-data dogecoind:v1 bash


## Test

    curl --user user:pass --data-binary '{"jsonrpc": "1.0", "id":"0", "method": "getinfo", "params": [] }' -H 'content-type: text/plain;' http://172.17.0.200:22555/
    curl --user user:pass --data-binary '{"jsonrpc": "1.0", "id":"0", "method": "getblockcount", "params": [] }' -H 'content-type: text/plain;' http://172.17.0.200:22555/

    curl -s https://chain.so/api/v2/get_info/DOGE | json data | json blocks

