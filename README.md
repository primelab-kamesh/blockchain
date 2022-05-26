# Blockchain in Go

## Before you start
- Install [Go](https://go.dev/dl) in you machine.
- Install Code Editor [VS CODE](https://code.visualstudio.com/download) or IDE [GO LAND](https://www.jetbrains.com/help/go/installation-guide.html#toolbox) 
- Get Some Understanding of How Blockchain Works
- Below are some youtube video link will give you some context
  - [How does a blockchain work - Simply Explained](https://www.youtube.com/watch?v=SSo_EIwHSd4)
  - [Blockchain 101 - A Visual Demo](https://www.youtube.com/watch?v=_160oMzblY8)
  - [Blockchain 101 - Part 2 - Public / Private Keys and Signing](https://www.youtube.com/watch?v=xIDL_akeras)
- If you are new to Go language follow the [Cheat Sheet](https://github.com/a8m/golang-cheat-sheet/blob/master/golang_refcard.pdf). You will get some understanding of Go syntax.


## Local Setup

### Clone the repository
```bash
$ git clone https://github.com/NearPrime/blockchain.git
```
### Open the project directory
```bash
$ cd blockchain
```
### Install the Go Dependency
```bash
$ go mod init blockchain
```
```bash
$ go get golang.org/x/crypto/ripemd160
```
```bash
$ go get github.com/boltdb/bolt
```
```bash
$ go get github.com/stretchr/testify/assert
```

## Build the executable
```bash
$ go build
```

***Cheers!! You have done with your local setup. You are ready to play with your blockchain.*** 

## Follow below steps to play around your blockchain
1. Set NODE_ID to 3000 `export NODE_ID=3000` in the first terminal window.
2. Open two more terminal and set the `NODE_ID` to 3001 & 3002.
3. Create a wallet and a new blockchain for `NODE 3000`
   1. `blockchain createwallet` it will create a wallet and give you the wallet address.
   2. `blockchain createblockchain -address CENTRAL_NODE` CENTRAL_NODE is address of wallet you just created.
   3. After that, the blockchain will contain single genesis block. We need to save the block and use it in other nodes `cp blockchain_3000.db blockchain_genesis.db `
4. Go to the terminal `NODE 3001`. This will be a wallet node. Generate some addresses with `blockchain createwallet`. Let's call them as `WALLET_1`, `WALLET_2`, `WALLET_3` for shake of understanding.
5. On terminal `NODE 3000`. Let's send some coins to the wallet addresses. We have to have `-mine` flag because initially there are no miner nodes in the network.
   1. `$ blockchain send -from CENTRAL_NODE -to WALLET_1 -amount 10 -mine` 
   2. `$ blockchain send -from CENTRAL_NODE -to WALLET_2 -amount 10 -mine`
   3. Start the node `blockchain startnode`. This node must be running until the end of the scenario.
6. On terminal `NODE 3001`. Start the node’s blockchain with the genesis block saved above `cp blockchain_genesis.db blockchain_3001.db`.
   1. Run the Node `blockchain startnode` It’ll download all the blocks from the central node. To check that everything’s ok, stop the node and check the balances.
   2. `blockchain getbalance -address WALLET_1` it will return `Balance of 'WALLET_1': 10`
   3. `blockchain getbalance -address WALLET_2` it will return `Balance of 'WALLET_2': 10`
   4. `blockchain getbalance -address CENTRAL_NODE` you can check the balance of the CENTRAL_NODE address, because the node 3001 now has its blockchain.
7. On terminal `NODE 3002`. Generate a Wallet `blockchain createwallet`. This will be a miner node.
   1. Initialize the blockchain: `cp blockchain_genesis.db blockchain_3002.db`
   2. Start the node: `blockchain startnode -miner MINER_WALLET`
8. On terminal `NODE 3001`. Send some coins.
   1. `blockchain send -from WALLET_1 -to WALLET_3 -amount 1`
   2. `blockchain send -from WALLET_2 -to WALLET_4 -amount 1`
9. Quickly! Switch to the miner node `NODE 3002` and see it mining a new block! Also, check the output of the central node.
10. On terminal `NODE 3001`. Switch to the wallet node.
    1. Start the wallet node: `blockchain startnode`. It’ll download the newly mined block.
    2. Now stop the node and check the balances using `blockchain getbalance -address WALLET_1`
11. `blockchain listaddresses` You can get all the addresses.
12. `blockchain printchain` It will return the complete chain.

## Scripts
- `blockchain getbalance -address {WALLET_ADDRESS}` It will return the current balance of wallet.
- `blockchain createblockchain -address {CENTRAL_NODE}` CENTRAL_NODE is wallet address. It will create blockchain.
- `blockchain createwallet` it will create a wallet and returns the wallet address.
- `blockchain listaddresses` it will list all wallet address available on blockchain.
- `blockchain printchain` It will return the blockchain.
- `blockchain send -from {WALLET_ADD_1} -to {WALLET_ADD_2} -amount {amount}` It will used to send the coins from one wallet to another.
- `blockchain startnode` It will start the node. and download all the newly mined block.


## Useful tips
* I've followed the below tutorial and the Blockchain Implementation is done as described in the following articles, It's good to follow if you want to get a good understanding of Code Base:*

1. [Basic Prototype](https://jeiwan.net/posts/building-blockchain-in-go-part-1/)
2. [Proof-of-Work](https://jeiwan.net/posts/building-blockchain-in-go-part-2/)
3. [Persistence and CLI](https://jeiwan.net/posts/building-blockchain-in-go-part-3/)
4. [Transactions 1](https://jeiwan.net/posts/building-blockchain-in-go-part-4/)
5. [Addresses](https://jeiwan.net/posts/building-blockchain-in-go-part-5/)
6. [Transactions 2](https://jeiwan.net/posts/building-blockchain-in-go-part-6/)
7. [Network](https://jeiwan.net/posts/building-blockchain-in-go-part-7/)
