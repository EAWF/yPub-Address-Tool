# Key Extractor Tool
## The Exported Account-Level Extended Public Key
* An exported account-level extended public key contains all of the basic information necessary for:
  - Verification of the exported public key when:
    - it is transmitted electronically via the Internet so as to avoid any possibility of Man-In-The-Middle attacks
    - humans copy the key manually
  - creating wallet receive addresses for:
    - incoming payments
    - change returning from a sent payment transaction
* The exported account-level extended public key data is encoded/encapsulated with the Base58Check algorithm.
  - Running the Extended Public Key through the Base58 decoding algorithm results in a binary data stream which must be converted to hexadecimal and thereafter makes the following data fields, formatted in the [BIP32 Serialization Format](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#serialization-format), available for use:
    - Type(4 bytes) - This directly translates to the [SLIP-0132](https://github.com/satoshilabs/slips/blob/master/slip-0132.md#registered-hd-version-bytes) registered coin types.
      - NOT USED IN THE DERIVATION PROCESS
    - Depth(1 byte) - This shows the level of the derivation path which has already been performed on the exported extended public key.
      - The Derivation Path Levels are defined [here](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki#Path_levels)
      - Here are some examples that hopefully help you to understand how the depth field works:
![Depth examples](https://github.com/EAWF/Bitcoin-Merchants-Toolbox/blob/master/Images/DepthExamples.jpg)
      - As you can see, for an exported account-level extended public key from an HD wallet, the depth will always be 03, and as the previous derivation does not affect the following derivation, for all intents and purposes, the Account can safely be ignored when deriving change or payment addresses from it.
    - Parent Fingerprint (4 bytes) - As it's not necessary for our purposes here, it's ignored.
    - Child (aka Account) Number (4 bytes) - **Always** hardened (0x800000xx) with account-level extended public keys and ***DO NOT USE for derivation as you may lose funds!***.
    - Parent Chain Code (32 bytes) - **Required for Derivation**
    - Parent Public Key (33 bytes) - **Required for Derivation**
    - Base58 Checksum (4 bytes)- *ONLY needed for verification when extended public key is transmitted via electronic means.*
    