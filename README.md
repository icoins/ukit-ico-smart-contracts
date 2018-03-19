# [![uKit ICO](./assets/logo.svg "uKit ICO")](https://ico.ukit.com)

[![License](https://img.shields.io/badge/License-Apache%202.0-66007A.svg)](https://opensource.org/licenses/Apache-2.0)
[![node](https://img.shields.io/badge/node-v9.4.0-50EA3B.svg)](https://nodejs.org/en/docs/)
[![yarn](https://img.shields.io/badge/yarn-v1.3.2-2281BA.svg)](https://yarnpkg.com/lang/en/docs/install/)
[![npm](https://img.shields.io/badge/npm-v5.6.0-DB0031.svg)](https://www.npmjs.com/)
[![truffle](https://img.shields.io/badge/truffle-v4.0.5-00F1C6.svg)](http://truffleframework.com/docs/getting_started/installation)
[![ganache-cli](https://img.shields.io/badge/ganache--cli-v6.0.3-EAAB5E.svg)](https://github.com/trufflesuite/ganache-cli)
[![solidity](https://img.shields.io/badge/solidity-docs-000000.svg)](http://solidity.readthedocs.io/en/develop/introduction-to-smart-contracts.html)

## Project description
uKit AI 2.0 is a system that creates personalized landing pages for visitors.

It is based on:

- ***Artificial intelligence***
Deciding what kind of website a person will most likely enjoy.

- ***Generative design***
Rebuilding a website on the fly.

- ***Blockchain database***
Giving traffic contributors an opportunity to earn by educating the system.

## Repo description

This repository is based on the [Truffle](http://truffleframework.com/docs/getting_started/installation) framework, which is using for compilation, testing and deployment smart contracts.

[Ganache](https://github.com/trufflesuite/ganache-cli) is used for testing of smart contracts in a private blockchain.

[Soljitsu](https://github.com/BlockChainCompany/soljitsu) is used to combine all imported smart contracts to a single file.

### Directory structure description

```
ukit-ico-smart-contracts
└─── bin
└─── build
|	 └─── combined
|	 └─── contracts
└─── config
|	 └─── deploy
|	 └─── networks
└─── contracts
└─── migrations
└─── src
|	 └─── shared
└─── test
└─── utils
```

- **./bin/** -- executable command files
- **./build/combined/** -- combined smart contracts files
- **./build/contract/** -- smart contracts artifacts
- **./config/deploy/** -- deployment configuration files (the name of each file must match the network name)
- **./config/networks/** -- network configuration files (the name of each file must match the network name)
- **./contracts/** -- combined smart contracts files
- **./migrations/** -- migration files
- **./src/** -- smart contracts sources
- **./test/** -- testing files
- **./utils/** -- utility files

## Installation

We recommend using the latest versions **node**, **npm** и **yarn**.

To install the dependencies, you must run the following command `yarn install`.

## Configuring

### Networks

By default, Truffle is configured to use the *development* network:

```javascript
{
	host       : '127.0.0.1',
	port       : 8545,
	network_id : '*',
	gas        : 2 * 10**6,
	gasPrice   : 20 * 10**9
}
```

Each network is configured by default as *development*.

To override any values, you must create a file ***./config/networks/development.js*** that exports the overridden directives, for example:

```javascript
module.exports = {
	host     : 'localhost',
	port     : 8646,
	gasPrice : 10 * 10**9
}
```

Transactions in the *development* network are signed by the first unlocked Ganache account. To specify a different account, you must specify the directive `from: <unlocked_address>`.

To add a new network, you must create a file named as the network, for example, ***./config/networks/live.js***:

```javascript
module.exports = {
	host       : '8.8.8.8',
	port       : 8747,
	network_id : '*',
	gas        : 2 * 10**6,
	gasPrice   : 10 * 10**9
}
```

### Deployment

By default, project comes with deployment configuration for *development* network -- ***./config/deploy/development.js***.

| Property                                       | Required | Type       | Description
| ---------------------------------------------- | :------: | :--------: | -----------
| token                                          | &middot; | `Object`   |
| &nbsp;&nbsp;&nbsp;name                         | &middot; | `String`   | Full name of the token
| &nbsp;&nbsp;&nbsp;symbol                       | &middot; | `String`   | Token short name (ticker)
| &nbsp;&nbsp;&nbsp;totalSupply                  | &middot; | `Number`   | Total supply amount; *can not be different than summ of all allocation amounts*; *should be an integer*
| &nbsp;&nbsp;&nbsp;decimals                     | &middot; | `Number`   | Number of digits after decimal separator; *should be an integer between 0 and 18*
| controller                                     | &middot; | `Object`   |
| &nbsp;&nbsp;&nbsp;finalizeType                 | &middot; | `String`   | Controller finalization type; *may be `burn` or `transfer`*
| &nbsp;&nbsp;&nbsp;finalizeTransferAddressType  |          | `String`   | Name of the one of the addresses from allocations object; *required for `transfer` finalizeType*; *can not be the name (`ico`) of the ICO controller address*
| allocations                                    | &middot; | `[Object]` |
| &nbsp;&nbsp;&nbsp;name                         | &middot; | `String`   | Name of the allocation address; *name `ico` is the reserved name for controller address*
| &nbsp;&nbsp;&nbsp;amount                       | &middot; | `Number`   | Amount of allocation; *should be an integer*
| &nbsp;&nbsp;&nbsp;address                      |          | `String`   | String representation of the Ethereum address; *not suitable for the allocation with name `ico`*
| &nbsp;&nbsp;&nbsp;lock                         |          | `Boolean`  | Flag for allocation locking; *all locked allocations will be unlocked after ICO finalization*
| &nbsp;&nbsp;&nbsp;timelock                     |          | `Number`   | Date till allocation will be locked; *timestamp (in seconds) of the particular unlocking date*; *should be an integer*

Example configuration may look like this:

```javascript
{
	token : {
		name        : 'Token Name',
		symbol      : 'TOKEN',
		totalSupply : 10**6,
		decimals    : 18
	},
	controller : {
		finalizeType                : 'burn',
		finalizeTransferAddressType : ''
	},
	allocations : [
		{
			name   : 'ico',
			amount : 3 * 10**6
		},
		{
			name     : 'timelocked',
			address  : '0x0000000000000000000000000000000000000000',
			amount   : 10**6,
			timelock : Math.ceil(Date.now() / 1000) + 60 * 60 * 24 * 548 // +1.5 years
		},
		{
			name    : 'locked',
			address : '0x0000000000000000000000000000000000000000',
			amount  : 10**6,
			lock    : true
		},
		{
			name    : 'transfer',
			address : '0x0000000000000000000000000000000000000000',
			amount  : 10**6
		}
	]
}
```

## Deploying

Before starting the deployment of smart contracts, you must have running Ethereum with enabled JSON-RPC option. The network configuration file should contain information about the connection to this node.

To deploy on the *development* network, it is sufficient to execute the following commands:

1. `yarn ganache` (in a separate tab) -- run Ganache
2. `yarn migrate` -- combine the files of smart contracts, compile and deploy it

To deploy on a network other than *development*, you must run the following command:

```bash
NETWORK=<network_name> yarn migrate
```

## Testing

Tests run by executing the command `yarn test` (and passes only on the network *development*). Tests of these smart contracts are both unit and integration.

The command above executes the compilation `./bin/compile.sh` (preliminary combining the files of smart contracts using `./bin/combine.sh`), then -- for each test file, the private blockchain is launched with `./bin/ganache.sh`.

Each test uses instances of smart contracts that are deployed once at the beginning of the file execution.

Please note that in the ***./test/*** directory only test files (which are executed sequentially, according to the naming convention) should be located.

## Command reference

* `yarn combine` -- combining of smart contract sources to the ***./build/combined/*** directory and moving the combined files (specified in the `COMBINED_FILES` variable from ***./bin/combine.sh*** -- *UKTToken.combined.sol* and *UKTTokenController.combined.sol*) to the ***./contracts/*** directory

* `[NETWORK=<network_name>] yarn compile` -- compilation of smart contracts from the ***./contracts/*** directory and subsequent deletion of files *./contracts/\*.combined.sol*

* `[NETWORK=<network_name>] yarn migrate` -- combining, compilation and deployment of smart contracts in the specified network

* `[PORT=<port_number>] yarn ganache` -- starting private blockchain (accounts are read from file ***./utils/constants.json***)

* `yarn test` -- start testing in the *development* network

## Contracts on Etherscan

Token address: [0x0000000000000000000000000000000000000000](https://etherscan.io/token/0x0000000000000000000000000000000000000000#readContract)

Token controller address: [0x0000000000000000000000000000000000000000](https://etherscan.io/token/0x0000000000000000000000000000000000000000#readContract)
