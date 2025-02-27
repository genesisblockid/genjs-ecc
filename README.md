[![NPM](https://img.shields.io/npm/v/eosjs-ecc.svg)](https://www.npmjs.com/package/genjs-ecc)
[![Build Status](https://travis-ci.org/EOSIO/eosjs-ecc.svg?branch=master)](https://travis-ci.org/EOSIO/eosjs-ecc)

# Elliptic curve cryptography functions (ECC)

Private Key, Public Key, Signature, AES, Encryption / Decryption

# Import

```js
import ecc from "genjs-ecc";
// or
const ecc = require("genjs-ecc");
```

# Include

- Install with: `yarn add genjs-ecc`
- Html script tag, see [releases](https://github.com/genesisblockid/genjs-ecc/releases) for the correct **version** and its matching script **integrity** hash.

```html
<html>
  <head>
    <meta charset="utf-8" />
    <!--
  sha512-cL+IQQaQ586s9DrXfGtDheRpj5iDKh2M+xlpfwbhNjRIp4BGQ1fkM/vB4Ta8mc+f51YBW9sJiPcyMDIreJe6gQ== lib/genjs-ecc.js
  sha512-dYFDmK/d9r3/NCp6toLtfkwOjSMRBaEzaGAx1tfRItC0nsI0hVLERk05iNBQR7uDNI7ludYhcBI4vUiFHdjsTQ== lib/genjs-ecc.min.js
  sha512-eq1SCoSe38uR1UVuQMwR73VgY8qKTBDc87n2nIiC5WLhn1o2y1U6c5wY8lrigVX7INM8fM0PxDlMX5WvpghKig== lib/genjs-ecc.min.js.map
  -->
    <script
      src="https://cdn.jsdelivr.net/npm/genjs-ecc@4.0.4/lib/genjs-ecc.min.js"
      integrity="sha512-dYFDmK/d9r3/NCp6toLtfkwOjSMRBaEzaGAx1tfRItC0nsI0hVLERk05iNBQR7uDNI7ludYhcBI4vUiFHdjsTQ=="
      crossorigin="anonymous"
    ></script>
  </head>
  <body>
    See console object: genjs_ecc
  </body>
</html>
```

# Common API

<!-- generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

- [wif](#wif)
- [ecc](#ecc)
  - [initialize](#initialize)
  - [unsafeRandomKey](#unsaferandomkey)
  - [randomKey](#randomkey)
    - [Parameters](#parameters)
    - [Examples](#examples)
  - [seedPrivate](#seedprivate)
    - [Parameters](#parameters-1)
    - [Examples](#examples-1)
  - [privateToPublic](#privatetopublic)
    - [Parameters](#parameters-2)
    - [Examples](#examples-2)
  - [isValidPublic](#isvalidpublic)
    - [Parameters](#parameters-3)
    - [Examples](#examples-3)
  - [isValidPrivate](#isvalidprivate)
    - [Parameters](#parameters-4)
    - [Examples](#examples-4)
  - [sign](#sign)
    - [Parameters](#parameters-5)
    - [Examples](#examples-5)
  - [signHash](#signhash)
    - [Parameters](#parameters-6)
  - [verify](#verify)
    - [Parameters](#parameters-7)
    - [Examples](#examples-6)
  - [recover](#recover)
    - [Parameters](#parameters-8)
    - [Examples](#examples-7)
  - [recoverHash](#recoverhash)
    - [Parameters](#parameters-9)
  - [sha256](#sha256)
    - [Parameters](#parameters-10)
    - [Examples](#examples-8)
- [pubkey](#pubkey)

## wif

[Wallet Import Format](https://en.bitcoin.it/wiki/Wallet_import_format)

Type: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)

## ecc

### initialize

Initialize by running some self-checking code. This should take a
second to gather additional CPU entropy used during private key
generation.

Initialization happens once even if called multiple times.

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)**

### unsafeRandomKey

Does not pause to gather CPU entropy.

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;PrivateKey>** test key

### randomKey

#### Parameters

- `cpuEntropyBits` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** gather additional entropy
  from a CPU mining algorithm. This will already happen once by
  default. (optional, default `0`)

#### Examples

```javascript
ecc.randomKey().then((privateKey) => {
  console.log("Private Key:\t", privateKey); // wif
  console.log("Public Key:\t", ecc.privateToPublic(privateKey)); // VEXkey...
});
```

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;[wif](#wif)>**

### seedPrivate

#### Parameters

- `seed` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** any length string. This is private. The same
  seed produces the same private key every time. At least 128 random
  bits should be used to produce a good private key.

#### Examples

```javascript
ecc.seedPrivate("secret") === wif;
```

Returns **[wif](#wif)**

### privateToPublic

#### Parameters

- `wif` **[wif](#wif)**
- `pubkey_prefix` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** public key prefix (optional, default `'VEX'`)

#### Examples

```javascript
ecc.privateToPublic(wif) === pubkey;
```

Returns **[pubkey](#pubkey)**

### isValidPublic

#### Parameters

- `pubkey` **[pubkey](#pubkey)** like VEXKey..
- `pubkey_prefix` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** (optional, default `'VEX'`)

#### Examples

```javascript
ecc.isValidPublic(pubkey) === true;
```

Returns **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** valid

### isValidPrivate

#### Parameters

- `wif` **[wif](#wif)**

#### Examples

```javascript
ecc.isValidPrivate(wif) === true;
```

Returns **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** valid

### sign

Create a signature using data or a hash.

#### Parameters

- `data` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Buffer](https://nodejs.org/api/buffer.html))**
- `privateKey` **([wif](#wif) | PrivateKey)**
- `encoding` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** data encoding (if string) (optional, default `'utf8'`)

#### Examples

```javascript
ecc.sign("I am alive", wif);
```

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** string signature

### signHash

#### Parameters

- `dataSha256` **([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Buffer](https://nodejs.org/api/buffer.html))** sha256 hash 32 byte buffer or string
- `privateKey` **([wif](#wif) | PrivateKey)**
- `encoding` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** dataSha256 encoding (if string) (optional, default `'hex'`)

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** string signature

### verify

Verify signed data.

#### Parameters

- `signature` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Buffer](https://nodejs.org/api/buffer.html))** buffer or hex string
- `data` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Buffer](https://nodejs.org/api/buffer.html))**
- `pubkey` **([pubkey](#pubkey) | PublicKey)**
- `encoding` (optional, default `'utf8'`)
- `hashData` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** sha256 hash data before verify (optional, default `true`)

#### Examples

```javascript
ecc.verify(signature, "I am alive", pubkey) === true;
```

Returns **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)**

### recover

Recover the public key used to create the signature.

#### Parameters

- `signature` **([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Buffer](https://nodejs.org/api/buffer.html))** (VEXbase58sig.., Hex, Buffer)
- `data` **([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Buffer](https://nodejs.org/api/buffer.html))** full data
- `encoding` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** data encoding (if data is a string) (optional, default `'utf8'`)

#### Examples

```javascript
ecc.recover(signature, "I am alive") === pubkey;
```

Returns **[pubkey](#pubkey)**

### recoverHash

#### Parameters

- `signature` **([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Buffer](https://nodejs.org/api/buffer.html))** (VEXbase58sig.., Hex, Buffer)
- `dataSha256` **([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Buffer](https://nodejs.org/api/buffer.html))** sha256 hash 32 byte buffer or hex string
- `encoding` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** dataSha256 encoding (if dataSha256 is a string) (optional, default `'hex'`)

Returns **PublicKey**

### sha256

#### Parameters

- `data` **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Buffer](https://nodejs.org/api/buffer.html))** always binary, you may need Buffer.from(data, 'hex')
- `resultEncoding` (optional, default `'hex'`)
- `encoding` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** result encoding 'hex', 'binary' or 'base64' (optional, default `'hex'`)

#### Examples

```javascript
ecc.sha256("hashme") === "02208b..";
```

```javascript
ecc.sha256(Buffer.from("02208b", "hex")) === "29a23..";
```

Returns **([string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String) \| [Buffer](https://nodejs.org/api/buffer.html))** Buffer when encoding is null, or string

## pubkey

VEXKey..

Type: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)

# Usage (Object API)

```js
let {
  PrivateKey,
  PublicKey,
  Signature,
  Aes,
  key_utils,
  config,
} = require("genjs-ecc");

// Create a new random private key
let privateWif;
PrivateKey.randomKey().then((privateKey) => (privateWif = privateKey.toWif()));

// Convert to a public key
pubkey = PrivateKey.fromString(privateWif).toPublic().toString();
```

- [PrivateKey](./src/key_private.js)
- [PublicKey](./src/key_public.js)
- [Signature](./src/signature.js)
- [Aes](./src/aes.js)
- [key_utils](./src/key_utils.js)
- [config](./src/config.js)

# Browser

```bash
git clone https://github.com/genesisblockid/genjs-ecc.git
cd genjs-ecc
yarn
yarn build_browser
# builds: ./dist/genjs-ecc.js
# Verify release hash
```

```html
<script src="genjs-ecc.js"></script>
```

```js
var ecc = genjs_ecc;

ecc.randomKey().then((privateWif) => {
  var pubkey = ecc.privateToPublic(privateWif);
  console.log(pubkey);
});
```
