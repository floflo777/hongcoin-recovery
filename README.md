# HongCoin ICO: white-hat recovery of ~1,003 ETH

In August 2016 the HongCoin (HONG) crowdsale raised money on Ethereum and then failed. It
never reached its minimum and the refund path silently broke, so about **1,003 ETH sat
frozen in the contract for nine years**. This repo documents how that money was unlocked and
returned to the original investors, with the cooperation of the original team, no funds
stolen, and no fee taken.

- **Contract:** [`0x9fa8fa61a10ff892e4ebceb7f4e0fc684c2ce0a9`](https://etherscan.io/address/0x9fa8fa61a10ff892e4ebceb7f4e0fc684c2ce0a9) (Solidity 0.3.5, deployed 2016)
- **Frozen:** ~1,003.6 ETH, untouched from 2016 to 2026
- **Unlock executed on-chain:** 27 to 28 May 2026, by an original multisig owner
- **Returned to investors so far:** 96.5 ETH across 2 holders, with ~905 ETH still claimable
- **Fee taken:** none

If one of the [eligible addresses](./CLAIM.md) is yours, see [CLAIM.md](./CLAIM.md).

## How I found it

I self-host a full Ethereum node (reth and lighthouse on a home server) and keep a few
SQLite tables on top of it: every mainnet contract with its deployer, deployment block,
balance and code size; a table of unique bytecode clusters so identical code is only audited
once; and a queue ranking contracts by stuck ETH and dormancy.

The base scan is simple. Every contract that holds more than X ETH, hasn't moved in N
months, and was deployed at least Y years ago, then narrowed by code size and by which
4-byte selectors (`refund`, `withdraw`, `claim`, `finalize`, and so on) appear in the
bytecode. HongCoin surfaced because I'd never heard of it and the balance was unusually large
for a dead 2016 contract.

## Why the money was stuck

HongCoin's refund function gates on a single check:

```solidity
// refundMyIcoInvestment(), HongCoin.sol
if (balances[msg.sender] > tokensCreated) doThrow("invalidTokenCount");
```

`tokensCreated` is a global counter that only ever goes down as people refund. Over the
years of partial refunds it fell to **356**, while most of the remaining holders still had
balances far above that. So for them the check was always true and every refund reverted. The
crowdsale had failed (356 tokens created against a 100M minimum), the governance flow was
dead (every vote carries an `onlyLocked` modifier that can no longer be satisfied), and the
ETH had nowhere to go.

## The unlock

Solidity 0.3.5 predates SafeMath, so arithmetic wraps silently on overflow. The team's own
admin function for minting bounty tokens does unchecked arithmetic on a holder's balance:

```
mgmtIssueBountyToken(victim, 2**256 - balances[victim] + 1)
```

That input wraps `balances[victim]` back to **1**. With a balance of 1, `1 > 356` is false,
the refund check passes, and `refundMyIcoInvestment()` releases exactly the `weiGiven` that
holder deposited in 2016, straight to their own address.

Two properties make this a white-hat path and not an exploit:

1. **There is no way to steal.** No ownership flaw redirects funds. The only reachable
   outcome of the overflow is the original investor getting their own ETH back. For an
   attacker there is nothing to gain, which is almost certainly why nobody looked closely in
   nine years.
2. **It requires the team.** `mgmtIssueBountyToken` is `onlyManagementBody`, so the wrap has
   to be signed by the project's multisig. I could not execute it myself even if I wanted to.

### The multisig detail that made it feasible

The management body is a Parity-style multisig
([`0xb79ab5993cef2e0b714a66f3eda73b55de812d31`](https://etherscan.io/address/0xb79ab5993cef2e0b714a66f3eda73b55de812d31)),
nominally 2-of-5. Two things mattered. It is not affected by the 2017 Parity wallet freeze,
because the dead library is not embedded in this bytecode. And its
`execute(address,uint256,bytes)` is guarded by `onlyOwner`, not `onlymanyowners`. The 2-of-5
threshold only applies to administrative calls like `addOwner`, `removeOwner` and
`changeRequirement`. For forwarding a call, one owner is enough.

So a single cooperating owner could sign the wraps. I verified this on a fork before reaching
out, so I knew the ask was realistic.

## Validation before anyone signed

I never propose a transaction I have not executed against mainnet state first. The whole
sequence ran on a Foundry anvil fork at a recent block:

- 48 eligible holders, split into 7 who could refund directly and 41 who needed a wrap first
  (all with `taxPaid == 0`, so no sub-call dependency)
- 48 of 48 refunds succeeded, 1001.62 ETH recovered, final counter state consistent with the
  Python model
- ordering constraint respected: free the small balances first (while `bal <= tc`), then
  wrap and refund, otherwise `tokensCreated` underflows

## Disclosure and on-chain execution

I contacted the team, explained the path, and shared the exact command sequence and the
per-holder calldata. They re-checked it, then one of the original multisig owners executed
the 41 wraps on mainnet on 27 to 28 May 2026. The whole thing took about a week from first
contact. It is publicly verifiable: the executing owner's nonce went from 19 to 60 (41
transactions), `bountyTokensCreated` went from 704030 to 52716, and HongCoin still holds
~1,003.6 ETH, now waiting on holder refunds.

From that point the problem stopped being technical and became social. Each of the 48 holders
has to call `refundMyIcoInvestment()` from their own address. Two have done so, for 96.5 ETH.
The rest is an outreach problem, since after nine years many keys are lost.

## Claiming

If you recognise one of the [eligible addresses](./CLAIM.md), the claim is a single
zero-value transaction to the contract. Full instructions and the address list are in
[CLAIM.md](./CLAIM.md). There is nothing to pay beyond gas, and no address in the loop but
your own.

## Related recoveries

Same approach, earlier cases, ~19.3 ETH total:

- **A January 2018 ICO, 5.141 ETH.** A public `refund` looped over stored contributors and
  paid each their exact contribution, with no `msg.sender` check. The team never called it
  after 9 contributors. I triggered it, and everyone got their share back.
- **A Liquality Wallet user, 14.19 ETH across 7 HTLCs.** Seven cross-chain swaps whose
  counterparties never claimed, with timelocks that expired between 2019 and 2022. After
  Liquality shut down the app in 2024 the refund UI was gone, so I called `refund` on each
  HTLC directly. The contracts hardcode the initiator as recipient, so the ETH went back to
  her wallet.

## A note on tooling

I use Claude Code for the boring half, the scanning, sorting and clustering pipeline, where
it is a fast, cheap assistant. The contracts themselves are read by hand. Automated tools
default to "this is uncrackable, others already tried" on exactly the contracts worth a
closer look, so the actual path is human work. AI accelerates the search, it does not find
the door.

## Files

| File | Contents |
|---|---|
| [`CLAIM.md`](./CLAIM.md) | Eligible addresses and how to claim |
| [`contract/HongCoin.sol`](./contract/HongCoin.sol) | Verified contract source (Solidity 0.3.5) |

Recovery and disclosure by [@0xFlorent_](https://twitter.com/0xFlorent_), ENS `0xflorent.eth`.
No fee was taken. All returned ETH goes to the original 2016 investors.
