# EOS Dev Resources
- `https://github.com/EOSIO/eos`
- `https://developers.eos.io`
- `https://developers.eos.io/eosio-nodeos/docs/testnets`
- `https://developers.eos.io/eosio-nodeos/docs/overview-1`
- `https://docs.google.com/presentation/d/1c7OOej5-QpdAgPcKsdDkM_3csxt4VZik24ArEsrB33o/edit#slide=id.g4251dc7d21_0_16`
- EOS is Easy
	- `https://www.youtube.com/watch?v=0mmOQNhFR_c`
- EOS: What Can You Build with One of the Fastest Blockchains Ever
	- `https://www.youtube.com/watch?v=gFZ2s7dHSik`
- Automating EOS with Ansible
	- `https://www.youtube.com/watch?v=tWZLjWx7C60`
- EOS - Live Coding of A Smart Contract
	- `https://www.youtube.com/watch?v=4OmQ7Ow9baI`
	- `https://www.youtube.com/watch?v=0kawSbMQ1v8`
- EOS Ignite Channel
	- https://www.youtube.com/channel/UCBfWngSZxm9A15fFMwUS_Ag
	- https://www.crowdcast.io/e/eos-ignite-conference/register?session=1

- https://medium.com/coinmonks/the-future-of-eos-tokens-c98bdbadbc36


# Install / Run the node
- Core Concepts
	- nodeos (node + eos = nodeos) - the core EOSIO node daemon that can be configured with plugins to run a node. Example uses are block production, dedicated API endpoints, and local development.
	- cleos (cli + eos = cleos) - command line interface to interact with the blockchain and to manage wallets
	- keosd (key + eos = keosd) - component that securely stores EOSIO keys in wallets. 
- Run
	- Install docker and start the daemon
		- `sudo systemctl start docker`
		- Windows: https://medium.com/@TeaSea1/how-to-install-eos-on-windows-ac1b6c7d8369
	- `sudo docker pull eosio/eos-dev`
	- `sudo docker network create eosdev`
	- Boot Containers
	```shell
	sudo docker run --name nodeos -d -p 8888:8888 --network eosdev \
	-v /tmp/eosio/work:/work -v /tmp/eosio/data:/mnt/dev/data \
	-v /tmp/eosio/config:/mnt/dev/config eosio/eos-dev  \
	/bin/bash -c "nodeos -e -p eosio --plugin eosio::producer_plugin \
	--plugin eosio::history_plugin --plugin eosio::chain_api_plugin \
	--plugin eosio::history_api_plugin \
	--plugin eosio::http_plugin -d /mnt/dev/data \
	--config-dir /mnt/dev/config \
	--http-server-address=0.0.0.0:8888 \
	--access-control-allow-origin=* --contracts-console --http-validate-host=false"
	```
	- Run Keosd (Wallet and Keystore)
	```shell
	sudo docker run -d --name keosd --network=eosdev \
	-i eosio/eos-dev /bin/bash -c "keosd --http-server-address=0.0.0.0:9876"
	```
	- Check your installation
	```shell
	sudo docker logs -f nodeos
	```
	- Check the Wallet
		- `sudo docker exec -it keosd bash`
		- `cleos --wallet-url http://127.0.0.1:9876 wallet list keys`
	- Create a wallet
		- `sudo docker exec -it keosd bash`
		- `cleos --wallet-url http://127.0.0.1:9876 wallet create --to-console` => "PW5JQczx9RdmC5ny9Vt7QW8nr2kFqCzNLPZ6YqrTd6Kj9RVfUGm7x"
		- `cleos --wallet-url http://127.0.0.1:9876 wallet unlock --password PW5JQczx9RdmC5ny9Vt7QW8nr2kFqCzNLPZ6YqrTd6Kj9RVfUGm7x`
	- Check Nodeos endpoints
		- `http://localhost:8888/v1/chain/get_info`
	- Find `keosd` IP
		- `sudo docker network inspect eosdev`

# Create Hello World Smart Contract
	- `sudo docker exec -it keosd bash`
	- CREATE: `eosiocpp -n HelloWorld`
	- `cd HelloWorld`
	- COMPILE:
		- `eosiocpp -o HelloWorld.wasd HelloWorld.cpp`
		- `eosiocpp -g HelloWorld.abi HelloWorld.hpp`
	- DEPLOY:
		- `cd ..`
		- `cleos --wallet-url http://127.0.0.1:9876 set contract myaccount HelloWorld`


# Code
- https://github.com/eostoolkit/eostoolkit - eostoolkit.io
- https://github.com/eoscanada
- https://github.com/MonsterEOS/monstereos - Tamagotchi app
- https://github.com/alohaeos
- https://github.com/generEOS/eostokeninfo
- https://github.com/EOSIO/eosio-project-boilerplate-simple


# Orgs
- 100x
- EOSYS
- EOS CANNON
- EOSDAQ
- EOS NATION
- Parls
- EOS GO
- MEET.ONE
- GenerEOS
- EOS Tribe
- https://eosauthority.com
- HKEOS - https://www.hkeos.com


# EOS Rush Through Concepts
- 500ms Block Intervals
	- Ethereum is 30,000ms
	- Bitcoin is ... 10min - aka forever
- 1 Block is TX
	- Transaction ID
	- etc
- Free TXes