

# TTC RPC Curl Examples

#### Common Specification 

All the wallet addresses and hashes in the mainnet start with "t0"

#### Error codes 

If an RPC method encounters an error, the `error` member included on the response object **MUST** be an object containing a `code` member and descriptive `message` member. The following list contains all possible error codes and associated messages:

| Code   | Message              | Meaning                            | Category     |
| ------ | -------------------- | ---------------------------------- | ------------ |
| -32700 | Parse error          | Invalid JSON                       | standard     |
| -32600 | Invalid request      | JSON is not a valid request object | standard     |
| -32601 | Method not found     | Method does not exist              | standard     |
| -32602 | Invalid params       | Invalid method parameters          | standard     |
| -32603 | Internal error       | Internal JSON-RPC error            | standard     |
| -32000 | Invalid input        | Missing or invalid parameters      | non-standard |
| -32001 | Resource not found   | Requested resource not found       | non-standard |
| -32002 | Resource unavailable | Requested resource not available   | non-standard |
| -32003 | Transaction rejected | Transaction creation failed        | non-standard |
| -32004 | Method not supported | Method is not implemented          | non-standard |

Example error response:

```
{
    "id": 1337
    "jsonrpc": "2.0",
    "error": {
        "code": -32003,
        "message": "Transaction rejected"
    }
}
```

#### Value encoding 

Specific types of values passed to and returned from RPC methods require special encoding:

##### Quantity

- A `Quantity` value **MUST** be hex-encoded.
- A `Quantity` value **MUST** be “0x”-prefixed.
- A `Quantity` value **MUST** be expressed using the fewest possible hex digits per byte.
- A `Quantity` value **MUST** express zero as “0x0”.

Examples `Quantity` values:

| Value  | Valid     | Reason                            |
| ------ | --------- | --------------------------------- |
| 0x     | `invalid` | empty not a valid quantity        |
| 0x0    | `valid`   | interpreted as a quantity of zero |
| 0x00   | `invalid` | leading zeroes not allowed        |
| 0x41   | `valid`   | interpreted as a quantity of 65   |
| 0x400  | `valid`   | interpreted as a quantity of 1024 |
| 0x0400 | `invalid` | leading zeroes not allowed        |
| ff     | `invalid` | values must be prefixed           |

##### Data

- A `Data` value **MUST** be hex-encoded.
- A `Data` value **MUST** be “0x”-prefixed.
- A `Data` value **MUST** be expressed using two hex digits per byte.

Examples `Data` values:

| Value    | Valid     | Reason                                             |
| -------- | --------- | -------------------------------------------------- |
| 0x       | `valid`   | interpreted as empty data                          |
| 0x0      | `invalid` | each byte must be represented using two hex digits |
| 0x00     | `valid`   | interpreted as a single zero byte                  |
| 0x41     | `true`    | interpreted as a data value of 65                  |
| 0x004200 | `true`    | interpreted as a data value of 16896               |
| 0xf0f0f  | `false`   | bytes require two hex digits                       |
| 004200   | `false`   | values must be prefixed                            |



# RPC Server

You can also replace "http://rpc-tokyo.ttcnet.io" with yours (http://ip:port/)

#### Methods 

- [eth_accounts](#eth_accounts)
- [eth_gasPrice](#eth_gasPrice)
- [eth_blockNumber](#eth_blockNumber)
- [eth_getBalance](#eth_getBalance)
- [eth_getTransactionCount](#eth_getTransactionCount)
- [eth_getTransactionByHash](#eth_getTransactionByHash)
- [eth_getTransactionByBlockHashAndIndex](#eth_getTransactionByBlockHashAndIndex)
- [eth_getTransactionByBlockNumberAndIndex](#eth_getTransactionByBlockNumberAndIndex)
- [eth_getTransactionReceipt](#eth_getTransactionReceipt)
- [eth_getBlockTransactionCountByHash](#eth_getBlockTransactionCountByHash)
- [eth_getBlockTransactionCountByNumber](#eth_getBlockTransactionCountByNumber)
- [eth_getBlockByHash](#eth_getBlockByHash)
- [eth_getBlockByNumber](#eth_getBlockByNumber)
- [eth_estimateGas](#eth_estimateGas)
- [eth_sign](#eth_sign)
- [eth_signTransaction](#eth_signTransaction)
- [eth_sendTransaction](#eth_sendTransaction)
- [eth_sendRawTransaction](#eth_sendRawTransaction)
- [eth_call](#eth_call)
- [net_version](#net_version)
- [net_listening](#net_listening)
- [net_peerCount](#net_peerCount)
- [eth_syncing](#eth_syncing)
- [eth_mining](#eth_mining)



#### eth_accounts

#### Description 

Returns a list of addresses owned by this client

#### Parameters 

*(none)*

#### Returns 

{[`Data[]`]} - array of addresses

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":["t0752C939ADe161dFC460346C34a1bb415c87A60bF"]}
```



#### eth_gasPrice

#### Description 

Returns the current price of gas expressed in wei

#### Parameters 

*(none)*

#### Returns 

{[`Quantity`](#Quantity)} - current gas price in wei

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"0x1c9c380"}

```



#### eth_blockNumber

#### Description 

Returns the number of the most recent block seen by this client

#### Parameters 

*(none)*

#### Returns 

{[`Quantity`](#Quantity)} - number of the latest block

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"0xe41"}
```



#### eth_getBalance

#### Description 

Returns the balance of an address in wei

#### Parameters 

| #    | Type                               | Description                                                  |
| ---- | ---------------------------------- | ------------------------------------------------------------ |
| 1    | {[`Data`](#Data)}                  | address to query for balance                                 |
| 2    | {[`Quantity`](#Quantity)|`string`} | block number, or one of `"latest"`, `"earliest"` or `"pending"` |

#### Returns

{[`Quantity`](#Quantity)} - balance of the provided account in wei

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":[
"t0752c939ade161dfc460346c34a1bb415c87a60bf","latest"],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"0x20000000000000000000000000000000000000000000321c405032af24e9a72"}
//904625697166533220022806242204646464202662822686422062006024204824882284082
```



#### eth_getTransactionCount

#### Description 

Returns the number of transactions sent from an address

#### Parameters 

| #    | Type                               | Description                                                  |
| ---- | ---------------------------------- | ------------------------------------------------------------ |
| 1    | {[`Data`](#Data)}                  | address to query for sent transactions                       |
| 2    | {[`Quantity`](#Quantity)|`string`} | block number, or one of `"latest"`, `"earliest"` or `"pending"` |

#### Returns 

{[`Quantity`]} - number of transactions sent from the specified address

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getTransactionCount","params":["t0752c939ade161dfc460346c34a1bb415c87a60bf","latest"],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"0x4"}//4
```



#### eth_getTransactionByHash

#### Description 

Returns information about a transaction specified by hash

#### Parameters 

| #    | Type              | Description           |
| ---- | ----------------- | --------------------- |
| 1    | {[`Data`](#Data)} | hash of a transaction |

#### Returns 

{`null|object`} - `null` if no transaction is found, otherwise a transaction object with the following members:

- {[`Data`](#Data)} `r` - ECDSA signature r
- {[`Data`](#Data)} `s` - ECDSA signature s
- {[`Data`](#Data)} `blockHash` - hash of block containing this transaction or `null` if pending
- {[`Data`](#Data)} `from` - transaction sender
- {[`Data`](#Data)} `hash` - hash of this transaction
- {[`Data`](#Data)} `input` - contract code or a hashed method call
- {[`Data`](#Data)} `to` - transaction recipient or `null` if deploying a contract
- {[`Quantity`](#Quantity)} `v` - ECDSA recovery ID
- {[`Quantity`](#Quantity)} `blockNumber` - number of block containing this transaction or `null` if pending
- {[`Quantity`](#Quantity)} `gas` - gas provided for transaction execution
- {[`Quantity`](#Quantity)} `gasPrice` - price in wei of each gas used
- {[`Quantity`](#Quantity)} `nonce` - unique number identifying this transaction
- {[`Quantity`](#Quantity)} `transactionIndex` - index of this transaction in the block or `null` if pending
- {[`Quantity`](#Quantity)} `value` - value in wei sent with this transaction

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["t09a4b3aedf4345a1ee8334896d9ec430f3689093962e308ee837c428a3683f465"],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":{"blockHash":"t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","blockNumber":"0xf5e
","from":"t043a788F4fb5889D6098eCdBbce2b723380f36950","gas":"0x2dc6c0","gasPrice":"0x1c9c380","hash":"t09a4b3aedf4345a1ee8334896d9ec430f
3689093962e308ee837c428a3683f465","input":"0x75666f3a313a73633a636f6e6669726d3a743130303030303030303030303030303030303030303030303030303
0303030303030303030303030303030303030303030303030303030303030303030303030313a31303739393a313535333439393239303a3130373933237431304235386
1376638364341386430353334354536433533413034333739413638363662664232383923313037393223743130423538613766383643413864303533343545364335334
1303433373941363836366266423238392331303739312374313042353861376638364341386430353334354536433533413034333739413638363662664232383923313
0373930237431346631323033356444344232463134354638394138424338393937663230653064663942313441372331303738392374313466313230333564443442324
6313435463839413842433839393766323065306466394231344137233130373838237431346631323033356444344232463134354638394138424338393937663230653
0646639423134413723313037383723743134336137383846346662353838394436303938654364426263653262373233333830663336393530233130373836237431343
361373838463466623538383944363039386543644262636532623732333338306633363935303a","nonce":"0x44b","to":"t043a788F4fb5889D6098eCdBbce2b723
380f36950","transactionIndex":"0x0","value":"0x0","v":"0xa67","r":"0x495d2787b2b7a896a70891028ef99d7c4f5abb6be495111b21844a39321bf53","s
":"0x3d3d15b5e160f63a49a3009ad31b7bf9e1fe171210aed21c3c9941f9144901ba"}}

```



#### eth_getTransactionByBlockHashAndIndex

#### Description 

Returns information about a transaction specified by block hash and transaction index

#### Parameters 

| #    | Type                      | Description                                   |
| ---- | ------------------------- | --------------------------------------------- |
| 1    | {[`Data`](#Data)}         | hash of a block                               |
| 2    | {[`Quantity`](#Quantity)} | index of a transaction in the specified block |

#### Returns 

{`null|object`} - `null` if no transaction is found, otherwise a transaction object with the following members:

- {[`Data`](#Data)} `r` - ECDSA signature r
- {[`Data`](#Data)} `s` - ECDSA signature s
- {[`Data`](#Data)} `blockHash` - hash of block containing this transaction or `null` if pending
- {[`Data`](#Data)} `from` - transaction sender
- {[`Data`](#Data)} `hash` - hash of this transaction
- {[`Data`](#Data)} `input` - contract code or a hashed method call
- {[`Data`](#Data)} `to` - transaction recipient or `null` if deploying a contract
- {[`Quantity`](#Quantity)} `v` - ECDSA recovery ID
- {[`Quantity`](#Quantity)} `blockNumber` - number of block containing this transaction or `null` if pending
- {[`Quantity`](#Quantity)} `gas` - gas provided for transaction execution
- {[`Quantity`](#Quantity)} `gasPrice` - price in wei of each gas used
- {[`Quantity`](#Quantity)} `nonce` - unique number identifying this transaction
- {[`Quantity`](#Quantity)} `transactionIndex` - index of this transaction in the block or `null` if pending
- {[`Quantity`](#Quantity)} `value` - value in wei sent with this transaction

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockHashAndIndex","params":["t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","0x0"],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":{"blockHash":"t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","blockNumber":"0xf5e
","from":"t043a788F4fb5889D6098eCdBbce2b723380f36950","gas":"0x2dc6c0","gasPrice":"0x1c9c380","hash":"t09a4b3aedf4345a1ee8334896d9ec430f
3689093962e308ee837c428a3683f465","input":"0x75666f3a313a73633a636f6e6669726d3a743130303030303030303030303030303030303030303030303030303
0303030303030303030303030303030303030303030303030303030303030303030303030313a31303739393a313535333439393239303a3130373933237431304235386
1376638364341386430353334354536433533413034333739413638363662664232383923313037393223743130423538613766383643413864303533343545364335334
1303433373941363836366266423238392331303739312374313042353861376638364341386430353334354536433533413034333739413638363662664232383923313
0373930237431346631323033356444344232463134354638394138424338393937663230653064663942313441372331303738392374313466313230333564443442324
6313435463839413842433839393766323065306466394231344137233130373838237431346631323033356444344232463134354638394138424338393937663230653
0646639423134413723313037383723743134336137383846346662353838394436303938654364426263653262373233333830663336393530233130373836237431343
361373838463466623538383944363039386543644262636532623732333338306633363935303a","nonce":"0x44b","to":"t043a788F4fb5889D6098eCdBbce2b723
380f36950","transactionIndex":"0x0","value":"0x0","v":"0xa67","r":"0x495d2787b2b7a896a70891028ef99d7c4f5abb6be495111b21844a39321bf53","s
":"0x3d3d15b5e160f63a49a3009ad31b7bf9e1fe171210aed21c3c9941f9144901ba"}}

```



#### eth_getTransactionByBlockNumberAndIndex

#### Description 

Returns information about a transaction specified by block number and transaction index

#### Parameters 

| #    | Type                               | Description                                                  |
| ---- | ---------------------------------- | ------------------------------------------------------------ |
| 1    | {[`Quantity`](#Quantity)|`string`} | block number, or one of `"latest"`, `"earliest"` or `"pending"` |
| 2    | {[`Quantity`](#Quantity)}          | index of a transaction in the specified block                |

#### Returns 

{`null|object`} - `null` if no transaction is found, otherwise a transaction object with the following members:

- {[`Data`](#Data)} `r` - ECDSA signature r
- {[`Data`](#Data)} `s` - ECDSA signature s
- {[`Data`](#Data)} `blockHash` - hash of block containing this transaction or `null` if pending
- {[`Data`](#Data)} `from` - transaction sender
- {[`Data`](#Data)} `hash` - hash of this transaction
- {[`Data`](#Data)} `input` - contract code or a hashed method call
- {[`Data`](#Data)} `to` - transaction recipient or `null` if deploying a contract
- {[`Quantity`](#Quantity)} `v` - ECDSA recovery ID
- {[`Quantity`](#Quantity)} `blockNumber` - number of block containing this transaction or `null` if pending
- {[`Quantity`](#Quantity)} `gas` - gas provided for transaction execution
- {[`Quantity`](#Quantity)} `gasPrice` - price in wei of each gas used
- {[`Quantity`](#Quantity)} `nonce` - unique number identifying this transaction
- {[`Quantity`](#Quantity)} `transactionIndex` - index of this transaction in the block or `null` if pending
- {[`Quantity`](#Quantity)} `value` - value in wei sent with this transaction

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockNumberAndIndex","params":["0xf5e","0x0"],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":{"blockHash":"t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","blockNumber":"0xf5e
","from":"t043a788F4fb5889D6098eCdBbce2b723380f36950","gas":"0x2dc6c0","gasPrice":"0x1c9c380","hash":"t09a4b3aedf4345a1ee8334896d9ec430f
3689093962e308ee837c428a3683f465","input":"0x75666f3a313a73633a636f6e6669726d3a743130303030303030303030303030303030303030303030303030303
0303030303030303030303030303030303030303030303030303030303030303030303030313a31303739393a313535333439393239303a3130373933237431304235386
1376638364341386430353334354536433533413034333739413638363662664232383923313037393223743130423538613766383643413864303533343545364335334
1303433373941363836366266423238392331303739312374313042353861376638364341386430353334354536433533413034333739413638363662664232383923313
0373930237431346631323033356444344232463134354638394138424338393937663230653064663942313441372331303738392374313466313230333564443442324
6313435463839413842433839393766323065306466394231344137233130373838237431346631323033356444344232463134354638394138424338393937663230653
0646639423134413723313037383723743134336137383846346662353838394436303938654364426263653262373233333830663336393530233130373836237431343
361373838463466623538383944363039386543644262636532623732333338306633363935303a","nonce":"0x44b","to":"t043a788F4fb5889D6098eCdBbce2b723
380f36950","transactionIndex":"0x0","value":"0x0","v":"0xa67","r":"0x495d2787b2b7a896a70891028ef99d7c4f5abb6be495111b21844a39321bf53","s
":"0x3d3d15b5e160f63a49a3009ad31b7bf9e1fe171210aed21c3c9941f9144901ba"}}

```



#### eth_getTransactionReceipt

#### Description 

Returns the receipt of a transaction specified by hash

**Note:** Transaction receipts are unavailable for pending transactions.

#### Parameters 

| #    | Type              | Description           |
| ---- | ----------------- | --------------------- |
| 1    | {[`Data`](#Data)} | hash of a transaction |

#### Returns 

{`null|object`} - `null` if no transaction is found, otherwise a transaction receipt object with the following members:

- {[`Data`](#Data)} `blockHash` - hash of block containing this transaction
- {[`Data`](#Data)} `contractAddress` - address of new contract or `null` if no contract was created
- {[`Data`](#Data)} `from` - transaction sender
- {[`Data`](#Data)} `logsBloom` - logs bloom filter
- {[`Data`](#Data)} `to` - transaction recipient or `null` if deploying a contract
- {[`Data`](#Data)} `transactionHash` - hash of this transaction
- {[`Quantity`](#Quantity)} `blockNumber` - number of block containing this transaction
- {[`Quantity`](#Quantity)} `cumulativeGasUsed` - gas used by this and all preceding transactions in this block
- {[`Quantity`](#Quantity)} `gasUsed` - gas used by this transaction
- {[`Quantity`](#Quantity)} `status` - `1` if this transaction was successful or `0` if it failed
- {[`Quantity`](#Quantity)} `transactionIndex` - index of this transaction in the block
- {`Array<Log>`} `logs` - list of log objects generated by this transaction

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params":["t09a4b3aedf4345a1ee8334896d9ec430f3689093962e308ee837c428a3683f465"],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":{"blockHash":"t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","blockNumber":"0xf5e
","contractAddress":null,"cumulativeGasUsed":"0xd4fc","from":"t043a788f4fb5889d6098ecdbbce2b723380f36950","gasUsed":"0xd4fc","logs":[],"
logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","
to":"t043a788F4fb5889D6098eCdBbce2b723380f36950","transactionHash":"t09a4b3aedf4345a1ee8334896d9ec430f3689093962e308ee837c428a3683f465",
"transactionIndex":"0x0"}}
```



#### eth_getBlockTransactionCountByHash

#### Description 

Returns the number of transactions in a block specified by block hash

#### Parameters 

| #    | Type              | Description     |
| ---- | ----------------- | --------------- |
| 1    | {[`Data`](#Data)} | hash of a block |

#### Returns 

{[`Quantity`](#Quantity)} - number of transactions in the specified block

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByHash","params":["t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b"],"id":67}' http://rpc-tokyo.ttcnet.io


#Response
{"jsonrpc":"2.0","id":67,"result":"0x2"}//2
```



#### eth_getBlockTransactionCountByNumber

#### Description 

Returns the number of transactions in a block specified by block number

#### Parameters 

| #    | Type                               | Description                                                  |
| ---- | ---------------------------------- | ------------------------------------------------------------ |
| 1    | {[`Quantity`](#Quantity)|`string`} | block number, or one of `"latest"`, `"earliest"` or `"pending"` |

#### Returns 

{[`Quantity`](#Quantity)} - number of transactions in the specified block

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByNumber","params":["0xf5e"],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"0x2"}//2

```



#### eth_getBlockByHash

#### Description 

Returns information about a block specified by hash

#### Parameters 

| #    | Type              | Description                                                  |
| ---- | ----------------- | ------------------------------------------------------------ |
| 1    | {[`Data`](#Data)} | hash of a block                                              |
| 2    | {`boolean`}       | `true` will pull full transaction objects, `false` will pull transaction hashes |

#### Returns 

{`null|object`} - `null` if no block is found, otherwise a block object with the following members:

- {[`Data`](#Data)} `extraData` - “extra data” field of this block
- {[`Data`](#Data)} `hash` - block hash or `null` if pending
- {[`Data`](#Data)} `logsBloom` - logs bloom filter or `null` if pending
- {[`Data`](#Data)} `miner` - address that received this block’s mining rewards
- {[`Data`](#Data)} `nonce` - proof-of-work hash or `null` if pending
- {[`Data`](#Data)} `parentHash` - parent block hash
- {[`Data`](#Data)} `receiptsRoot` -root of the this block’s receipts trie
- {[`Data`](#Data)} `sha3Uncles` - SHA3 of the uncles data in this block
- {[`Data`](#Data)} `stateRoot` - root of this block’s final state trie
- {[`Data`](#Data)} `transactionsRoot` - root of this block’s transaction trie
- {[`Quantity`](#Quantity)} `difficulty` - difficulty for this block
- {[`Quantity`](#Quantity)} `gasLimit` - maximum gas allowed in this block
- {[`Quantity`](#Quantity)} `gasUsed` - total used gas by all transactions in this block
- {[`Quantity`](#Quantity)} `number` - block number or `null` if pending
- {[`Quantity`](#Quantity)} `size` - size of this block in bytes
- {[`Quantity`](#Quantity)} `timestamp` - unix timestamp of when this block was collated
- {[`Quantity`](#Quantity)} `totalDifficulty` - total difficulty of the chain until this block
- {`Array<Transaction>`} `transactions` - list of transaction objects or hashes
- {`Array<Transaction>`} `uncles` - list of uncle hashes

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params":["t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b",true],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":{"difficulty":"0x1","extraData":"0xd7820103846765746888676f312e31312e35856c696e75780000000000000000f90
21cc0c0c0c0c0845c66ab5af83f949f4fa2a2c5195cb8ad0521181aab31a55b5bfb3194aef47cf078825297bb897b8cf8d72d0757fa157d94752c939ade161dfc460346c
34a1bb415c87a60bfc0820f5cf901c7f901c4a000000000000000000000000000000000000000000000000000000000000000019443a788f4fb5889d6098ecdbbce2b723
380f36950822a2ff90188853130373933aa743130423538613766383643413864303533343545364335334130343337394136383636626642323839853130373932aa743
130423538613766383643413864303533343545364335334130343337394136383636626642323839853130373931aa74313042353861376638364341386430353334354
5364335334130343337394136383636626642323839853130373930aa7431346631323033356444344232463134354638394138424338393937663230653064663942313
44137853130373839aa743134663132303335644434423246313435463839413842433839393766323065306466394231344137853130373838aa7431346631323033356
44434423246313435463839413842433839393766323065306466394231344137853130373837aa743134336137383846346662353838394436303938654364426263653
262373233333830663336393530853130373836aa743134336137383846346662353838394436303938654364426263653262373233333830663336393530c0c0c0c9bf1
ef85822b7e0d1f85c8eb50d8ed13665ff7fd97623bb9c4721d825637184207a0005eb5b7834e85961d8ead1f5169a0b387779b704452aad2795adc80ff200","gasLimit
":"0x2faf080","gasUsed":"0x12704","hash":"t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","logsBloom":"0x00000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000","miner":"t0aef47cf078825297bb897b8cf8d72d
0757fa157d","mixHash":"t00000000000000000000000000000000000000000000000000000000000000000","nonce":"0x0000000000000000","number":"0xf5e"
,"parentHash":"t0e355687649a7ecf7e9c02411f8b2b3ab9e4066ac020d440e8280f9427b3ce7d5","receiptsRoot":"t0f39cc39809051789c907635a38fc0c0749b
41892cf5d467782bb17e01dbb88b9","sha3Uncles":"t01dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347","size":"0x748","stateRo
ot":"t0675ff5b961e39a1420a7d739dbbac87b5fbe60ef0106f588ef91e844d37c5c3a","timestamp":"0x5c9884a0","totalDifficulty":"0xf5f","transaction
s":[{"blockHash":"t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","blockNumber":"0xf5e","from":"t043a788F4fb5889D609
8eCdBbce2b723380f36950","gas":"0x2dc6c0","gasPrice":"0x1c9c380","hash":"t09a4b3aedf4345a1ee8334896d9ec430f3689093962e308ee837c428a3683f4
65","input":"0x75666f3a313a73633a636f6e6669726d3a743130303030303030303030303030303030303030303030303030303030303030303030303030303030303
0303030303030303030303030303030303030303030313a31303739393a313535333439393239303a3130373933237431304235386137663836434138643035333435453
6433533413034333739413638363662664232383923313037393223743130423538613766383643413864303533343545364335334130343337394136383636626642323
8392331303739312374313042353861376638364341386430353334354536433533413034333739413638363662664232383923313037393023743134663132303335644
4344232463134354638394138424338393937663230653064663942313441372331303738392374313466313230333564443442324631343546383941384243383939376
6323065306466394231344137233130373838237431346631323033356444344232463134354638394138424338393937663230653064663942313441372331303738372
3743134336137383846346662353838394436303938654364426263653262373233333830663336393530233130373836237431343361373838463466623538383944363
039386543644262636532623732333338306633363935303a","nonce":"0x44b","to":"t043a788F4fb5889D6098eCdBbce2b723380f36950","transactionIndex":
"0x0","value":"0x0","v":"0xa67","r":"0x495d2787b2b7a896a70891028ef99d7c4f5abb6be495111b21844a39321bf53","s":"0x3d3d15b5e160f63a49a3009ad
31b7bf9e1fe171210aed21c3c9941f9144901ba"},{"blockHash":"t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","blockNumber
":"0xf5e","from":"t0752C939ADe161dFC460346C34a1bb415c87A60bF","gas":"0x15f90","gasPrice":"0x1c9c380","hash":"t09a433f4fbb52963924966c6bd
2fff42c0266bd5b5bb8f975fb8e991696de30b3","input":"0x","nonce":"0x4","to":"t0752C939ADe161dFC460346C34a1bb415c87A60bF","transactionIndex"
:"0x1","value":"0x0","v":"0xa68","r":"0x89f18f7271b4b5547473176d59a00bf3c025505c5b57561835b26039aeebd4eb","s":"0x3cb0b6489eb4f3f35d695d4
513a7951cfae90037c289fd4b3f1b1586d083bfa1"}],"transactionsRoot":"t0430d237a3af732dcc03e939c4367f4c5200d41b94ab04a4a2e51752b8e107d03","un
cles":[]}}
```



#### eth_getBlockByNumber

#### Description 

Returns information about a block specified by number

#### Parameters 

| #    | Type                               | Description                                                  |
| ---- | ---------------------------------- | ------------------------------------------------------------ |
| 1    | {[`Quantity`](#Quantity)|`string`} | block number, or one of `"latest"`, `"earliest"` or `"pending"` |
| 2    | {`boolean`}                        | `true` will pull full transaction objects, `false` will pull transaction hashes |

#### Returns 

{`null|object`} - `null` if no block is found, otherwise a block object with the following members:

- {[`Data`](#Data)} `extraData` - “extra data” field of this block
- {[`Data`](#Data)} `hash` - block hash or `null` if pending
- {[`Data`](#Data)} `logsBloom` - logs bloom filter or `null` if pending
- {[`Data`](#Data)} `miner` - address that received this block’s mining rewards
- {[`Data`](#Data)} `nonce` - proof-of-work hash or `null` if pending
- {[`Data`](#Data)} `parentHash` - parent block hash
- {[`Data`](#Data)} `receiptsRoot` -root of the this block’s receipts trie
- {[`Data`](#Data)} `sha3Uncles` - SHA3 of the uncles data in this block
- {[`Data`](#Data)} `stateRoot` - root of this block’s final state trie
- {[`Data`](#Data)} `transactionsRoot` - root of this block’s transaction trie
- {[`Quantity`](#Quantity)} `difficulty` - difficulty for this block
- {[`Quantity`](#Quantity)} `gasLimit` - maximum gas allowed in this block
- {[`Quantity`](#Quantity)} `gasUsed` - total used gas by all transactions in this block
- {[`Quantity`](#Quantity)} `number` - block number or `null` if pending
- {[`Quantity`](#Quantity)} `size` - size of this block in bytes
- {[`Quantity`](#Quantity)} `timestamp` - unix timestamp of when this block was collated
- {[`Quantity`](#Quantity)} `totalDifficulty` - total difficulty of the chain until this block
- {`Array<Transaction>`} `transactions` - list of transaction objects or hashes
- {`Array<Transaction>`} `uncles` - list of uncle hashes

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["0xf5e",true],"id":67}' http://rpc-tokyo.ttcnet.io
//0xf5e = 3934

#Response
{"jsonrpc":"2.0","id":67,"result":{"difficulty":"0x1","extraData":"0xd7820103846765746888676f312e31312e35856c696e75780000000000000000f90
21cc0c0c0c0c0845c66ab5af83f949f4fa2a2c5195cb8ad0521181aab31a55b5bfb3194aef47cf078825297bb897b8cf8d72d0757fa157d94752c939ade161dfc460346c
34a1bb415c87a60bfc0820f5cf901c7f901c4a000000000000000000000000000000000000000000000000000000000000000019443a788f4fb5889d6098ecdbbce2b723
380f36950822a2ff90188853130373933aa743130423538613766383643413864303533343545364335334130343337394136383636626642323839853130373932aa743
130423538613766383643413864303533343545364335334130343337394136383636626642323839853130373931aa74313042353861376638364341386430353334354
5364335334130343337394136383636626642323839853130373930aa7431346631323033356444344232463134354638394138424338393937663230653064663942313
44137853130373839aa743134663132303335644434423246313435463839413842433839393766323065306466394231344137853130373838aa7431346631323033356
44434423246313435463839413842433839393766323065306466394231344137853130373837aa743134336137383846346662353838394436303938654364426263653
262373233333830663336393530853130373836aa743134336137383846346662353838394436303938654364426263653262373233333830663336393530c0c0c0c9bf1
ef85822b7e0d1f85c8eb50d8ed13665ff7fd97623bb9c4721d825637184207a0005eb5b7834e85961d8ead1f5169a0b387779b704452aad2795adc80ff200","gasLimit
":"0x2faf080","gasUsed":"0x12704","hash":"t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","logsBloom":"0x00000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000","miner":"t0aef47cf078825297bb897b8cf8d72d
0757fa157d","mixHash":"t00000000000000000000000000000000000000000000000000000000000000000","nonce":"0x0000000000000000","number":"0xf5e"
,"parentHash":"t0e355687649a7ecf7e9c02411f8b2b3ab9e4066ac020d440e8280f9427b3ce7d5","receiptsRoot":"t0f39cc39809051789c907635a38fc0c0749b
41892cf5d467782bb17e01dbb88b9","sha3Uncles":"t01dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347","size":"0x748","stateRo
ot":"t0675ff5b961e39a1420a7d739dbbac87b5fbe60ef0106f588ef91e844d37c5c3a","timestamp":"0x5c9884a0","totalDifficulty":"0xf5f","transaction
s":[{"blockHash":"t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","blockNumber":"0xf5e","from":"t043a788F4fb5889D609
8eCdBbce2b723380f36950","gas":"0x2dc6c0","gasPrice":"0x1c9c380","hash":"t09a4b3aedf4345a1ee8334896d9ec430f3689093962e308ee837c428a3683f4
65","input":"0x75666f3a313a73633a636f6e6669726d3a743130303030303030303030303030303030303030303030303030303030303030303030303030303030303
0303030303030303030303030303030303030303030313a31303739393a313535333439393239303a3130373933237431304235386137663836434138643035333435453
6433533413034333739413638363662664232383923313037393223743130423538613766383643413864303533343545364335334130343337394136383636626642323
8392331303739312374313042353861376638364341386430353334354536433533413034333739413638363662664232383923313037393023743134663132303335644
4344232463134354638394138424338393937663230653064663942313441372331303738392374313466313230333564443442324631343546383941384243383939376
6323065306466394231344137233130373838237431346631323033356444344232463134354638394138424338393937663230653064663942313441372331303738372
3743134336137383846346662353838394436303938654364426263653262373233333830663336393530233130373836237431343361373838463466623538383944363
039386543644262636532623732333338306633363935303a","nonce":"0x44b","to":"t043a788F4fb5889D6098eCdBbce2b723380f36950","transactionIndex":
"0x0","value":"0x0","v":"0xa67","r":"0x495d2787b2b7a896a70891028ef99d7c4f5abb6be495111b21844a39321bf53","s":"0x3d3d15b5e160f63a49a3009ad
31b7bf9e1fe171210aed21c3c9941f9144901ba"},{"blockHash":"t0d8ce94cb71fa85635a4ac0899b2fd8cdf5a442c6d3f02abe52f8fbd1f04df92b","blockNumber
":"0xf5e","from":"t0752C939ADe161dFC460346C34a1bb415c87A60bF","gas":"0x15f90","gasPrice":"0x1c9c380","hash":"t09a433f4fbb52963924966c6bd
2fff42c0266bd5b5bb8f975fb8e991696de30b3","input":"0x","nonce":"0x4","to":"t0752C939ADe161dFC460346C34a1bb415c87A60bF","transactionIndex"
:"0x1","value":"0x0","v":"0xa68","r":"0x89f18f7271b4b5547473176d59a00bf3c025505c5b57561835b26039aeebd4eb","s":"0x3cb0b6489eb4f3f35d695d4
513a7951cfae90037c289fd4b3f1b1586d083bfa1"}],"transactionsRoot":"t0430d237a3af732dcc03e939c4367f4c5200d41b94ab04a4a2e51752b8e107d03","un
cles":[]}}

```



#### eth_estimateGas

#### Description 

Estimates the gas necessary to complete a transaction without submitting it to the network

**Note:** The resulting gas estimation may be significantly more than the amount of gas actually used by the transaction. This is due to a variety of reasons including EVM mechanics and node performance.

#### Parameters 

| #    | Type                               | Description                                                  |
| ---- | ---------------------------------- | ------------------------------------------------------------ |
| 1    | {`object`}                         | @property {[`Data`](#Data)} `[from]` - transaction sender<br />@property {[`Data`](#Data)} `[to]` - transaction recipient<br />@property {[`Quantity`](#Quantity)} `[gas]` - gas provided for transaction execution<br />@property {[`Quantity`](#Quantity)} `[gasPrice]` - price in wei of each gas used<br />@property {[`Quantity`](#Quantity)} `[value]` - value in wei sent with this transaction<br />@property {[`Data`](#Data)} `[data]` - contract code or a hashed method call with encoded args |
| 2    | {[`Quantity`](#Quantity)|`string`} | block number, or one of `"latest"`, `"earliest"` or `"pending"` |

#### Returns 

{[`Quantity`](#Quantity)} - amount of gas required by transaction

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_estimateGas","params":[{"from":"t0752c939ade161dfc460346c34a1bb415c87a60bf","to":"t0752c939ade161dfc460346c34a1bb415c87a60bf","gasPrice":"0x9184e72a000", "gas":"0x76c0", "value":"0x1", "data":"0x"}],"id":67}' http://rpc-tokyo.ttcnet.io

#{"jsonrpc":"2.0","id":67,"result":"0x5208"}//20520
```



#### eth_sign

Calculates an specific signature in the form of `keccak256("\x19 Signed Message:\n" + len(message) + message))`

#### Parameters 

| #    | Type              | Description                |
| ---- | ----------------- | -------------------------- |
| 1    | {[`Data`](#Data)} | address to use for signing |
| 2    | {[`Data`](#Data)} | data to sign               |

#### Returns 

{[`Data`](#Data)} - signature hash of the provided data

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_sign","params":["t0752c939ade161dfc460346c34a1bb415c87a60bf","0xdeadbeaf"],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"0x5b4be7f601895254034167f674c3b37bbba55745f89bcf2fbce983557b2735af1e5754ae85c8ba3e6cf1f1a7a7e382f4567
2c9ce1b666207c8880e82db9490a81c"}

```



#### eth_signTransaction

#### Description 

Signs a transaction that can be submitted to the network at a later time using with `eth_sendRawTransaction`

#### Parameters 

| #    | Type       | Description                                                  |
| ---- | ---------- | ------------------------------------------------------------ |
| 1    | {`object`} | @property {[`Data`](#Data)} `from` - transaction senderxx<br />@property {[`Data`](#Data)} `[to]` - transaction recipient<br />@property {[`Quantity`](#Quantity)} `[gas="0x76c0"]` - gas provided for transaction execution<br />@property {[`Quantity`](#Quantity)} `[gasPrice]` - price in wei of each gas used<br />@property {[`Quantity`](#Quantity)} `[value]` - value in wei sent with this transaction@property {[`Data`](#Data)} `[data]` - contract code or a hashed method call with encoded args<br />@property {[`Quantity`](#Quantity)} `[nonce]` - unique number identifying this transaction |

#### Returns 

{[`Data`](#Data)} - signature hash of the transaction object

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_signTransaction","params":[{"from":"t0752c939ade161dfc460346c34a1bb415c87a60bf","to":"t0752c939ade161dfc460346c34a1bb415c87a60bf","gasPrice":"0x9184e72a000", "gas":"0x76c0", "value":"0x1", "data":"0x", "nonce":"0x5"}],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":{"raw":"0xf867058609184e72a0008276c094752c939ade161dfc460346c34a1bb415c87a60bf0180820a67a0a177506363ad9ac721341cbd19f78a68a1069acd7c50f796eca20e759e82a45aa0420b460ef46139560e67a2f3f697236ee54fbd3364f5911cab05ab65988fbc69","tx":{"nonce":"0x5","gasPrice":"0x9184e72a000","gas":"0x76c0","to":"t0752C939ADe161dFC460346C34a1bb415c87A60bF","value":"0x1","input":"0x","v":"0xa67","r":"0xa177506363ad9ac721341cbd19f78a68a1069acd7c50f796eca20e759e82a45a","s":"0x420b460ef46139560e67a2f3f697236ee54fbd3364f5911cab05ab65988fbc69","hash":"t04deb2e30dab439aaedcdb9b79649e59a90fe4f96bc1c71dfac95f3da92901ee7"}}}
```



#### eth_sendTransaction

#### Description 

Creates, signs, and sends a new transaction to the network

#### Parameters 

| #    | Type       | Description                                                  |
| ---- | ---------- | ------------------------------------------------------------ |
| 1    | {`object`} | @property {[`Data`](#Data)} `from` - transaction sender@property {[`Data`](#Data)} `[to]` - transaction recipient@property {[`Quantity`](#Quantity)} `[gas="0x76c0"]` - gas provided for transaction execution@property {[`Quantity`](#Quantity)} `[gasPrice]` - price in wei of each gas used@property {[`Quantity`](#Quantity)} `[value]` - value in wei sent with this transaction@property {[`Data`](#Data)} `[data]` - contract code or a hashed method call with encoded args@property {[`Quantity`](#Quantity)} `[nonce]` - unique number identifying this transaction |

#### Returns 

{[`Data`](#Data)} - transaction hash, or the zero hash if the transaction is not yet available

#### Example 

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_sendTransaction","params":[{"from":"t0752c939ade161dfc460346c34a1bb415c87a60bf","to":"t0752c939ade161dfc460346c34a1bb415c87a60bf","gasPrice":"0x9184e72a000", "gas":"0x76c0", "value":"0x1", "data":"0x"}],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"t04deb2e30dab439aaedcdb9b79649e59a90fe4f96bc1c71dfac95f3da92901ee7"}
```



#### eth_sendRawTransaction

#### Description 

Sends and already-signed transaction to the network

#### Parameters 

| #    | Type              | Description             |
| ---- | ----------------- | ----------------------- |
| 1    | {[`Data`](#Data)} | signed transaction data |

#### Returns 

{[`Data`](#Data)} - transaction hash, or the zero hash if the transaction is not yet available

#### Example



```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_sendRawTransaction","params":["0xf867058609184e72a0008276c094752c939ade161dfc460346c34a1bb415c87a60bf0180820a67a0a177506363ad9ac721341cbd19f78a68a1069acd7c50f796eca20e759e82a45aa0420b460ef46139560e67a2f3f697236ee54fbd3364f5911cab05ab65988fbc69"],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"t04deb2e30dab439aaedcdb9b79649e59a90fe4f96bc1c71dfac95f3da92901ee7"}
```



#### eth_call

#### Description 

Executes a new message call immediately without submitting a transaction to the network

#### Parameters 

| #    | Type                               | Description                                                  |
| ---- | ---------------------------------- | ------------------------------------------------------------ |
| 1    | {`object`}                         | @property {[`Data`](#Data)} `[from]` - transaction sender<br />@property {[`Data`](#Data)} `to` - transaction recipient or `null` if deploying a contract<br />@property {[`Quantity`](#Quantity)} `[gas]` - gas provided for transaction execution<br />@property {[`Quantity`](#Quantity)} `[gasPrice]` - price in wei of each gas used<br />@property {[`Quantity`](#Quantity)} `[value]` - value in wei sent with this transaction<br />@property {[`Data`](#Data)} `[data]` - contract code or a hashed method call with encoded args |
| 2    | {[`Quantity`](#Quantity)|`string`} | block number, or one of `"latest"`, `"earliest"` or `"pending"` |

#### Returns 

{[`Data`]} - return value of executed contract

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_call","params":[{"from":"t0752c939ade161dfc460346c34a1bb415c87a60bf","to":"t0752c939ade161dfc460346c34a1bb415c87a60bf","gasPrice":"0x9184e72a000", "gas":"0x76c0", "value":"0x1", "data":"0x"},"latest"],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"0x"}
```



#### net_version

#### Description 

Returns the chain ID associated with the current network

#### Parameters 

*(none)*

#### Returns 

{`string`} - chain ID associated with the current network

#### Example

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"net_version","params":[],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"1314"}
```



#### net_listening

#### Description 

Determines if this client is listening for new network connections

#### Parameters 

*(none)*

#### Returns 

{`boolean`} - `true` if listening is active or `false` if listening is not active

#### Example 

Request

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":true}
```



#### net_peerCount

Returns the number of peers currently connected to this client

#### Parameters 

*(none)*

#### Returns 

{[`Quantity`](#Quantity)} - number of connected peers

#### Example

Request

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
{"jsonrpc":"2.0","id":67,"result":"0x2"}//2
```



#### eth_syncing

#### Description 

Returns information about the status of this client’s network synchronization

#### Parameters 

*(none)*

#### Returns 

{`boolean|object`} - `false` if this client is not syncing with the network, otherwise an object with the following members:

- {[`Quantity`](#Quantity)} `currentBlock` - number of the most-recent block synced
- {[`Quantity`](#Quantity)} `highestBlock` - number of latest block on the network
- {[`Quantity`](#Quantity)} `startingBlock` - block number at which syncing started



```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
// When syncing
{
  "id":1,
  "jsonrpc": "2.0",
  "result": {
    startingBlock: '0x384',
    currentBlock: '0x386',
    highestBlock: '0x454'
  }
}
// Or when not syncing
{
  "id":1,
  "jsonrpc": "2.0",
  "result": false
}
```



#### eth_mining

#### Description 

Determines if this client is mining new blocks

#### Parameters 

*(none)*

#### Returns 

{`boolean`} - `true` if this client is mining or `false` if it is not mining

#### Example

Request

```
#Request
curl -X POST  -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_mining","params":[],"id":67}' http://rpc-tokyo.ttcnet.io

#Response
//if client is actively mining new blocks. 
{"jsonrpc":"2.0","id":67,"result":true}

//if client is not mining new blocks. 
{"jsonrpc":"2.0","id":67,"result":false}
```



