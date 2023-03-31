# [Dash Tools SDK](https://github.com/dashhive/dash-tools)

Meta repo for various Dash Tools

## Dash SDK Core

Production-Ready Reference Implementations. \
Building blocks for all future "DASH Core" and "DASH Platform" tools.

- [@dashincubator/secp256k1.js][secp256k1-js] - A Browser-Friendly fork of
  `@noble/secp256k1.js` \
  (for Private & Public Keys and X Key Tweaking)
- [dashkeys.js][dashkeys-js] - WIF & Addr, and other Key conversions \
  (Base-X, Base58, Base58Check, & RIPEMD160)
- [dashphrase.js][dashphrase-js] - Generate Dash HD Wallet Passphrases & Seeds \
  (BIP-39 compatible)
- [dashhd.js][dashhd-js] - HD Wallet derivations and conversions \
  (HD Wallet, Account, X Keys - XPrv & XPub, and HD Addresses)

[secp256k1-js]: https://github.com/dashhive/Secp256k1.js
[dashkeys-js]: https://github.com/dashhive/DashKeys.js
[dashphrase-js]: https://github.com/dashhive/DashPhrase.js
[dashhd-js]: https://github.com/dashhive/DashHD.js

## Developing

Clone all the code in at once:

```bash
git clone --recursive https://github.com/dashhive/dash-tools
```

For the convenience of the casual git user, these submodules use
(unauthenticated) https.

If you'd like to develop and contribute, update your git config to automatically
substitute the proper ssh url automatically:

```bash
git config --global \
  url."ssh://git@github.com/dashhive/".insteadOf \
  "https://github.com/dashhive/"
```

## Writing Examples

When writing examples for Dash, these are the values that should be used:

| Placeholder Type                   | Placeholder / Example Value                                                                                                                          |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Input Entropy                      | `ffffffffffffffff`                                                                                                                                   |
| Wallet Passphrase <br> (mnemonic)  | `zoo zoo zoo zoo zoo zoo zoo zoo zoo zoo zoo wrong`                                                                                                  |
| Wallet Secret <br> (mnemonic salt) | Empty string or `Trezor`                                                                                                                             |
| Wallet Seed                        | `ac27495480225222079d7be181583751 <br> e86f571027b0497b5b5d11218e0a8a13 <br> 332572917f0f8e5a589620c6f15b11c6 <br> 1dee327651a14c34e18231052e48c069` |

<!--
| Mainnet xPrv | `` |
| Mainnet xPub | `` |
| Testnet tPrv | `` |
| Testnet tPub | `` |
| HD Path | `` |
| WIF | `` |
| PayAddr | `` |
-->

## Glossary


[bip-20]: https://github.com/bitcoin/bips/blob/master/bip-0020.mediawiki
[bip-21]: https://github.com/bitcoin/bips/blob/master/bip-0021.mediawiki
[bip-70]: https://github.com/bitcoin/bips/blob/master/bip-0070.mediawiki
[bip-71]: https://github.com/bitcoin/bips/blob/master/bip-0071.mediawiki
[bip-72]: https://github.com/bitcoin/bips/blob/master/bip-0072.mediawiki
[json-payment-protocol]: https://bitpay.com/docs/payment-protocol

- Base2048
  - A standard for encoding bytes (+ tiny checksum) as "_mnemonic_" words. \
    There are 2048, easy-to-spell, easy-to-speak words. \
    Each word represents 11 bits (2^11 = 2048). \
    Any extra bits are used for a checksum. \
    (128 bits of data require 12 words - 132 bits - thus 4-bit checksum) \
    The canonical example "_input entropy_" bytes are `ffffffffffffffff`. \
    The canonical example "_mnemonic_" phrase is
    `zoo zoo zoo zoo zoo zoo zoo zoo zoo zoo zoo wrong`. \
    (`zoo` contains 11 bits of all 1s, which is the last word in the dictionary)
    (`wrong` contains 7 bits of data (1s), and 4 bits of checksum)
- Base58
  - A "_Base-X_" derivative with a human-friendly, unambiguous alphabet: \
    `"123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz"` \
    (doesn't have problem with `1` vs `l` vs `I` or `0` vs `O`, etc)
- Base58Check
  - Base58-encoded version, data, and checksum. \
    (typically encoding PubKeyHash to PayAddr, PrivKey to WIF, xPrv, or xPub) \
    In the format of `{version}{massaged-data}{checksum}`. \
    It starts with magic `version` byte(s) (see Magic Bytes). \
    It ends with the first 4 bytes of a `sha256(sha256(data))` as a checksum .\
- Base-X
  - A well-known algorithm for arbitrary bases. \
    Not compatible with standardized Base64, Base32, Hex etc. \
    (pretty much only used for Base58 / Base58Check)
- BIP-0020, BIP-20, BIP20, BIP-0021, BIP-21, BIP21
  - See _Payment URLs (single)_
- BIP-0039, BIP-39, BIP39
  - A standard for generating HD wallet "seeds" and "cold wallets". \
    "_Input Entropy_" is used as a single source for multiple wallets and keys. \
    The "_Input Entropy_" is _Base2048_ encoded as a "mnemonic". \
    (the padding/checksum is the first bits of a _SHA256_ of the data) \
    However, the "_mnemonic_" is canonical (even if the checksum fails). \
    The "mnemonic" is directly used as a "PBKDF2 password". \
    By default, the literal word 'mnemonic' is used as the "PBKDF2 salt". \
    A (misnomer) "passphrase" can be appended after 'mnemonic' as a salt. \
    In this way, a single "mnemonic" can produce infinite "_seeds_". \
    (only the correct full salt will reveal the correct "seeds") \
- BIP-0039 Mnemonic
  - A list of space-separated Base2048 words. \
    What we ([correctly][correct-horse]) call a "Wallet Passphrase". \
    Note: it's _valid_ to use a mnemonic that doesn't pass checksums. \
    (and all software should allow this, with a warning, for historical keys)
- BIP-0039 Password
  - **Misnomer**. This is the "_mnemonic_" list of words (passphrase)
- BIP-0039 Passphrase
  - **Misnomer**. This is the secret part of the PBKDF2 salt.
- BIP-0070, BIP-70, BIP70, BIP-0071, BIP-71, BIP71, BIP-0072, BIP-72, BIP72
  - See _Payment URLs (merchant)_
- Coin
  - See UTXO.
- Cold Storage / Cold Wallet
  - A mnemonic passphrase that you keep offline. \
    Perhaps written down on paper in a safe. \
    Perhaps on a device (e.g. Trezor).
- Compression Flag
  - Also: Compressed Byte, Recovery Bit, Quadrant Byte \
    Public Keys have X and Y values (32-bytes each). \
    A "compressed" key stores the X value and a "recover" bit. \
    Since both possible Y values can be generated from an X, \
    the "recovery bit" can tell us _which_ to use: \
    0x02 if Y is even. \
    0x03 if Y is odd.
- Input Entropy
  - The initial cryptographically random bytes used for generating a BIP-0039
    "mnemonic".
- HD Path
  - A string that produces XPrv and XPub keys, or WIFs and PayAddrs A string
    that looks like `m/44'/5'/0'/0/0`. \
    The string is relative to a "_Seed_". \
    The string is split into its component parts. \
    `m/` or `m'/` is ignored (just a _magic byte_ to denote HD Path) \
    There are two algorithms used (Hardened and Non-Hardened). \
    `'` denotes a "_Hardened Path_". \
    `44/` or `44'/` as a _magic byte_ referring to BIP*0044 (HD Path) \
    `5'/` is the Dash HD coin type (same for mainnet and testnet) \
    `0'/` increments for each *HD Account* (a.k.a. *HD Wallet*) \
    The next component specifies how derived Payment Addresses are used. \
    This is the level at which XPrv and XPub keys are produced. \
    `0/` is for "*external*" (receiving payments).\
    `1/` is for "*internal*" (change) \
    The final component specifies the WIF or Payment Address index Technically, a
    path \_could* go on for ever (just more iterations)
- HD Path (hardened), Hardened Path, Unhardened Path
  - `'` is used to denote a "hardened" path. \
    The Hardened derivation algorithm produces child keys that are not related. \
    The Non-Hardened derivation algorithm produces related child keys. \
    If related Private Keys are leaked together, they may reveal their parent. \
    ALL Hardened Path components should come before Non-Hardened components. \
    For these paths a different algorithm is used that produces unrelated keys
- HD Wallet
  - **Ambiguous**. Any of: \
    an XPrv / XPub key \
    an XPrv / XPub generated from a "_seed_" and "_HD Account_" HD Path \
    the Passphrase used
- Lock Script, lockscript, subscript
  - A series of _OP Codes_ and data that define how to spend coins (_UTXOs_).
- Magic Bytes
  - A sequence of arbitrarily chosen bits used to identify or categorize. \
    (to distinguish different coins, protocols, mainnet vs testnet, etc) \
    `5'/` for Dash HD Path coin type \
    `0xcc` (204) for Dash mainnet Base58Check WIFs \
    `0x4c` (76) for Dash mainnet Base58Check Payment Addresses \
    `0xef` (239) for Dash testnet Base58Check WIFs \
    `0xec` (236) for Dash testnet Base58Check Payment Addresses \
    `0x0488ade4` (76066276), `xprv` as Base58 mainnet (same for all coins) \
    `0x0488b21e` (76067358), `xpub` as Base58 mainnet (same for all coins) \
    `0x04358394` (70615956), `tprv` as Base58 testnet (different for some) \
    `0x043587CF` (70617039), `tpub` as Base58 testnet (different for some) \
    `0x03` (3), `0x03000000` (LE) `version` for Dash Blockchain Transactions
- Mnemonic Passphrase
  - The the list of 12 or 24 words. \
    Referred to by BIP-0039 as both terms "mnemonic" and "password".
- Mnemonic Salt
  - The secret or password used to create a pairwise seed from the mnemonic. \
    (literally 'mnemonic'+_mnemonic salt_ is the the _PBKDF2 salt_) \
    Referred to by BIP-0039 as (misnomer) "passphrase"
- Pairwise, Pairwise Entropy
  - A small amount of data that cause a wildly different hash result. \
- Paper Wallet
  - A WIF, typically encoded as a QR Code, on a piece of paper (or PNG)
- Payment Address, PayAddr
  - A Base58Check-encoded PubKeyHash. \
    In the format of `{version}{pubKeyHash}01{checksum}`
- Payment URLs (Single)
  - `dash:<address>[?amount=<amount>][?label=<label>][?message=<message>]` \
    See also [BIP 21][bip-21] (previously [BIP 20][bip-20])
- Payment URL Protocol (Merchant)
  - `dash:<address>?amount=<decimal>&r=<merchant-request-url>` \
    `dash:XrZJJfEKRNobcuwWKTD3bDu8ou7XSWPbc9?amount=1.1&r=https://example.com?q%3Dxxxxxxxx` \
    `dash:?r=<merchant-request-url>` \
    `dash:?r=https://example.com?q%3Dxxxxxxxx` \
    The payload is BIP-spec'd as binary, but bitpay introduced a JSON version.
    See also [JSON Payment Protocol][json-payment-protocol], [BIP 72][bip-72] (includes [BIP 70][bip-70], [71][bip-71])
- PayToPubKeyHash, p2pkh, Pay To Public Key Hash
  - TODO
- PBKDF2
  - A suite of "_Key Derivation_" algorithms, originally for authentication. \
    The "password" is the primary source of entropy (i.e. user's password). \
    The "salt" creates pairwise entropy (to prevent rainbow tables). \
    Those are recursively hashed for a number of "rounds" to increase CPU time. \
    The "psuedo-random" function performs the hash, typically a "SHA-2" algo. \
- PBKDF2 password
  - The primary entropy source, in this case the Wallet Passphrase. \
    (which is described by BIP-0039 as "_mnemonic_ sentence")
- PBKDF2 salt
  - The pairwise entropy source, the "\_mnemonic salt". \
    Literally 'mnemonic' + the Wallet Passphrase's password, or secret. \
- Private Key, PrivateKey, PrivKey
  - This is any 128-bits of data that can be used to compute a Public Key. \
    (generate a random number until computing its Public Bey doesn't throw). \
    (but it will almost never throw - unless it's closer to 1 than to 2^128). \
    It's what you use to create a Payment Address and spend "coins" (UTXOs). \
    (if you lose this, that money can _never_ be spent, _ever_) \
- Public Key (compressed)
  - The 128-bit X value (and 1-bit Y quadrant) derived from a Private Key. \
    Any 'X' will have exactly two 'Y's on a elliptic curve. \
    A single bit can tell you which Y is the correct one. \
- Public Key (uncompressed)
  - 128-bit X and full 128-bit Y value derived from a Private Key. \
    **DO NOT USE**. Some libraries produce uncompressed keys by default.
- Public Key Hash, PubKeyHash, PKH
  - A special 20-byte hash of a Compressed Public Key. \
    SHA-256 of the raw Public Key bytes. \
    RIPEMD160 of the SHA-256. \
    RIPEMD160 does make "_lock scripts_" vulnerable to "birthday attacks". \
    Fortunately it's reasonably costly and can't target a specific key. \
    Regardless, we're stuck with it for now.
- Recovery Bit
  - See _Compression Flag_.
- Seed
  - Entropy used to create an XPrv key, typically for with HD Wallet path. \
    (typically PBKDF2-derived from a _mnemonic passphrase_, as per BIP-0039)
- SHA-2
  - A hash algorithm suite. \
    (not related to SHA-1 or SHA-3 by math - just by standards body name)
- SHA-256, SHA256, (part of SHA-2)
  - A 32-bytes (256-bit) hash. \
    Used for the checksum/padding of "Input Entropy" for Base2048-encoded data.
- SHA-512, SHA512, (part of SHA-2)
  - A 64-byte (512-bit) hash. \
    Used as HMAC-SHA512 for PBKDF2 rounds.
- UTXO
  - Unspent Transaction Outputs ("coins") that have not been spent / unlocked. \
    A _Lock Script_ that has not yet been unlocked. \
    Transaction ID \
    TODO
- Wallet
  - **Ambiguous**. Due to old terminology and market usage, this means any of: \
    A private key or WIF \
    A seed used for generating private keys or WIFs \
    An HD "account", generated from a Wallet Passphrase (a.k.a. "_mnemonic_") \
    A Wallet Passphrase (a.k.a. "_mnemonic_")
- Wallet Passphrase
  - The list of 12 or 24 words used to create HD Wallet "accounts".
- WIF
  - A Base58Check-encoded private key
- Word List
  - **Misnomer** This refers to the dictionary used for Base2048 encoding. \
    It is **_not_**, in fact, the list of words of the "_mnemonic_" passphrase.
- xPrv, eXtended Private Key, tprv
  - Base58Check-encoded "_Wallet Seed_" used for generating WIFs
  - In the format of `{version}{extendedPrivateKey}{checksum}`
- xPrv, eXtended Public Key, tpub
  - Base58Check-encoded Payment Address generator key
  - In the format of `{version}{extendedPublicKey}{checksum}`

[correct-horse]: https://xkcd.com/936/
