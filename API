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
