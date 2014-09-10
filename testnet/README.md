# Docker recipe for [dogecoind](https://github.com/dogecoin/dogecoin)

See the global picture how this container interacts with other components to run Dogeparty:

[Global Component Overview](http://www.inkpad.io/1GMXYwxl4Q)

## Build

Pre-requisite: [Docker installation](https://docs.docker.com/).

Installed Docker? Great. Now build this docker project according to the recipe found at ``your/path/to/dogecoind-docker/testnet/Dockerfile``.
	
```
cd /your/path/to/dogecoind-docker/testnet
docker build -t dogecoind:testnet .
```
Wait for ``Successfully built [your image_id presented here]`` (may take 10+ mins. on first run).
	
Then issue command ```docker images``` to find a result similar to this:
	
| REPOSITORY |   TAG   |  IMAGE ID    |    CREATED     | VIRTUAL SIZE |
| ---------- | ------- | ------------ | -------------- | ------------ |
| dogecoind  | testnet | ba3f4163b790 | 30 minutes ago | 1.791 GB |
| ubuntu     | 14.04   | 826544226fdc | 4 days ago     | 194.2 MB |
	
What you have now is a Docker _image_ identified by _ba3f4163b790_ (think your id here), stored in a _repository_ called _dogecoind_ and _tagged_ as _testnet_.

## Instantiate Data Container

In order to separate dogecoind _execution_ from its _data_, we will now create a separate Docker data volume _container_ for use by the dogecoin software. This makes it possible for future builds of the dogecoin software to reuse the data, instead of having to wait for the long-running process of re-reading the data off the dogecoin peers. 

``docker run -d --name=dogecoind-testnet-data dogecoind:testnet bash``


## Run Container

First time: Create, Launch & Attach to a docker _container_ named _dogecoind-testnet_, using the image _dogecoind:testnet_ and the data container volume _dogecoind-testnet-data:

``docker run -it --name=dogecoind-testnet --volumes-from=dogecoind-testnet-data dogecoind:testnet bash``
    
OR
    
Consecutive times: Launch & Attach to the docker container:
	
``
docker start dogecoind-testnet
docker attach dogecoind-testnet
``

### Detaching from container

In order to keep the container running while leaving the container shell:
	
``ctrl-p ctrl-q``
	
and back again:
	
``docker attach dogecoind-testnet``
	
### Stopping the container

From the host shell:
	
``docker stop dogecoind-testnet``
	
From the container shell:
	
``exit``   

## Run Process

Run the dogecoind daemon from within the container shell:

``dogecoind -addnode=54.237.28.96 -addnode=107.170.14.48 -addnode=54.74.34.153 -addnode=178.201.149.20 -addnode=54.217.8.3 -disablesafemode &``

Then check the daemon status by means of its Command-Line Interface:
	
``dogecoin-cli getinfo``

## Debug 

[I Dunno how this is used. Or why. I do get that it's a nameless container that is being removed when it exits, but cannot see how it relates to a basic usage scenario or how its more usable for debugging.  /Peter ]
``docker run -it --rm --volumes-from=dogecoind-testnet-data dogecoind:testnet bash``


## Test

To test how other docker containers access _dogecoind-testnet_, we first need to get the IP address of it:
	
``docker inspect dogecoind-testnet | grep IPAddress``
	
Then edit the _curl_ requests below and run them from the host shell ( or from ``boot2docker ssh`` if running on MacOsX, see [how-to-use-docker-on-os-x-the-missing-guide](http://viget.com/extend/how-to-use-docker-on-os-x-the-missing-guide))


	curl --user user:pass --data-binary '{"jsonrpc": "1.0", "id":"0", "method": "getinfo", "params": [] }' -H 'content-type: text/plain;' http://[insert IP here]:44555/

	curl --user user:pass --data-binary '{"jsonrpc": "1.0", "id":"0", "method": "getblockcount", "params": [] }' -H 'content-type: text/plain;' http://[Insert IP here]:44555/


[What's this doing here, and why? /Peter]

	curl -s https://chain.so/api/v2/get_info/DOGETEST | json data | json blocks

