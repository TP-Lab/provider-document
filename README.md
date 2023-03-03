# TokenPocket Providers API (Draft)

<!-- vscode-markdown-toc -->
* [TokenPocketProvider](#tokenpocketprovider)
    * [Interfaces](#interfaces)
* [EVM Provider](#evm-provider)
    * [Interfaces](#interfaces-1)
* [APTOS Provider](#aptos-provider)
    * [Interfaces](#interfaces-2)
* [TRON Provider](#tron-provider)
    * [Interfaces](#interfaces-3)
    * [Methods](#methods)
        * [1. tron_requestAccounts / eth_requestAccounts](#1.-tron_requestaccounts-/-eth_requestaccounts)
* [BTC & Forked Provider](#btc-&-forked-provider)
    * [Interfaces](#interfaces-4)
    * [Methods](#methods-1)
        * [1. btc_accounts](#1.-btc_accounts)
        * [2. btc_signMessage](#2.-btc_signmessage)
        * [3. btc_chainId](#3.-btc_chainid)
* [Nostr (Under development)](#nostr-(under-development))
    * [Interfaces](#interfaces-5)

<!-- vscode-markdown-toc-config
	numbering=false
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

## <a name='tokenpocketprovider'></a>TokenPocketProvider

### <a name='interfaces'></a>Interfaces

```typescript
interface TokenPocketWindow extends Window {
    tokenpocket: TokenPocketProvider
}

interface TokenPocketProvider extends Window {
    ethereum?: EvmProvider
    tron?: TronProvider
    aptos?: AptosProvider
    bitcoin?: BitcoinProvider
    nostr?: NostrProvider
    // solana?: SolanaProvider
    // cosmos?: CosmosProvider
    // eos?: EosProvider
}
```

## <a name='evm-provider'></a>EVM Provider

Support ETH, BSC, POLYGON, OP, ARB, HECO, OKC and any EVM chains.

Implementation of [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193), [EIP-1102](https://eips.ethereum.org/EIPS/eip-1102), [EIP-3085](https://eips.ethereum.org/EIPS/eip-3085), [EIP-695](https://eips.ethereum.org/EIPS/eip-695), [EIP-1474](https://eips.ethereum.org/EIPS/eip-1474), [EIP-3326](https://eips.ethereum.org/EIPS/eip-3326)

### <a name='interfaces-1'></a>Interfaces

```typescript
interface RequestPayload {
    method: string;
    params?: unknown[] | object
}

interface EvmProvider {
    readonly isTokenpocket: true;
    request(payload: RequestPayload): Promise<RequestResponse>;
}

```


## <a name='aptos-provider'></a>APTOS Provider
Support APTOS Network


Implementation of [aptos-wallet-adapter](https://github.com/TP-Lab/aptos-wallet-adapter)

### <a name='interfaces-2'></a>Interfaces
```typescript
interface AptosProvider {
    readonly isTokenPocket: true
    connect(): Promise<AptosAccount>;
    account(): Promise<AptosAccount>;
    getChainId(): Promise<string | number>
    getNodeUrl(): Promise<string>
    network(): Promise<string>
    signAndSubmitTransaction(transaction: AptosTransaction, options: AptosTransactionOpt): Promise<AptosTransactionResponse>;
    signTransaction(transaction: AptosTransaction, options: AptosTransactionOpt): Promise<AptosSignedTransaction>;
    signMessage(messageObj: AptosMessage): Promise<AptosMessage>;
    request(payload: RequestPayload): Promise<RequestResponse>
}
```



## <a name='tron-provider'></a>TRON Provider

implementation of [TIP-1193](https://github.com/tronprotocol/tips/blob/master/tip-1193.md), [TIP-1102](https://github.com/tronprotocol/tips/blob/master/tip-1102.md)

Support TRON

### <a name='interfaces-3'></a>Interfaces
```typescript
interface RequestPayload {
    method: string;
    params?: unknown[] | object
}

interface TronProvider {
    readonly isTokenpocket: true;
    tronWeb: TronWeb;
    request(payload: RequestPayload): Promise<RequestResponse>;
}
```
### <a name='methods'></a>Methods

#### <a name='1.-tron_requestaccounts-/-eth_requestaccounts'></a>1. tron_requestAccounts / eth_requestAccounts

``` typescript
bitcoin.request({
    method: 'tron_requestAccounts'
}).then(console.log)

// return
['TKqQ7zuL7yKp1NWuhGpoXTDiE3j2qiAbRE']
```

## <a name='btc-&-forked-provider'></a>BTC & Forked Provider

Support BTC / DOGE ...

### <a name='interfaces-4'></a>Interfaces
```typescript

interface RequestPayload {
    method: string;
    params?: unknown[] | object
}


interface BitcoinProvider {
    readonly isTokenpocket: true;
    request(payload: RequestPayload): Promise<RequestResponse>;
}

```

### <a name='methods-1'></a>Methods

#### <a name='1.-btc_accounts'></a>1. btc_accounts

``` typescript
bitcoin.request({
    method: 'btc_accounts'
}).then(console.log)

// return

['39Px4rNdV1c1MzEKeVUQgQThbWAD4vfVHC']
```


#### <a name='2.-btc_signmessage'></a>2. btc_signMessage
```typescript
bitcoin.request({
    method: 'btc_signMessage',
    params: ['Hello world!', '39Px4rNdV1c1MzEKeVUQgQThbWAD4vfVHC']
}).then(console.log)

// return signature
"I9knbZ0oaUo7fl8mKBJPP4cTy9kOOdTed51EC4BbrfdJdMdqUKw5yX81J7nrDBbDlA/7adqhQr2Fdg8MRSbA1W8="
```

##### <a name='2.1-verification'></a>2.1 Verification

Using https://github.com/bitcoinjs/bitcoinjs-message to verify the signature

```typescript
var bitcoinMessage = require('bitcoinjs-message')

console.log(bitcoinMessage.verify(message, address, signature[, network.messagePrefix]))
// return true or false
```

#### <a name='3.-btc_chainid'></a>3. btc_chainId

Reference: [BIP-122](https://github.com/bitcoin/bips/blob/master/bip-0122.mediawiki)

``` typescript
bitcoin.request({
    method: 'btc_chainId'
}).then(console.log)

// return chainid 
'000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f'
```


## <a name='nostr-(under-development)'></a>Nostr (Under development)

Implementation of [NIP-07](https://github.com/nostr-protocol/nips/blob/master/07.md)

### <a name='interfaces-5'></a>Interfaces
```typescript
interface Nip04 {
    encrypt(pubkey: string, plaintext: string): Promise<string>
    decrypt(pubkey: string, ciphertext: string): Promise<string> 
}

interface Relays {
    [url: string]: {read: boolean, write: boolean}
}

interface Event {
    id: string;
    pubkey: string:
    created_at: string;
    kind: number;
    tags?: array;
    content: string;
    sig?: string; 
}

interface NostrProvider {
    readonly isTokenPocket: true;
    getPublicKey(): Promise<string>;
    signEvent(event: Event): Promise<Event>;
    getRelays(): Promise<Relays>
    nip04: Nip04
}
```






