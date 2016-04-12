# Lisk API
## INTRODUCTION
Lisk Wallet REST API. All API calls are using /api prefix.
### How to work with the API
- Information need to return.
- Success parameter. Determines the success of a response.
- Error parameter. Provided if the success parameter equal `false`.
 
The API is only available after the client is completely loaded (!=synced), else all routes return:
```
{
    "success" : false,
    "error" : "loading"
}
```
In the case the client is not fully synced all routes return intermediate/old values. 

## Accounts
Account related API calls.

### Open account
Get information about an account.

POST `/api/accounts/open`

**Request**
```
{
  "secret": "secret key of account"
}
```
**Response**
```
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

### Get balance
Get the balance of an account.

GET `/api/accounts/getBalance?address=address`

- address: Address of the account

**Response**
```
{
  "success": true,
  "balance": "Balance of account",
  "unconfirmedBalance": "Unconfirmed balance of account"
}
```

### Get account public key
Get the public key of an account. If the account does not exist the API call will return an error.

GET `/api/accounts/getPublicKey?address=address`

- address: Address of account

**Response**
```
{
  "success": true,
  "publicKey": "Public key of account. Hex"
}
```

### Generate public key
Returns the public key of the provided secret key.

POST `/api/accounts/generatePublicKey`

**Request**
```
{
  "secret": "secret key of account"
}
```
**Response**
```
{
  "success": true,
  "publicKey": "Public key of account. Hex"
}
```

### Get account
Return account information of an address.

GET `/api/accounts?address=address`

- address: Address of account

**Response**
```
{
  "success": true,
  "account": {
    "address": "Address of account. String",
    "unconfirmedBalance": "Unconfirmed balance of account. Integer",
    "balance": "Balance of account. Integer",
    "publicKey": "Public key of account. Hex",
    "unconfirmedSignature": "If account enabled second signature, but it's still not confirmed. Boolean: true or false",
    "secondSignature": "If account enabled second signature. Boolean: true or false",
    "secondPublicKey": "Second signature public key. Hex"
  }
}
```

### Get delegates
Returns delegate accounts by address.

GET `/api/accounts/delegates?address=address`

- address: Address of account

**Response**
```
{
    "success": true,
    "delegates": [array]
}
```

- Delegates Array includes: delegateId, address, publicKey, vote (# of votes), producedBlocks, missedBlocks, rate, productivity

### Put delegates
Vote for the selected delegates. Maximum of 33 delegates at once.

PUT `/api/accounts/delegates`

**Request**
```
{
    "secret" : "Secret key of account",
    "publicKey" : "Public key of sender account, to verify secret passphrase in wallet. Optional, only for UI",
    "secondSecret" : "Secret key from second transaction, required if user uses second signature",
    "delegates" : "Array of string in the following format: ["+DelegatePublicKey"] OR ["-DelegatePublicKey"]. Use + to UPvote, - to DOWNvote"
}
```
**Response**
```
{
    "success": true,
    "transaction": {object}
}
```

## Loader
Provides the synchronisation and loading information of a client. These API calls are only working if the client is syncing or loading.

### Get loading status
Returns account's delegates by address.

GET `/api/loader/status`

**Response**
```
{
   "success": true,
   "loaded": "Is blockchain loaded? Boolean: true or false",
   "now": "Last block loaded during loading time. Integer",
   "blocksCount": "Total blocks count in blockchain at loading time. Integer"
}
```

### Get synchronisation status
Get the synchronisation status of the client.

GET `/api/loader/status/sync`

**Response**
```
{
   "success": true,
   "sync": "Is wallet is syncing with another peers? Boolean: true or false",
   "blocks": "Number of blocks remaining to sync. Integer",
   "height": "Total blocks in blockchain. Integer"
}
```

## Transactions
API calls related to transactions.

### Get list of transactions
Transactions list matched by provided parameters.

GET `/api/transactions?blockId=blockId&senderId=senderId&recipientId=recipientId&limit=limit&offset=offset&orderBy=field`

- blockId: Block id of transaction. (String)
- senderId: Sender address of transaction. (String)
- recipientId: Recipient of transaction. (String)
- limit: Limit of transaction to send in response. Default is 20. (Number)
- offset: Offset to load. (Integer number)
- orderBy: Name of column to order. After column name must go "desc" or "acs" to choose order type, prefix for column name is t_. Example: orderBy=t_timestamp:desc (String)

All parameters joins by "OR". 

Example:  
`/api/transactions?blockId=10910396031294105665&senderId=6881298120989278452C&orderBy=timestamp:desc` looks like: blockId=10910396031294105665 OR senderId=6881298120989278452C

**Response**
```
{
  "success": true,
  "transactions": [
    "list of transactions objects"
  ]
}
```

### Send transaction
Send transaction to broadcast network.

PUT `/api/transactions`

**Request**
```
{
    "secret" : "Secret key of account",
    "amount" : /* Amount of transaction * 10^8. Example: to send 1.1234 LISK, use 112340000 as amount */,
    "recipientId" : "Recipient of transaction. Address or username.",
    "publicKey" : "Public key of sender account, to verify secret passphrase in wallet. Optional, only for UI",
    "secondSecret" : "Secret key from second transaction, required if user uses second signature"
}
```
**Response**
```
{
  "success": true,
  "transactionId": "id of added transaction"
}
```

### Get transaction
Transaction matched by id.

GET `/api/transactions/get?id=id`

- id: String of transaction (String)

**Response**
```
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
    "companyGeneratorPublicKey": "If transaction was sent to merchant, provided comapny generator public key to find company. Hex",
    "confirmations": "Number of confirmations. Integer"
  }
}
```

### Get unconfirmed transaction
Get unconfirmed transaction by id.

GET `/api/transactions/unconfirmed/get?id=id`

- id: String of transaction (String)

**Response**
```
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

### Get list of unconfirmed transactions
Get list of unconfirmed transactions.

GET `/api/transactions/unconfirmed`

**Response**
```
{
    "success" : true,
    "transactions" : [list of transaction objects]
}
```

## Peers
Peers API.

### Get peers list
Get peers list by parameters.

GET `/api/peers?state=state&os=os&shared=shared&version=version&limit=limit&offset=offset&orderBy=orderBy`

- state: State of peer. 1 - disconnected. 2 - connected. 0 - banned. (String)
- os: OS of peer. (String)
- shared: Is peer shared? Boolean: true or false. (String)
- version: Version of peer. (String)
- limit: Limit to show. Max limit is 100. (Integer)
- offset: Offset to load. (Integer)
- orderBy: Name of column to order. After column name must go "desc" or "acs" to choose order type. (String)

All parameters joins by "OR". 

Example:   
`/api/peers?state=1&version=0.1.8` looks like: state=1 OR version=0.1.8

**Response**
```
{
  "success": true,
  "peers": [
    "list of peers"
  ]
}
```

### Get peer
Get peer by ip and port

GET `/api/peers/get?ip=ip&port=port`

- ip: Ip of peer. (String)
- port: Port of peer. (Integer)

**Response**
```
{
  "success": true,
  "peer": "peer object"
}
```

### Get peer version, build time
Get peer version and build time

GET `/api/peers/version`

**Response**
```
{
  "success": true,
  "version": "version of Crypti",
  "build": "time of build"
}
```

## Blocks
Blocks manage API.

### Get block
Get block by id.

GET `/api/blocks/get?id=id`

- id: Id of block.

**Response**
```
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

### Get blocks
Get all blocks.

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
```
{
  "success": true,
  "blocks": [
    "array of blocks"
  ]
}
```

### Get blockchain fee percent

GET `/api/blocks/getFee`

**Response**
```
{
  "success": true,
  "fee": "fee percent"
}
```

### Get blockchain height
Get blockchain height.

GET `/api/blocks/getHeight`

**Response**
```
{
  "success": true,
  "height": "Height of blockchain. Integer"
}
```

### Get forged by account
Get amount forged by account.

GET `/api/delegates/forging/getForgedByAccount?generatorPublicKey=generatorPublicKey`

- generatorPublicKey: generator id of block in hex. (String)

**Response**
```
{
  "success": true,
  "sum": "Forged amount. Integer"
}
```

## Signatures
Blocks manage API.

### Get signature
Get second signature of account.

GET `/api/signatures/get?id=id`

- id: Id of signature. (String)

**Response**
```
{
    "success" : true,
    "signature" : {
        "id" : "Id. String",
        "timestamp" : "TimeStamp. Integer",
        "publicKey" : "Public key of signature. hex",
        "generatorPublicKey" : "Public Key of Generator. hex",
        "signature" : [array],
        "generationSignature" : "Generation Signature"
    }
}
```

### Add second signature
Add second signature to account.

PUT `/api/signatures`

**Request**
```
{
  "secret": "secret key of account",
  "secondsecret": "second key of account",
  "publicKey": "optional, to verify valid secret key and account"
}
```
**Response**
```
{
  "success": true,
  "transactionId": "id of transaction with new signature",
  "publicKey": "Public key of signature. hex"
}
```

## Delegates
Delegates API.

### Enable delegate on account
Calls for delegates functional.

PUT `/api/delegates`

**Request**
```
{
  "secret": "Secret key of account",
  "secondSecret": "Second secret of account",
  "username": "Username of delegate. String from 1 to 20 characters."
}
```
**Response**
```
{
  "success": true,
  "transaction": "transaction object"
}
```

### Get delegates
Get delegates list.

GET `/api/delegates?limit=limit&offset=offset&orderBy=orderBy`

- limit: Limit to show. Integer. Maximum is 100. (Integer)
- offset: Offset (Integer)
- orderBy: Order by field (String)

**Response**
```
{
  "success": true,
  "delegates": "delegates objects array"
}
```

- Delegates Array includes: delegateId, address, publicKey, vote (# of votes), producedBlocks, missedBlocks, rate, productivity

### Get delegate
Get delegate by transaction id.

GET `/api/delegates/get?id=transactionId`

- transactionId: Id of transaction where delegated was putted. (String)

**Response**
```
{
    "success": true,
    "delegate": {
        "username": "username of delegate",
        "transactionId": "transaction id",
        "votes": "amount of stake voted for this delegate"
}
```

### Get votes of account
Get votes by account address.

GET `/api/accounts/delegates/?address=address`

- address: Address of account. (String)

**Response**
```
{
  "success": true,
  "delegates": "array of delegates"
}
```

- Delegates Array includes: delegateId, address, publicKey, vote (# of votes), producedBlocks, missedBlocks, rate, productivity

### Get voters
Get voters of delegate.

GET `/api/delegates/voters?publicKey=publicKey`

- publicKey: Public key of delegate. (String)

**Response**
```
{
  "success": true,
  "accounts": [
    "array of accounts who vote for delegate"
  ]
}
```

### Enable forging on delegate
Enable forging

POST `/api/delegates/forging/enable`

**Request**
```
{
  "secret": "secret key of delegate account"
}
```
**Response**
```
{
  "success": true,
  "address": "address"
}
```

### Disable forging on delegate
Disable forging

POST `/api/delegates/forging/disable`

**Request**
```
{
  "secret": "secret key of delegate account"
}
```
**Response**
```
{
  "success": true,
  "address": "address"
}
```

## Messages
Messages API.

### Send message
Send message to recipient.

PUT `/api/messages`

**Request**
```
{
    "recipientId" : "id of recipient. String. Optional.",
    "encrypt" : "encrypt message or not encrypt. Boolean. Default to false. If equal true need to provide recipient public key.",
    "recipientPublicKey" : "Public key of recipient account. Required if encrypt == true. String."
    "message" : "Message to send. From 1 to 140 characters. Required",
    "secret" : "Secret passphrase of your account. String. Required",
    "secondSecret": "Second public key if using. String. ",
    "publicKey": "Your public key to verify sender. String, Hex. Optional"
}
```
**Response**
```
{
  "success": true,
  "transactionId": "Id of transaction."
}
```

## Usernames
Usernames API

### Register username
Register username.

PUT `/api/accounts/username`

**Request**
```
{
  "secret": "Secret passphrase of your account. String. Required",
  "secondSecret": "Second public key if using. String. ",
  "publicKey": "Your public key to verify sender. String, Hex. Optional",
  "username": "Username of delegate. String from 1 to 20 characters."
}
```
**Response**
```
{
  "success": true,
  "transactionId": "Id of transaction."
}
```

## Contacts
Usernames API

### Add contact
Add contact

PUT `/api/contacts`

**Request**
```
{
  "secret": "Secret passphrase of your account. String. Required",
  "secondSecret": "Second public key if using. String. ",
  "publicKey": "Your public key to verify sender. String, Hex. Optional",
  "following": "Address or username of account to follow. String. Required"
}
```
**Response**
```
{
  "success": true,
  "transactionId": "Id of transaction."
}
```

### Get contacts
Get contacts of account by public key.

GET `/api/contacts/?publicKey=publicKey`

- publicKey: Public key of account. (String)

**Response**
```
{
  "success": true,
  "following": "array of you following accounts",
  "followers": "array of your followers"
}
```

### Get unconfirmed contacts
Get unconfirmed contacts of account by public key.

GET `/api/contacts/unconfirmed?publicKey=publicKey`

- publicKey: Public key of account. (String)

**Response**
```
{
  "success": true,
  "contacts": [
    "array of contacts transactions"
  ]
}
```

## Dapps
Dapp API.

### Dapps
Register dapp.

PUT `/api/dapps`

**Request**
```
{
  "secret": "Secret of account. String. Required",
  "secondSecret": "Second secret of account. String. Optional",
  "publicKey": "Public key to verify sender secret key. Hex. Optional",
  "category": "Category of DApp. Integer. Required",
  "name": "Name of DApp. String. Required",
  "description": "Description of DApp. String. Optional",
  "tags": "Tags of DApp. String. Optional",
  "type": "Type of DApp, now supported only 0 type. Integer. Required",
  "siaAscii": "ASCII code of sia shared file. String. Optional",
  "git": "Link to git repository. String. Optional",
  "icon": "Link to icon file. PNG and JPG/JPEG supported. String. Optional",
  "siaIcon": "ASCII code of sia shared icon file. String. Optional"
}
```
**Response**
```
{
  "success": true,
  "transactionId": "id of transaction"
}
```

### Get dapps
Get specifc dapps.

GET `/api/dapps?category=category&name=name&type=type&git=git&siaAscii=siaAscii&siaIcon=siaIcon&icon=icon&limit=limit&offset=offset&orderBy=orderBy`

- category: Category of DApp. (Integer)
- name: Name of DApp. (String)
- type: Type of DApp. (Integer)
- git: Git repository link to DApp. (String)
- limit: Limit of dapps in query. Maximum is 100. (Integer)
- offset: Offset of dapps in query. (Integer)
- orderBy: Order by field. (String)

**Response**
```
{
  "success": true,
  "dapps": "Array of dapps"
}
```

### Get dapp
Get a specifc dapp.

GET `/api/dapps/get?id=id`

- id: Id of dapp

**Response**
```
{
  "success": true,
  "dapp": "dapp"
}
```

### Search dapp store
Get specifc dapps.

GET `/api/dapps/search?q=q&category=category&installed=installed`

- q: Query to search. (String)
- category: Category to search. (Integer)
- installed: Search only in installed dapps. 1 or 0. (Integer)

**Response**
```
{
  "success": true,
  "dapps": [
    "array of dapps"
  ]
}
```

### Install dapp
Will install dapp on your node.

POST `/api/dapps/install`

**Request**
```
{
  "id": "id of dapp"
}
```
**Response**
```
{
  "success": true,
  "path": "path of installed dapp"
}
```

### Installed dapps
Return list of installed dapps.

GET `/api/dapps/installed`

**Response**
```
{
  "success": "true",
  "dapps": [
    "array of dapps"
  ]
}
```

### Installed dapps Ids
Return list of installed dapps ids.

GET `/api/dapps/installedIds`

**Response**
```
{
  "success": true,
  "ids": [
    "ids of dapps"
  ]
}
```

### Uninstall dapps
Will uninstall dapp from your node.

POST `/api/dapps/uninstall`

**Request**
```
{
  "id": "id of dapp"
}
```
**Response**
```
{
  "success": true
}
```

### Launch dapp
It will launch dapp on your node.

POST `/api/dapps/launch`

**Request**
```
{
  "id": "id of dapp to launch",
  "params": "params to launch dapp, not required, array"
}
```
**Response**
```
{
  "success": true
}
```

### Installing
Will return dapps that in installing right now.

GET `/api/dapps/installing`

**Response**
```
{
  "success": true,
  "installing": [
    "ids of dapps that installing"
  ]
}
```

### Removing
Will return dapps that in uninstalling right now.

GET `/api/dapps/removing`

**Response**
```
{
  "success": true,
  "removing": [
    "ids of dapps that removing"
  ]
}
```

### Launched
Will return launched dapps.

GET `/api/dapps/launched`

**Response**
```
{
  "success": true,
  "launched": [
    "list of launched dapps ids"
  ]
}
```

### Categories
Will return categories of dapps.

GET `/api/dapps/categories`

**Response**
```
{
  "success": true,
  "category": "object with names and ids of dapps categories"
}
```

### Stop dapp
Will stop dapp on your node.

POST `/api/dapps/stop`

**Request**
```
{
  "id": "id of dapp to stop it"
}
```
**Response**
```
{
  "success": true
}
```

### Icon dapp
Using to get icon of dapp from sia.

GET `/api/dapps/icon`

- id: Id of dapp. (String)

**Response**
```
Not specified.
```

## Multi-Signature
Multisignature group API.

### Get pending multi-signature transactions
Return multisig transaction that waiting for your signature.

GET `/api/multisignatures/pending?publicKey=publicKey`

- publicKey: Public key of account (String)

**Response**
```
{
    "success": true,
    "transactions": ['array of transactions to sign']
}
```

### Create multi-signature account
Create a multisignature account.

PUT `/api/multisignatures`

**Request**
```
{
    "secret": "your secret. string. required.",
    "lifetime": "request lifetime in hours (1-24). required.",
    "min": "minimum signatures needed to approve a tx or a change (1-15). integer. required",
    "keysgroup": [array of public keys strings]. add '+' before publicKey to add an account or '-' to remove. required. 
}
```
**Response**
```
{
  "success": true,
  "transactionId": "transaction id"
}
```

### Sign transaction
Sign transaction that wait for your signature.

POST `/api/multisignatures/sign`

**Request**
```
{
  "secret": "your secret. string. required.",
  "publicKey": "public key of your account. string. optional.",
  "transactionId": "id of transaction to sign"
}
```
**Response**
```
{
  "success": true,
  "transactionId": "transaction id"
}
```

### Get accounts of multisignature
Get accounts of multisignature.

GET `/api/multisignatures/accounts?publicKey=publicKey`

- publicKey: Public key of multi-signature account (String)

**Response**
```
{
  "success": true,
  "accounts": "array of accounts"
}
```
