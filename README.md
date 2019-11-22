# Introduce
DCRM SDK is a distributed key generation and distributed signature module that forms the cornerstone of decentralized value exchange.  This technology was developed for over a year, with the feedback of 4 leading cryptographers: Rosario Gennaro, Dr. Pascal Paillier.

When used in context of blockchain, this module can serve as a non-custodial solution, a keyless wallet, a component to an interoperable solution, and more. Please read the [Wiki](https://github.com/fsn-dev/dcrm-sdk/wiki) for more information.

This SDK allows you to connect to DCRM's network directly in either:
1. a 2+1 configuration where you form a private group with 2 fusion nodes and your own node.
2. a local configuration where you can set any ownership of nodes in your group.

This library contains 2 functions:
1. Distributed key generation which returns the public key (dcrm_genPubkey)
2. Distributed signing of transactions (dcrm_sign)

# Prerequisites
1. Linux terminal
2. Golang

## mode

* [online mode](#online mode)
* [local mode](#local mode)

# Setting Up
## Clone The Repository
To get started, launch your terminal and download the latest version of the SDK.
```
git clone https://github.com/fsn-dev/dcrm-sdk.git
```
## Switch to Directory
```
cd dcrm-sdk
```
## Build
Make sure you are in /dcrm-sdk directory.
```
make
```

## Run
***
### online mode

#### Run node

##### Parameters

`rpcport` - (Optional) HTTP-RPC server listening port (default: 5559).   
`port` - (Optional) Network listening port.  
`nodekey` - (Optional) The key of the node (default:node.key).  

##### Example

./gdcrm (--rpcport 3333  -port 7777 --nodekey "node1.key")

#### JSON RPC API

If you don't define a rpcport when you start node, the default rpcport is 5559.

##### dcrm_genPubkey

Generate dcrm pubkey.

###### Parameters

none

###### Return

`error` - error info.  
`pubkey` - dcrm pubkey.  

###### Example

```
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"dcrm_genPubkey","params":[],"id":67}' http://127.0.0.1:5559
```
// Result
```
{
	"jsonrpc":"2.0",
	"id":67,
	"result":{
		"pubkey":"04bd9f609bb5a2b78f24a8ba102743c8db1b76450dd036cd7997056b997e84ba6a4575b82c71730b032e60223a3bf7e6b479ca5f69a6011bbb92e342a704750ad6"
		}
}
```

##### dcrm_sign

dcrm sign.

###### Parameters

1. `DATA`,pubkey - the pubkey from dcrm_genPubkey request.  
2. `String|HexNumber|TAG` - the hash want to sign.it must be 16-in-32-byte character sprang at the beginning of 0x,for example,0x19b6236d2e7eb3e925d0c6e8850502c1f04822eb9aa67cb92e5004f7017e5e41.  

###### Return

`error` - error info.  
`rsv` - signature str.  

###### Example

// Request
```
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"dcrm_sign","params":["0429c26a073d903d30fce8c24a5355eaa79aea2221f352c5f3ea0263de8a2ae97433d5f12c160d74340434870c139cefcbe98fbb8b6a775e1b0aeda20032861a62","0x19b6236d2e7eb3e925d0c6e8850502c1f04822eb9aa67cb92e5004f7017e5e41"],"id":67}' http://127.0.0.1:5559
```
// Result
```
{
	"jsonrpc":"2.0",
	"id":67,
	"result":{
		"rsv":"3DA1D12F280D4B1608FAAC2295AB75E5F02AF6FEB88BEF31CB40B7B52C7BF039226F438876B25AA9FE8CE08EB57138E70FD7145D600F3A599054C5FE535CA4B201"
		}
}
```
***
### local mode

At least three nodes by default.

#### Generate bootnode.key

./bootnode --genkey ./bootnode.key

#### Run bootnode

##### Parameters

`nodekey` - The key of the bootnode.   
`addr` - The port of the bootnode.   
`group` - The number of the group.

##### Example

// Request
```
./bootnode --nodekey bootnode.key --addr :12340 --group 0  
```
// Rturn  
```
UDP listener up, self enode://ed1c947316187ab1a5d89c14a21e05585facb5eb65229106b586b7aaf0374566048e5d14e803b50729c9d4f1e3b4bc7448f8d6958aab7ff163274bb899ce2b11@[::]:12340
groupNum: 0  
```
#### Run dcrm node

##### Parameters

`bootnodes` - The self enode of the bootnode.  
`rpcport` - HTTP-RPC server listening port .   
`port` - Network listening port.  
`nodekey` - The key of the node.  

##### Run node1

```
./gdcrm --rpcport 9010 --bootnodes "enode://ed1c947316187ab1a5d89c14a21e05585facb5eb65229106b586b7aaf0374566048e5d14e803b50729c9d4f1e3b4bc7448f8d6958aab7ff163274bb899ce2b11@127.0.0.1:12340" --port 12341 --nodekey "node1.key"   
```

##### Run node2

```
./gdcrm --rpcport 9011 --bootnodes enode://ed1c947316187ab1a5d89c14a21e05585facb5eb65229106b586b7aaf0374566048e5d14e803b50729c9d4f1e3b4bc7448f8d6958aab7ff163274bb899ce2b11@127.0.0.1:12340 --port 12342 --nodekey "node2.key"
```

##### Run node3

```
./gdcrm --rpcport 9012 --bootnodes enode://ed1c947316187ab1a5d89c14a21e05585facb5eb65229106b586b7aaf0374566048e5d14e803b50729c9d4f1e3b4bc7448f8d6958aab7ff163274bb899ce2b11@127.0.0.1:12340 --port 12343 --nodekey "node3.key"
```

#### JSON RPC API

##### dcrm_genPubkey

Generate dcrm pubkey.

###### Parameters

none

###### Return

`error` - error info.  
`pubkey` - dcrm pubkey.  

###### Example
```
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"dcrm_genPubkey","params":[],"id":67}' http://127.0.0.1:9012
```
// Result
```
{
	"jsonrpc":"2.0",
	"id":67,
	"result":{
		"pubkey":"0429c26a073d903d30fce8c24a5355eaa79aea2221f352c5f3ea0263de8a2ae97433d5f12c160d74340434870c139cefcbe98fbb8b6a775e1b0aeda20032861a62"
	}
}
```

##### dcrm_sign

dcrm sign.

###### Parameters

1. `DATA`,pubkey - the pubkey from dcrm_genPubkey request.  
2. `String|HexNumber|TAG` - the hash want to sign.it must be 16-in-32-byte character sprang at the beginning of 0x,for example,0x19b6236d2e7eb3e925d0c6e8850502c1f04822eb9aa67cb92e5004f7017e5e41.

###### Return

`error` - error info.  
`rsv` - signature str.  

###### Example

// Request
```
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"dcrm_sign","params":["0429c26a073d903d30fce8c24a5355eaa79aea2221f352c5f3ea0263de8a2ae97433d5f12c160d74340434870c139cefcbe98fbb8b6a775e1b0aeda20032861a62","0x19b6236d2e7eb3e925d0c6e8850502c1f04822eb9aa67cb92e5004f7017e5e41"],"id":67}' http://127.0.0.1:9012
```
// Result
```
{
	"jsonrpc":"2.0",
	"id":67,
	"result":{
		"rsv":"C473948F87A958F3FFD622AFF6596FE2F244CA68D92FE11A77ADFFC9B73A4A8470BC514C2E0028529A8E3EDBBBCD66B368A7B2C3416C5098B3CBA172ED2E57C600"
	}
}
```
