# HUETON JavaScript Provider

## Summary

HUETON mobile wallet [application](https://apps.apple.com/app/id1629214799) has it's own in-app dApp browser with TON JavaScript provider to help users get a best expearance with TON dApps on it's mobile devices. To do this we are super exited to work with each dApp developer and brings some userful tools.

## Developers UX

Plugin API based [EIP-1193](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1193.md), [EIP-1102](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1102.md). Also it's very similar to [Metamask API](https://metamask.github.io/metamask-docs/guide/ethereum-provider.html#new-api) and [TON Wallet Plugin API](https://github.com/toncenter/dapp-example/blob/master/README.md). When you use this JavaScript provider - you don't use any of centralised servers to communicate with TON, our [application](https://apps.apple.com/app/id1629214799) going to the decentralised network for you. We hope it will be easy to use.

## API

### Summary

HUETON JavaScript Provider injects in page `window.ton` ([as TON Wallet](https://github.com/toncenter/dapp-example/blob/master/README.md)) object before any script or content be loaded.

```js
interface HUETON {
  
  // The current type of `window.ton` object, e.g. `HUETON`
  type: string;
  
  // The function to send and retrieve information about user
  function send(method: string, parameters?: any[]): Promise<any>;
  
  // To receive an updates from provider and user
  function on(event: string, listener: (result: any) => void);
  
  // To stop receiving updates from provider and user
  function removeListener(event: string, listener: (result: any) => void);
}
```
> **_NOTE:_** To determinate type of current `window.ton` object, simply check `window.ton.type` - it should be equal to `'HUETON'` string.

### Supported methods

#### - `ton_requestAccounts` - returns an array of a single, TON address string.

> **_NOTE:_** Will be deprecated in future because of `ton_requestWallets` method.

```js
function requestAccounts() {
  window.ton
    .send('ton_requestAccounts')
    .then(handleUserAccounts)
    .catch((error) => {
        console.error(error);
    });
}
```

#### - `ton_requestWallets` - returns an array of a `Wallet` object.

```js
interface Wallet {
  
  // The user wallet address
  address: string
  
  // The use wallet public key
  publicKey: string
  
  // The user wallet version, e.g. `v3R2`
  walletVersion: string
}

function requestWallets() {
  window.ton
    .send('ton_requestWallets')
    .then(handleUserWallets)
    .catch((error) => {
        console.error(error);
    });
}
```

#### - `ton_getBalance` - return user's balance string in nanotons.

```js
function getBalance() {
  window.ton
    .send('ton_getBalance')
    .then(handleUserBalance)
    .catch((error) => {
        console.error(error);
    });
}
```

#### - `ton_sendTransaction` - send transaction to TON network
```js
interface SendTransactionParameters {
  
  // The value of transaction, nanotons
  value: string;

  // The receipt address
  to: string;

  // The type of custom payload
  // (text | hex | base64 | boc) (boc: base64 encoded, hex representation of BOC)
  dataType?: string;

  // The custom payload of transaction
  data?: string;
  
  // Additional state init for transaction, boc: base64 encoded, hex representation of BOC
  stateInit?: string;
}

const parameters = {
    value: "1000000000", 
    to: string, 
    data: string
};

function connect() {
  window.ton
    .send('ton_sendTransaction', parameters)
    .then(handleWalletsChange)
    .catch((error) => {
      if (error.code === 4001) {
        // EIP-1193 userRejectedRequest error
        console.log('Please connect to HUETON.');
      } else {
        console.error(error);
      }
    });
}
```

### Supported events

#### - `accountsChanged` - called when user did change it's currently selected account
```js
type AccountsChangedData = string[];

function connect() {
  window.ton
    .on('accountsChanged', (accounts) => {
        // accounts: AccountsChangedData
        console.log(accounts)
    });
}
```

## From Us

You always free to open issues and make TON better.