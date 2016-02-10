# Crypti API
## INTRODUCTION
Crypti Wallet REST API. All api using /api prefix.
### How to work with API
- Information need to return.
- Success parameter. Determines the success of a response.
- Error perameter. Provided if the success parameter equal "false".
 
API available only after wallet loading, before all routes will return:
```
{
    "success" : false,
    "error" : "loading"
}
```

## Accounts
Account manage api.

### Open account
Open account in wallet.

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
    "address": "Address of account. string",
    "unconfirmedBalance": "Unconfirmed balance of account. integer",
    "balance": "Balance of account. integer",
    "publicKey": "Public key of account. Hex",
    "unconfirmedSignature": "If account enabled second signature, but it's still not confirmed. Boolean: true or false",
    "secondSignature": "If account enabled second signature. Boolean: true or false",
    "secondPublicKey": "Second signature public key. Hex",
    "username": "Username of account."
  }
}
```

### Get balance
Get balance of account.

GET `/api/accounts/getBalance?address=address`

- address: Address of account

**Response**
```
{
  "success": true,
  "balance": "Balance of account",
  "unconfirmedBalance": "Unconfirmed balance of account"
}
```

### Get account public key
Get public key of account. If the account does not exist (not opened) will return error.

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
Will return public key of provided secret key.

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
Will return account by address.

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
Will return account's delegates by address.

GET `/api/accounts/delegates?address=address`

- address: Address of account

**Response**
```
{
    "success": true,
    "delegates": [array]
}
```

### Put delegates
Will vote for selected delegates.

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
Loader API. Providing sync & loading information. Only this API working when wallet on loading.

### Get loading status
Will return account's delegates by address.

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

### Get synchronization status
Get synchronization status of wallet.

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
Transactions API.

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
    "amount" : /* Amount of transaction * 10^8. Example: to send 1.1234 XCR, use 112340000 as amount */,
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



























