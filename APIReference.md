**Table of Contents**

- [Lisk API](#lisk-api)
  - [Introduction](#introduction)
  - [Accounts](#accounts)
    - [Open account](#open-account)
    - [Get balance](#get-balance)
    - [Get account public key](#get-account-public-key)
    - [Generate public key](#generate-public-key)
    - [Get account](#get-account)
    - [Get delegates](#get-delegates)
    - [Put delegates](#put-delegates)
  - [Loader](#loader)
    - [Get loading status](#get-loading-status)
    - [Get synchronization status](#get-synchronization-status)
    - [Get block receipt status](#get-block-receipt-status)
  - [Transactions](#transactions)
    - [Get list of transactions](#get-list-of-transactions)
    - [Send transaction](#send-transaction)
    - [Get transaction](#get-transaction)
    - [Get unconfirmed transaction](#get-unconfirmed-transaction)
    - [Get list of unconfirmed transactions](#get-list-of-unconfirmed-transactions)
    - [Get list of queued transactions](#get-list-of-queued-transactions)
    - [Get specific queued transaction](get-specific-queued-transaction)
  - [Peers](#peers)
    - [Get peers list](#get-peers-list)
    - [Get peer](#get-peer)
    - [Get peer version, build time](#get-peer-version-build-time)
  - [Blocks](#blocks)
    - [Get block](#get-block)
    - [Get blocks](#get-blocks)
    - [Get blockchain fee](#get-blockchain-fee)
    - [Get blockchain fees schedule](#get-blockchain-fees-schedule)
    - [Get blockchain reward schedule](#get-blockchain-reward-schedule)
    - [Get supply of available Lisk](#get-supply-of-available-lisk)
    - [Get blockchain height](#get-blockchain-height)
    - [Gets status of height, fee, milestone, blockreward and supply](#gets-status-of-height-fee-milestone-blockreward-and-supply)
    - [Get blockchain nethash](#get-blockchain-nethash)
    - [Get blockchain milestone](#get-blockchain-milestone)
  - [Signatures](#signatures)
    - [Get signature fees](#get-signature-fees)
    - [Add second signature](#add-second-signature)
  - [Delegates](#delegates)
    - [Enable delegate on account](#enable-delegate-on-account)
    - [Get delegates list](#get-delegates-list)
    - [Get delegate](#get-delegate)
    - [Search for delegates](#search-for-delegates)    
    - [Get total count of delegates](#get-total-count-of-delegates)
    - [Get votes of account](#get-votes-of-account)
    - [Get voters](#get-voters)
    - [Enable forging on delegate](#enable-forging-on-delegate)
    - [Disable forging on delegate](#disable-forging-on-delegate)
    - [Get forged by account](#get-forged-by-account)
    - [Get next forgers](#get-next-forgers)
  - [Apps](#apps)
    - [Apps](#apps-1)
    - [Get apps](#get-apps)
    - [Get app](#get-app)
    - [Search for apps](#search-for-apps)
    - [Install app](#install-app)
    - [Installed apps](#installed-apps)
    - [Installed apps Ids](#installed-apps-ids)
    - [Uninstall apps](#uninstall-apps)
    - [Launch app](#launch-app)
    - [Installing](#installing)
    - [Uninstalling](#uninstalling)
    - [Launched](#launched)
    - [Categories](#categories)
    - [Stop app](#stop-app)
  - [Multi-Signature](#multi-signature)
    - [Create multi-signature account](#create-multi-signature-account)
    - [Get multi-signature accounts](#get-multi-signature-accounts)
    - [Sign transaction](#sign-transaction)
    - [Get pending multi-signature transactions](#get-pending-multi-signature-transactions)


# Lisk API

## Introduction
Lisk client API. All API endpoints are relative to the `/api` prefix.

All endpoints will return:

- Success parameter. `true` or `false` dependent on success.
- Error parameter. Provided when response is unsuccessful.

The API is only available after the client has successfully loaded, otherwise all endpoints will return:

```text
{
    "success" : false,
    "error" : "loading"
}
```

In the case the client is not fully synced all routes may return intermediate/old values.

Each API entry contains an example call to help provide understanding of how to use the call. These examples rely on `curl` being installed and Lisk running on the localhost on port 8000 (Mainnet). The examples also include `<field>`; use this for easy identification of what needs to be changed for the call to function.

## Accounts
API calls related to Account functionality.

### Open account
Request information about an account.

POST `/api/accounts/open`

**Request**
```text
{
  "secret": "secret key of account"
}
```
**Response**
```text
{
  "success": true,
  "account": {
    "address": "Address of account. String",
    "unconfirmedBalance": "Unconfirmed balance of account. Integer",
    "balance": "Balance of account. Integer",
    "publicKey": "Public key of account. Hex",
    "unconfirmedSignature": "If account enabled second signature, but it's still not confirmed. Boolean: true or false",
    "secondSignature": "If account enabled second signature. Boolean: true or false",
    "secondPublicKey": "Second signature public key. Hex",
    "username": "Username of account."
  }
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X POST -d '{"secret":"<INSERT SECRET HERE>"}' \
http://localhost:8000/api/accounts/open
```

### Get balance
Request the balance of an account.

GET `/api/accounts/getBalance?address=<address>`

- address: wallet address of the account

**Response**
```text
{
  "success": true,
  "balance": "Balance of account",
  "unconfirmedBalance": "Unconfirmed balance of account"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/accounts/getBalance?address=<address>
```

### Get account public key
Get the public key of an account. If the account does not exist the API call will return an error.

GET `/api/accounts/getPublicKey?address=address`

- address: wallet address of the account

**Response**
```text
{
  "success": true,
  "publicKey": "Public key of account. Hex"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/accounts/getPublicKey?address=<address>
```

### Generate public key
Returns the public key of the provided secret key.

POST `/api/accounts/generatePublicKey`

**Request**
```text
{
  "secret": "secret key of account"
}
```
**Response**
```text
{
  "success": true,
  "publicKey": "Public key of account. Hex"
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X POST -d '{"secret":"<INSERT SECRET HERE>"}' \
http://localhost:8000/api/accounts/generatePublicKey
```

### Get account
Returns account information of an address.

GET `/api/accounts?address=address`

- address: wallet address of an account

**Response**
```text
{
  "success": true,
  "account": {
    "address": "Address of account. String",
    "unconfirmedBalance": "Unconfirmed balance of account. Integer",
    "balance": "Balance of account. Integer",
    "publicKey": "Public key of account. Hex",
    "unconfirmedSignature": "If account enabled second signature, but it's still not confirmed. Boolean: true or false",
    "secondSignature": "If account enabled second signature. Boolean: true or false",
    "multisignatures": "Array",
    "u_multisignatures": "Array"
  }
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/accounts?address=<address>
```

### Get delegates
Returns delegate accounts by address.

GET `/api/accounts/delegates?address=address`

- address: wallet address of account

**Response**
```text
{
    "success": true,
    "delegates": [array]
}

```

- Delegates Array includes: delegateId, address, publicKey, vote (# of votes), producedBlocks, missedBlocks, rate, productivity

**Example**
```text
curl -k -X GET http://localhost:8000/api/accounts/delegates?address=<address>
```

### Put delegates
Vote for the selected delegates. Maximum of 33 delegates at once.

PUT `/api/accounts/delegates`

**Request**
```text
{
    "secret" : "Secret key of account",
    "publicKey" : "Public key of sender account, to verify secret passphrase in wallet. Optional, only for UI",
    "secondSecret" : "Secret key from second transaction, required if user uses second signature",
    "delegates" : "Array of string in the following format: ["+DelegatePublicKey"] OR ["-DelegatePublicKey"]. Use + to UPvote, - to DOWNvote"
}
```

**Response**
```text
{
    "success": true,
    "transaction": {object}
}
```

**Example - No Second Secret**
```text
curl -k -H "Content-Type: application/json" \
-X PUT -d '{"secret":"<INSERT SECRET HERE>","publicKey"="<INSERT PUBLICKEY HERE>","delegates":["<INSERT DELEGATE PUBLICKEY HERE>"]}' \
http://localhost:8000/api/accounts/delegates
```

**Example - With Second Secret**
```text
curl -k -H "Content-Type: application/json" \
-X PUT -d '{"secret":"<INSERT SECRET HERE>","publicKey"="<INSERT PUBLICKEY HERE>",secondSecret"="<INSERT SECONDSECRET HERE>,"delegates":["<INSERT DELEGATE PUBLICKEY HERE>"]}' \
http://localhost:8000/api/accounts/delegates
```

**Example - Multiple Votes**
```text
curl -k -H "Content-Type: application/json" \
-X PUT -d '{"secret":"<INSERT SECRET HERE>","publicKey"="<INSERT PUBLICKEY HERE>","delegates":["<INSERT DELEGATE PUBLICKEY HERE>","<INSERT DELEGATE PUBLICKEY HERE>"]}' \
http://localhost:8000/api/accounts/delegates
```

## Loader
Provides the synchronization and loading information of a client. These API calls will only work if the client is syncing or loading.

### Get loading status
Returns the status of the blockchain

GET `/api/loader/status`

**Response**
```text
{
   "success": true,
   "loaded": "Is blockchain loaded? Boolean: true or false",
   "now": "Last block loaded during loading time. Integer",
   "blocksCount": "Total blocks count in blockchain at loading time. Integer"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/loader/status/
```

### Get synchronization status
Get the synchronization status of the client.

GET `/api/loader/status/sync`

**Response**
```text
{
   "success": true,
   "syncing": "Is wallet is syncing with another peers? Boolean: true or false",
   "blocks": "Number of blocks remaining to sync. Integer",
   "height": "Total blocks in blockchain. Integer",
   "broadhash": "Block propagation efficiency and reliability. String",
   "consensus": "Efficiency (%). Integer"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/loader/status/sync
```

### Get block receipt status
Get the status of last received block. Returns true if block was received in the past 120 seconds

GET `/api/loader/status/ping`

**Response**
```text
{
   "success": true
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/loader/status/ping
```

## Transactions
API calls related to transactions.

### Get list of transactions
List of transactions matched by provided parameters.

GET `/api/transactions?blockId=blockId&senderId=senderId&recipientId=recipientId&limit=limit&offset=offset&orderBy=field`

- blockId: Block id of transaction. (String)
- senderId: Sender address of transaction. (String)
- recipientId: Recipient of transaction. (String)
- limit: Limit of transaction to send in response. Default is 20. (Number)
- offset: Offset to load. (Integer number)
- orderBy: Name of column to order. After column name must go "desc" or "acs" to choose order type, prefix for column name is t_. Example: orderBy=t_timestamp:desc (String)

All parameters join by "OR".

Example:  
`/api/transactions?blockId=10910396031294105665&senderId=6881298120989278452L&orderBy=timestamp:desc` looks like: blockId=10910396031294105665 OR senderId=6881298120989278452L

**Response**
```text
{
  "success": true,
  "transactions": [
    "list of transactions objects"
  ]
}
```

**Example - blockId**
```text
curl -k -X GET http://localhost:8000/api/transactions?blockId=<blockId>
```

**Example - senderId**
```text
curl -k -X GET http://localhost:8000/api/transactions?senderId=<senderId>
```

**Example - recipientId**
```text
curl -k -X GET http://localhost:8000/api/transactions?recipientId=<recipientId>
```


### Send transaction
Send transaction to broadcast network.

PUT `/api/transactions`

**Request**
```text
{
    "secret" : "Secret key of account",
    "amount" : /* Amount of transaction * 10^8. Example: to send 1.1234 LISK, use 112340000 as amount */,
    "recipientId" : "Recipient of transaction. Address or username.",
    "publicKey" : "Public key of sender account, to verify secret passphrase in wallet. Optional, only for UI",
    "secondSecret" : "Secret key from second transaction, required if user uses second signature"
}
```

**Response**
```text
{
  "success": true,
  "transactionId": "id of added transaction"
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X PUT -d '{"secret":"<INSERT SECRET HERE>","amount":<INSERT AMOUNT HERE>,"recipientId":"<INSERT WALLET ADDRESS HERE>"}' \
http://localhost:8000/api/transactions
```

**Example - Second Secret**
```text
curl -k -H "Content-Type: application/json" \
-X PUT -d '{"secret":"<INSERT SECRET HERE>","secondSecret":"<INSERT SECOND SECRET HERE>",
"amount":<INSERT AMOUNT HERE>,"recipientId":"<INSERT WALLET ADDRESS HERE>"}' \
http://localhost:8000/api/transactions
```

### Get transaction
Get transaction that matches the provided id.

GET `/api/transactions/get?id=id`

- id: String of transaction (String)

**Response**
```text
{
  "success": true,
  "transaction": {
    "id": "Id of transaction. String",
    "height": "Tx blockchain height. Integer",
    "blockId" "Tx blockId. String",
    "type": "Type of transaction. Integer",
    "timestamp": "Timestamp of transaction. Integer",
    "senderPublicKey": "Sender public key of transaction. Hex",
    "senderId": "Address of transaction sender. String",
    "recipientId": "Recipient id of transaction. String",
    "amount": "Amount. Integer",
    "fee": "Fee. Integer",
    "signature": "Signature. Hex",
    "signatures": "Signatures. Array",
    "confirmations": "Number of confirmations. Integer",
    "asset": "Resources. Object"
  }
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/transactions/get?id=<id>
```

### Get unconfirmed transaction
Get unconfirmed transaction that matches the provided id.

GET `/api/transactions/unconfirmed/get?id=id`

- id: String of transaction (String)

**Response**
```text
{
  "success": true,
  "transaction": {
    "type": "Type of transaction. Integer",
    "amount": "Amount. Integer",
    "senderPublicKey": "Sender public key of transaction. Hex",
    "timestamp": "Timestamp of transaction. Integer",
    "asset": "Resources. Object"
    "recipientId": "Recipient id of transaction. String",
    "signature": "Signature. Hex",
    "id": "Id of transaction. String",
    "fee": "Fee. Integer",
    "senderId": "Address of transaction sender. String",
    "relays": "Propagation. Integer",
    "receivedAt": "Timestamp. String"
  }
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/transactions/unconfirmed/get?id=<id>
```

### Get list of unconfirmed transactions
Gets a list of unconfirmed transactions.

GET `/api/transactions/unconfirmed`

**Response**
```text
{
    "success" : true,
    "transactions" : [list of transaction objects]
}
```
**Example**
```text
curl -k -X GET http://localhost:8000/api/transactions/unconfirmed
```

### Get list of queued transactions
Gets a list of queued transactions.

GET `/api/transactions/queued`

**Response**
```text
{
    "success" : true,
    "transactions" : [list of transaction objects]
}
```
**Example**
```text
curl -k -X GET http://localhost:8000/api/transactions/queued
```

### Get specific queued transaction
Get queued transaction that matches the provided id.

GET `/api/transactions/queued/get?id=id`

- id: String of transaction (String)

**Response**
```text
{
  "success": true,
  "transaction": {
    "id": "Id of transaction. String",
    "type": "Type of transaction. Integer",
    "subtype": "Subtype of transaction. Integer",
    "timestamp": "Timestamp of transaction. Integer",
    "senderPublicKey": "Sender public key of transaction. Hex",
    "senderId": "Address of transaction sender. String",
    "recipientId": "Recipient id of transaction. String",
    "amount": "Amount. Integer",
    "fee": "Fee. Integer",
    "signature": "Signature. Hex",
    "signSignature": "Second signature. Hex",
    "confirmations": "Number of confirmations. Integer"
  }
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/transactions/queued/get?id=<id>
```

## Peers
Peers API.

### Get peers list
Gets list of peers from provided filter parameters.

GET `/api/peers?state=state&os=os&version=version&limit=limit&offset=offset&orderBy=orderBy`

- state: State of peer. 1 - disconnected. 2 - connected. 0 - banned. (String)
- os: OS of peer. (String)
- version: Version of peer. (String)
- limit: Limit to show. Max limit is 100. (Integer)
- offset: Offset to load. (Integer)
- orderBy: Name of column to order. After column name must go "desc" or "acs" to choose order type. (String)

All parameters joins by "OR".

Example:   
`/api/peers?state=1&version=0.3.2` looks like: state=1 OR version=0.3.2

**Response**
```text
{
  "success": true,
  "peers": [
    "list of peers as objects (see below the peer object response)"
  ]
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/peers
```

### Get peer
Gets peer by IP address and port

GET `/api/peers/get?ip=ip&port=port`

- ip: Ip of peer. (String)
- port: Port of peer. (Integer)

**Response**
```text
{
  "success": true,
  "peer": {
        "ip":"requested ip. String",
        "port":"requested port. Integer",
        "state":"1 - disconnected. 2 - connected. 0 - banned. Integer",
        "os":"operating system. String",
        "version":"Lisk client version. String",
        "broadhash":"peer block propagation efficiency and reliability. String",
        "height":"blockchain height. Integer"
  }
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/peers/get?ip=<ip>&port=<port>
```

### Get peer version, build time
Gets a list peer versions and build times

GET `/api/peers/version`

**Response**
```text
{
  "success": true,
  "version": "version of Lisk",
  "build": "time of build"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/peers/version
```

## Blocks
Blocks management API.

### Get block
Gets block by provided id.

GET `/api/blocks/get?id=id`

- id: Id of block.

**Response**
```text
{
    "success": true,
    "block": {
        "id": "Id of block. String",
        "version": "Version of block. Integer",
        "timestamp": "Timestamp of block. Integer",
        "height": "Height of block. Integer",
        "previousBlock": "Previous block id. String",
        "numberOfRequests": "Not using now. Will be removed in 0.2.0",
        "numberOfTransactions": "Number of transactions. Integer",
        "numberOfConfirmations": "Not using now.",
        "totalAmount": "Total amount of block. Integer",
        "totalFee": "Total fee of block. Integer",
        "payloadLength": "Payload length of block. Integer",
        "requestsLength": "Not using now. Will be removed in 0.2.0",
        "confirmationsLength": "Not using now.,
        "payloadHash": "Payload hash. Hex",
        "generatorPublicKey": "Generator public key. Hex",
        "generatorId": "Generator id. String.",
        "generationSignature": "Generation signature. Not using. Will be removed in 0.2.0",
        "blockSignature": "Block signature. Hex"
    }
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/blocks/get?id=<id>
```

### Get blocks
Gets all blocks by provided filter(s).

GET `/api/blocks?generatorPublicKey=generatorPublicKey&height=height&previousBlock=previousBlock&totalAmount=totalAmount&totalFee=totalFee&limit=limit&offset=offset&orderBy=orderBy`

All parameters joins by OR.

Example:  
`/api/blocks?height=100&totalAmount=10000` looks like: height=100 OR totalAmount=10000

- totalFee: total fee of block. (Integer)
- totalAmount: total amount of block. (Integer)
- previousBlock: previous block of need block. (String)
- height: height of block. (Integer)
- generatorPublicKey: generator id of block in hex. (String)
- limit: limit of blocks to add to response. Default to 20. (Integer)
- offset: offset to load blocks. (Integer)
- orderBy: field name to order by. Format: fieldname:orderType. Example: height:desc, timestamp:asc (String)

**Response**
```text
{
  "success": true,
  "blocks": [
    "array of blocks"
  ]
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/blocks?generatorPublicKey=<generatorPublicKey>
```

### Get blockchain fee
Get transaction fee for sending "normal" transactions.

GET `/api/blocks/getFee`

**Response**
```text
{
  "success": true,
  "fee": Integer
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/blocks/getFee
```

### Get blockchain fees schedule
Get transaction fee for all types of transactions.

GET `/api/blocks/getFees`

**Response**
```text
{
  "success": true,
  "fees":{
    "send": Integer,
    "vote": Integer,
    "secondsignature": Integer,
    "delegate": Integer,
    "multisignature": Integer,
    "dapp": Integer
  }
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/blocks/getFees
```

### Get blockchain reward schedule
Gets the forging reward for blocks.

GET `/api/blocks/getReward`

**Response**
```text
{
  "success": true,
  "reward": Integer
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/blocks/getReward
```

### Get supply of available Lisk
Gets the total amount of Lisk in circulation

GET `/api/blocks/getSupply`

**Response**
```text
{
  "success": true,
  "supply": Integer
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/blocks/getSupply
```

### Get blockchain height
Gets the blockchain height of the client.

GET `/api/blocks/getHeight`

**Response**
```text
{
  "success": true,
  "height": "Height of blockchain. Integer"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/blocks/getHeight
```

### Gets status of height, fee, milestone, blockreward and supply
Gets status of height, fee, milestone, blockreward and supply

GET `/api/blocks/getStatus`

**Response**
```text
{
  "success": true,
  "height": Integer
  "fee": Integer
  "milestone": Integer
  "reward": Integer
  "supply": Integer
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/blocks/getStatus
```

### Get blockchain nethash
Gets the nethash of the blockchain on a client.

GET `/api/blocks/getNethash`

**Response**
```text
{
  "success": true,
  "nethash": "Nethash of the Blockchain. String"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/blocks/getNethash
```

### Get blockchain milestone
Gets the milestone of the blockchain on a client.

GET `/api/blocks/getMilestone`

**Response**
```text
{
  "success": true,
  "milestone": Integer
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/blocks/getMilestone
```

## Signatures
Signature management API.

### Get Signature Fees
Gets the second signature status of an account.

GET `/api/signatures/fee`


**Response**
```text
{
    "success" : true,
    "fee" : Integer
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/signatures/fee
```

### Add second signature
Add a second signature to an account.

PUT `/api/signatures`

**Request**
```text
{
  "secret": "secret key of account",
  "secondsecret": "second secret key of account",
  "publicKey": "optional, to verify valid secret key and account"
}
```

**Response**
```text
{
  "success": true,
  "transactionId": "id of transaction with new signature",
  "publicKey": "Public key of signature. hex"
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X PUT -d '{"secret":"<INSERT SECRET HERE>","secondSecret":"<INSERT SECOND SECRET HERE>","publicKey":"<INSERT PUBLIC KEY HERE>" }' \
http://localhost:8000/api/signatures
```

## Delegates
Delegates API.

### Enable delegate on account
Puts request to create a delegate.

PUT `/api/delegates`

**Request**
```text
{
  "secret": "Secret key of account",
  "secondSecret": "Second secret of account",
  "username": "Username of delegate. String from 1 to 20 characters."
}
```
**Response**
```text
{
  "success": true,
  "transaction": "transaction object"
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X PUT -d '{"secret":"<INSERT SECRET HERE>","secondSecret":"<INSERT SECOND SECRET HERE>","username":"<INSERT USERNAME HERE>" }' \
http://localhost:8000/api/delegates
```

### Get delegates list
Gets list of delegates by provided filter.

GET `/api/delegates?limit=limit&offset=offset&orderBy=orderBy`

- limit: Limit to show. Integer. Maximum is 100. (Integer)
- offset: Offset (Integer)
- orderBy: Order by field (String)

**Response**
```text
{
  "success": true,
  "delegates": "delegates objects array"
}
```

- Delegates Array includes: delegateId, address, publicKey, vote (# of votes), producedBlocks, missedBlocks, rate, productivity

**Example**
```text
curl -k -X GET http://localhost:8000/api/delegates?limit=<limit>
```

### Get delegate
Gets delegate by public key or username.

GET `/api/delegates/get?publicKey=publicKey`

- publicKey: Public key of delegate account (String)

GET `/api/delegates/get?username=username`

- username: Username of delegate account (String)

**Response**
```text
{
    "success": true,
    "delegate": {
        "username": "Username. String",
        "address": "Address. String",
        "publicKey": "Public key. String",
        "vote": "Total votes. Integer",
        "producedblocks": "Produced blocks. Integer",
        "missedblocks": "Missed blocks. Integer",
        "rate": "Ranking. Integer",
        "approval": "Approval percentage. Float",
        "productivity": "Productivity percentage. Float"
    }
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/delegates/get?publicKey=publicKey`
```

### Search for delegates
Search for Delegates by "fuzzy" username.

GET `/api/delegates/search?q=username&orderBy=producedblocks:desc`

- q: Search criteria. (String)
- orderBy: Order results by ascending or descending property. Valid sort fields are: `username:asc`, `username:desc`, `address:asc`, `address:desc`, `publicKey:asc`, `publicKey:desc`, `vote:asc`, `vote:desc`, `missedblocks:asc`, `missedblocks:desc`, `producedblocks:asc`, `producedblocks:desc`

**Response**
```text
{
  "success": true,
  "delegates": [
    "array of delegates"
  ]
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/dapps/search?q=username&orderBy=producedblocks:desc
```

### Get delegates count
Get total count of registered delegates.

GET `/api/delegates/count`

**Response**
```js
{
  "success": true,
  "count": 101
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/delegates/count
```

### Get votes of account
Get votes by account wallet address.

GET `/api/accounts/delegates/?address=address`

- address: Address of account. (String)

**Response**
```text
{
  "success": true,
  "delegates": "array of delegates"
}
```

- Delegates Array includes: delegateId, address, publicKey, vote (# of votes), producedBlocks, missedBlocks, rate, productivity

**Example**
```text
curl -k -X GET http://localhost:8000/api/accounts/delegates/?address=<address>
```

### Get voters
Get voters of delegate.

GET `/api/delegates/voters?publicKey=publicKey`

- publicKey: Public key of registered delegate account. (String)

**Response**
```text
{
  "success": true,
  "accounts": [
    "array of accounts who vote for delegate"
  ]
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/delegates/voters?publicKey=<publicKey>
```

### Enable forging on delegate
Enables forging for a delegate on the client node.

POST `/api/delegates/forging/enable`

**Request**
```text
{
  "secret": "secret key of delegate account"
}
```
**Response**
```text
{
  "success": true,
  "address": "address"
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X POST -d '{"secret":"<INSERT SECRET HERE>"}' \
http://localhost:8000/api/delegates/forging/enable
```

### Disable forging on delegate
Disables forging for a delegate on the client node.

POST `/api/delegates/forging/disable`

**Request**
```text
{
  "secret": "secret key of delegate account"
}
```

**Response**
```text
{
  "success": true,
  "address": "address"
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X POST -d '{"secret":"<INSERT SECRET HERE>"}' \
http://localhost:8000/api/delegates/forging/disable
```

### Get forged by account
Get amount of Lisk forged by an account.

GET `/api/delegates/forging/getForgedByAccount?generatorPublicKey=generatorPublicKey`

- generatorPublicKey: generator id of block in hex. (String)

**Response**
```text
{
  "success": true,
  "sum": "Forged amount. Integer"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/delegates/forging/getForgedByAccount?generatorPublicKey=<generatorPublicKey>
```

### Get next forgers
Get amount of Lisk forged by an account.

GET `/api/delegates/getNextForgers?limit=limit`

- limit: limits the amount of delegates returned, default 10, max 101 (Integer)

**Response**
```text
{
  "success": true
  "delegates": [Array of publicKeys. String],
  "currentSlot": "Current slot based on time. Integer"
  "currentBlock": "Current block based on height. Integer"

}

```

**Example**
```text
curl -k -X GET http://localhost:8000/api/delegates/getNextForgers
```

## Apps
Blockchain Applications API.

### Apps
Registers a Blockchain Application.

PUT `/api/dapps`

**Request**
```text
{
  "secret": "Secret of account. String. Required",
  "secondSecret": "Second secret of account. String. Optional",
  "publicKey": "Public key to verify sender secret key. Hex. Optional",
  "category": "DApp category. Integer. Required",
  "name": "DApp name. String. Required",
  "description": "DApp description. String. Optional",
  "tags": "DApp tags. String. Optional",
  "type": "DApp type. Integer. Required (Only type 0 is currently supported)",
  "link": "Link to DApp file. ZIP supported. String. Required",
  "icon": "Link to icon file. PNG and JPG/JPEG supported. String. Optional"
}
```
**Response**
```text
{
  "success": true,
  "transactionId": "transaction id"
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X PUT -d '{"secret":"<INSERT SECRET HERE>","secondSecret":"<INSERT SECOND SECRET HERE>"
"category":<INSERT INTEGER HERE>,"name":"<INSERT APPLICATION NAME HERE>",
"type":0,"link":"<INSERT LINK TO APPLICATION HERE>"}' \
http://localhost:8000/api/dapps
```

### Get apps
Gets a list of Blockchain Applications registered on the network.

GET `/api/dapps?category=category&name=name&type=type&link=link&limit=limit&offset=offset&orderBy=orderBy`

- category: DApp category. (Integer)
- name: DApp name. (String)
- type: DApp type. (Integer)
- link: DApp link. (String)
- limit: Query limit. Maximum is 100. (Integer)
- offset: Query offset. (Integer)
- orderBy: Order by field. (String)

**Response**
```text
{
  "success": true,
  "dapps": "array of dapps"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/dapps?name=<INSERT APPLICATION NAME HERE>
```

### Get app
Gets a specific Blockchain Application by registered id.

GET `/api/dapps/get?id=id`

- id: Id of app

**Response**
```text
{
  "success": true,
  "dapp": "dapp object"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/dapps/get?id=<id>
```

### Search for apps
Searches for Blockchain Applications by filter(s) on a node.

GET `/api/dapps/search?q=q&category=category&installed=installed`

- q: Search criteria. (String)
- category: Category to search within. (Integer)
- installed: Search installed apps only. 1 or 0. (Integer)

**Response**
```text
{
  "success": true,
  "dapps": [
    "array of dapps"
  ]
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/dapps/search?installed=1
```

### Install app
Installs a app by id on the node.

POST `/api/dapps/install`

**Request**
```text
{
  "id": "Blockchain Application id to install"
}
```

**Response**
```text
{
  "success": true,
  "path": "dppp install path"
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X POST -d '{"id":"<INSERT ID HERE>"}' \
http://localhost:8000/api/dapps/install
```

### Installed apps
Returns a list of installed apps on the requested node.

GET `/api/dapps/installed`

**Response**
```text
{
  "success": "true",
  "dapps": [
    "array of dapps"
  ]
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/dapps/installed
```

### Installed apps Ids
Returns a list of installed app ids on the requested node.

GET `/api/dapps/installedIds`

**Response**
```text
{
  "success": true,
  "ids": [
    "array of dapp ids"
  ]
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/dapps/installedIds
```

### Uninstall apps
Uninstalls a app by id from the requested node.

POST `/api/dapps/uninstall`

**Request**
```text
{
  "id": "Blockchain Application id to install"
}
```
**Response**
```text
{
  "success": true
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X POST -d '{"id":"<INSERT ID HERE>"}' \
http://localhost:8000/api/dapps/uninstall
```

### Launch app
Launches a app by id on the requested node.

POST `/api/dapps/launch`

**Request**
```text
{
  "id": "dapp id to launch",
  "params": "dapp launch params, not required, array"
}
```
**Response**
```text
{
  "success": true
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X POST -d '{"id":"<INSERT ID HERE>"}' \
http://localhost:8000/api/dapps/launch
```

### Installing
Returns a list of app ids currently being installed on the requested node.

GET `/api/dapps/installing`

**Response**
```text
{
  "success": true,
  "installing": [
    "array of dapp ids"
  ]
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/dapps/installing
```

### Uninstalling
Returns a list of app ids currently being uninstalled on the client node.

GET `/api/dapps/uninstalling`

**Response**
```text
{
  "success": true,
  "uninstalling": [
    "array of dapp ids"
  ]
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/dapps/uninstalling
```

### Launched
Returns a list of app ids which are currently launched on the client node.

GET `/api/dapps/launched`

**Response**
```text
{
  "success": true,
  "launched": [
    "array of dapp ids"
  ]
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/dapps/launched
```

### Categories
Returns a full list of app categories.

GET `/api/dapps/categories`

**Response**
```text
{
  "success": true,
  "category": "object containing category names and ids"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/dapps/categories
```

### Stop app
Stops a app by id on the requested node.

POST `/api/dapps/stop`

**Request**
```text
{
  "id": "dapp id to stop"
}
```

**Response**
```text
{
  "success": true
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X POST -d '{"id":"<INSERT ID HERE>"}' \
http://localhost:8000/api/dapps/stop
```

## Multi-Signature
Multi-signature API.

### Create multi-signature account
Create a multi-signature account.

PUT `/api/multisignatures`

**Request**
```text
{
    "secret": "your secret. string. required.",
    "lifetime": "request lifetime in hours (1-24). required.",
    "min": "minimum signatures needed to approve a tx or a change (1-15). integer. required",
    "keysgroup": [array of public keys strings]. add '+' before publicKey to add an account or '-' to remove. required.
}
```

**Response**
```text
{
  "success": true,
  "transactionId": "transaction id"
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X PUT -d '{"secret":"<INSERT SECRET HERE>","lifetime":<INSERT NUMBER HERE>,"min":<INSERT NUMBER OF SIGNATURES HERE>,"keysgroup":["+<INSERT PUBLIC KEY HERE>","+<INSERT PUBLIC KEY HERE>"] }' \
http://localhost:8000/api/multisignatures
```

### Get multi-signature accounts
Gets a list of accounts that belong to a multi-signature account.

GET `/api/multisignatures/accounts?publicKey=publicKey`

- publicKey: Public key of multi-signature account (String)

**Response**
```text
{
  "success": true,
  "accounts": "array of accounts"
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/multisignatures/accounts?publicKey=<publicKey>
```

### Sign transaction
Signs a transaction that is awaiting signature.

POST `/api/multisignatures/sign`

**Request**
```text
{
  "secret": "your secret. string. required.",
  "publicKey": "public key of your account. string. optional.",
  "transactionId": "id of transaction to sign. REQUIRED"
}
```
**Response**
```text
{
  "success": true,
  "transactionId": "transaction id"
}
```

**Example**
```text
curl -k -H "Content-Type: application/json" \
-X POST -d '{"secret":"<INSERT SECRET HERE>","transactionId":"<INSERT TRANSACTION ID HERE>"}' \
http://localhost:8000/api/multisignatures/sign
```

### Get pending multi-signature transactions
Returns a list of multi-signature transactions that waiting for signature by publicKey.

GET `/api/multisignatures/pending?publicKey=publicKey`

- publicKey: Public key of account (String)

**Response**
```text
{
    "success": true,
    "transactions": ['array of transactions to sign']
}
```

**Example**
```text
curl -k -X GET http://localhost:8000/api/multisignatures/pending?publicKey=<publicKey>
```
