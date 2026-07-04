# HongCoin ICO Recovery: Eligible Addresses

The HongCoin ICO contract (`0x9fa8fa61a10ff892e4ebceb7f4e0fc684c2ce0a9`) had 1,003 ETH stuck since 2016 due to a refund logic bug. The path to unblock it was disclosed to the project team, who executed the 41 unlock transactions on-chain on May 27, 2026.

The 46 addresses below are now eligible to call `refundMyIcoInvestment()` on the contract and recover exactly the ETH they originally deposited in 2016.

*\* 2 additional addresses already refunded for a total of 96.5 ETH.*

## How to claim

From the address listed below (you'll need its private key / hardware wallet), send a single transaction:

- **To**: `0x9fa8fa61a10ff892e4ebceb7f4e0fc684c2ce0a9`
- **Value**: 0 ETH
- **Data**: `0xe84f7054` (selector for `refundMyIcoInvestment()`)
- **Gas**: ~60,000

Or with [foundry](https://book.getfoundry.sh/):

```bash
cast send 0x9fa8fa61a10ff892e4ebceb7f4e0fc684c2ce0a9 \
  "refundMyIcoInvestment()" \
  --keystore /path/to/your/keystore.json \
  --rpc-url https://eth.llamarpc.com
```

The contract will send your ETH directly to your address.

## Eligible addresses (46 total, ~905 ETH refundable)

| Address | Refundable |
|---|---|
| `0x30d1f87561af86d5ab7b9ec04e65607fab61833d` | 0.0700 ETH |
| `0x8a71e8ffa962b2947402737a7ccdf382cb12bfda` | 0.1000 ETH |
| `0x94964bdf167cd9729df470ca75f56c087eaefda8` | 0.2000 ETH |
| `0x3828104cb3d7a22c08f0439fac16b82931630425` | 0.2500 ETH |
| `0x01dca3cf6c886c774e043f2ff8ec8c6790e18fcc` | 0.5000 ETH |
| `0x252162b54813cb291c5531b1668fe567ffcde7b9` | 0.9000 ETH |
| `0x7543a0b6d278618e9ec3cf4a376905a224c51291` | 0.9900 ETH |
| `0x409dc2674c4c99c34600bf16c93f06bf97fa98fc` | 1.0000 ETH |
| `0x8fe7563bd0e8c832e8047bf592c2646df63b94f3` | 1.0000 ETH |
| `0xa175e1aa0860c128a4e2a8c7228fd156156b8b20` | 1.0000 ETH |
| `0xb26477286af3ea304deb2f22f674418ad4a4a7cf` | 1.0000 ETH |
| `0xb8382f6298f01b4eebfda0e2bb2dde60250af575` | 1.0000 ETH |
| `0xcf0432166457bc96bf3e026d0fbef083fedd3ee3` | 1.9999 ETH |
| `0xbcd3827fee55bd0491db0b8cef39be50e6d11cc6` | 2.0000 ETH |
| `0xf3fe51fde34413c73318b9c85437fe7e820f561a` | 2.0000 ETH |
| `0x59670678bb9f4ca8122b57cd7b9bc0f22ba5702a` | 2.0000 ETH |
| `0xf8c41fce8460a392d22f441835e653d2f9fa8caa` | 2.0000 ETH |
| `0x58fc7816cc938192c19c11abc2fc0b1979ce7b5c` | 3.0000 ETH |
| `0xf2da5add7c6f8a47997efa04049ee7888542744b` | 4.0000 ETH |
| `0x2fcfc0eacb685de30e78745277ebe164a1fc1c2f` | 5.0000 ETH |
| `0x6abdc251e5e99b628ea4261d178e2661f59b90ea` | 7.0000 ETH |
| `0x35b55a2a0d4b915b33f88d74885b518fd247fc7b` | 9.9834 ETH |
| `0x04930ccd1e3977a1b45788ae99c8b76daacb7a82` | 10.0000 ETH |
| `0x58ad4fd70d88c630a8844817c37de5b0355086d8` | 10.0000 ETH |
| `0x521dabfd2c8b76deac89d44222bd3f75f388a2ec` | 10.4000 ETH |
| `0x267148fd72c54f620a592fb92799319cc4532b5c` | 12.0000 ETH |
| `0x271d65fb7ff503e675b3ed7f663839caade51312` | 12.0000 ETH |
| `0x32be343b94f860124dc4fee278fdcbd38c102d88` | 18.9939 ETH |
| `0x10eb123404632ee42eb491b06982d0f200d748bf` | 20.0000 ETH |
| `0x2cecea38eff2085ac7b13c9b53173141a2f4d7dc` | 20.0000 ETH |
| `0xe7bafa7bd2491632c9dc33bec87372218277a2ce` | 24.9458 ETH |
| `0x61377598304bfe4f08d90ba2e1dae92d7a35c121` | 25.0482 ETH |
| `0xeadb8adf68579f6fbc23bdfe3b98bdd87a4cf105` | 25.9965 ETH |
| `0x46a75e570420572c3ca38486fda26b57d1823dae` | 49.9800 ETH |
| `0x9bd905f1719fc7acd0159d4dc1f8db2f21472338` | 50.0000 ETH |
| `0x549a07fde06eb5aee4f5d866a146797e0656ab28` | 50.0000 ETH |
| `0xbf50ce2e264b9fe2b06830617aedf502b2351b45` | 50.0000 ETH |
| `0x0f50d774d0836df20257475036cb058a1ffc005a` | 65.0000 ETH |
| `0x1d2e25d9a1f7e41f2dcf81beda35d39f10f4d7b8` | 85.1021 ETH |
| `0x700365b641f5bf7c2af8fc046a85e8c96b9658fd` | 100.1020 ETH |
| `0x5a792cec3bea929a50db44623407223d80347533` | 201.9960 ETH |
| `0xe5e75746215f25a4c291dd4e468048d1742b9261` | 0.2000 ETH |
| `0xc3d6eea27e43ae6017086bced4746ab9a5efbef5` | 9.9864 ETH |
| `0x1b3f59c28f5c6ffc10e36ce7f2ed1b8a18640376` | 1.5000 ETH |
| `0x24ef064d742bb91bd0397bc11071882d60c97003` | 1.8800 ETH |
| `0x1dd4b76c22cbbb03d5eba7ed8d21dc7f6ef3b5be` | 3.0000 ETH |

## Need help?

If one of these is yours and you're not comfortable signing the transaction yourself, reach out:

- X / Twitter: [@0xFlorent_](https://twitter.com/0xFlorent_)
- ENS: `0xflorent.eth`
