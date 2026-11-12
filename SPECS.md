# Elysium Free Mint — Plan (DRAFT, not locked)
Date: 2026-11-09
Status: PLAN ONLY. No contract built, no testnet deploy, no mainnet deploy, no HYPE spent.
Model: Blurb-style free mint, on Elysium, using the user's own contract — NOT via blurbcollective.xyz (checked 2026-11-09 ~18:25 GMT: Blurb is one collection's own mint site on HyperEVM only, no creator/deploy tool, no Elysium support).

## 1. Proposed specs (DRAFT — user to confirm each line)
- Chain: Elysium mainnet, chain ID 99901, gas HYPE. Testnet first: 99801, faucet HYPE.
  Official mainnet RPC (verified live 2026-11-09 ~17:50 GMT, eth_chainId 0x1863d = 99901): https://rpc.elysium.kinetiq.xyz
  Explorer: https://elysium.kinetiq.xyz/explorer
  Bridge: https://elysium.kinetiq.xyz/bridge — HYPE HyperEVM → Elysium 1:1.
- Name: Elysian Fireflies — LOCKED 2026-11-09 ~18:43 GMT (user tapped it from fresh-names shortlist; fox/agent/Neon names explicitly rejected — fresh and unique only).
  Symbol: ELFY — PROPOSED, not locked (check collision in team research).
  Tagline (team proposal, not locked): "Light that found its way to Elysium."
  Art: NEW fireflies art (glow colours / variants, team to propose) — do NOT reuse NFOX fox art.
- Total supply: 3,333 — all free, no paid phase. (Blurb model: 3,333 free.)
- Price: $0. Free mint, gas only, paid by minter in HYPE.
- Max paid revenue: $0. There is no paid mint in this design.
- Free per wallet: 1, ever. (Blurb allowed 5/wallet public; 1/wallet is the user's proven Sigma model — 166 holders, each exactly 1 — and limits farming.)
- Max per tx: 1.
- Transferable: yes. NOT soulbound.
- Royalties: 5% ERC-2981, receiver = user wallet.
- Owner: user wallet 0x75d251Ba39dED2E456d9f6DD776f77d422864403.
  Owner CAN: withdraw any HYPE in the contract (expected $0, free mint), setBaseURI, transfer/renounce ownership.
  Owner CANNOT: change supply, free supply, price (none), max 1/wallet, mint extra, pause, freeze, burn, or take holders' NFTs. (Same owner-power limits as NFOX; confirm in contract code before deploy.)
- Art: NEW fireflies art, 12 variants per Team 2 (see TEAM-RESEARCH.md) — no fox reuse, locked 2026-11-09 ~18:43 GMT.
- Metadata: one image on IPFS + one JSON per token ID hosted on the mint site, setBaseURI after deploy, tokenURI(1) proven live before launch. Same as NFOX.
- No audit by a third party. OpenZeppelin use is not an audit.
- No promises on price: no floor price, returns, APY, game, or merch. Do NOT copy Blurb's "$BLURB airdrop per NFT" promise.
- TOKEN — PLANNED, user decision 2026-11-09 ~18:50 GMT: "let's launch it with a planned token in near future. rework it". A token IS now part of the plan (see §9 below, PROPOSED not locked). No token launch date, price, or allocation is promised in public until §9 is locked. Do not publish token numbers until then.

## 2. The one big decision: the gate — LOCKED 2026-11-09 ~18:42 GMT: Option A OPEN
Binance Agent ID registry 0x60B9262456dE011F7d15a54CdD8c48bAaa8B022F is on BSC (chain 56) only. No evidence it is deployed on Elysium (99901). The NFOX template's on-chain Agent ID check cannot be copied as-is.

Option A — OPEN free mint (LOCKED, user tapped 2026-11-09):
  Anyone with an Elysium wallet can mint 1 free. No gate in the contract.
  Pros: simplest, fastest, no list to build, no extra gas.
  Cons: bots/farmers can mint 1 each from many wallets. 1/wallet limits, does not prevent, sybil.

Option B — ALLOWLIST free mint (recommended for holder quality):
  Owner sets an allowlist on-chain (mapping, set before mint opens) from a snapshot — e.g. wallets that hold a Binance Agent ID on BSC, or Hyperliquid genesis HYPE airdrop recipients (the Elysium x OpenSea mint used that eligibility).
  Pros: keeps the Agent-ID-holder quality story; only listed wallets can mint.
  Cons: list must be built + verified first; setting it on-chain costs gas; a wallet that changes after the snapshot is not in.
  Note: the snapshot is BSC/Hyperliquid data read off-chain, then written on-chain as a list — the contract does NOT read BSC itself.

User to choose A or B before any code. If B, user also chooses which snapshot.

## 3. Contract plan (adapted from the agent-mint-launch playbook)
- Template: ~/workspace/agent-mint-launch/contracts/NeonFoxAgents.sol (Solidity 0.8.26, OpenZeppelin 5.6.1, Cancun, optimizer 200).
- Changes from NFOX, to be recorded in the contract header:
  1. Chain Elysium, not BSC. Registry address removed.
  2. No paid mint at all: remove mint() paid path / paid price / paid supply. Keep freeMint() only, plus views totalSupply, freeMintedCount, freeRemaining, freeOpen, freeMinted.
  3. Gate: Option A — no registry check, EOA-only + 1/wallet. Option B — allowlist mapping check instead.
  4. Royalties 5% kept.
- Local gate: compile + full Anvil suite, all tests pass (free mint, double-free revert, 1/wallet cap, royalty, no paid path). Slither + Aderyn, no new issues.
- Testnet gate: fresh throwaway deployer, faucet HYPE only, never mainnet key. Deploy, live free-mint test, setBaseURI, prove tokenURI(1) art loads. Label testnet contract superseded, never point minters at it.
- Verification: Sourcify + Elysium explorer. NOT BscScan (BSC explorer does not cover Elysium).
- Mainnet gate: recheck all gates in one message, explicit user go-ahead on the final contract, triple-check constructor args + owner = user wallet, check totalSupply 0 / freeRemaining 3,333 / freeOpen true on-chain right after deploy, return leftover deployer HYPE.

## 4. Site plan
- Cloudflare Pages, same professional system as the 13 BSC sites: dark, restrained, one accent, mobile-first, EN / 中文 toggle.
- No gates on the mint site (per user rule 2026-11-06: NFT mint sites use NO gates — minting stays open to any eligible wallet).
- Paste-address checker as main flow, browser mint as secondary. No deploy panels / preview banners in the public site.
- List on: ElysiumEco directory, Elysium explorer, OpenSea — only link a marketplace after it actually loads the collection; do not claim OpenSea until it indexes.

## 5. Costs (estimates — verify live at deploy time)
- Testnet: $0 (faucet HYPE).
- Mainnet deploy gas: HYPE only. Gas seen ~0.149 gwei on 2026-11-09 — deploy is expected to be cents, not dollars. Exact amount is only known at deploy time; keep a small HYPE reserve (suggest 0.5 HYPE bridged, use what is needed, rest stays in the user wallet).
- Minters: gas only, in HYPE, per mint.
- No platform fee (own contract). No launchpad fee.
- User must bridge HYPE themselves via the official bridge before mainnet deploy (user sends, never claim on their behalf).

## 6. Risks (plain English)
- Day-one chain: Elysium mainnet went live 9 Nov 2026, single sequencer, only the bridge audit found. Chain could be slow / down / change.
- No Agent ID gate on-chain (Option A) means farming is possible; 1/wallet is a limit, not sybil-proof.
- OpenSea / marketplace indexing on Elysium is unproven for a new collection — do not promise it.
- Free mints on Elysium are crowded: Ascend alone had 1,124 new coins in 24h on 2026-11-09, only 18 above $40k. A free NFT mint needs a post + a reason to mint, not just a live contract. User posts manually on X; no X login.
- ElysiumEco listing is community-run, not official — listing is a request, not guaranteed.

## 7. Steps in order (no step skipped)
0. User confirms: name, symbol, gate A/B (+ snapshot if B), art (reuse fox / new).
1. Build contract v1 complete (adapted template).
2. Local gate: compile + tests + scans.
3. Testnet gate: deploy + live free mint + baseURI proof.
4. Metadata: IPFS image + 3,333 JSONs, zero placeholders.
5. Verification: Sourcify + Elysium explorer.
6. Site: build + preview + zip, user deploys via Cloudflare.
7. Mainnet gate: full recheck + explicit go-ahead.
8. Mainnet deploy + setBaseURI + live count check.
9. Launch post (drafts in X Post Drafts, user posts manually) + ElysiumEco listing request.

## 9. Token plan — PROPOSED 2026-11-09 ~18:50 GMT, NOT locked
- Idea: Fireflies NFT first, token second. NFT is the ticket, token is the reward for holding it. This is now a 2-stage launch, not a free mint only.
- Token ticker (PROPOSED): $GLOW — the glow of a firefly. NOT checked on Ascend yet — check before locking. Do NOT use ELFY for the token: ELFY is the NFT symbol, and it is also a live Nasdaq ETF ticker, so $ELFY would confuse search. $GLOW is clean, visual, and works as a verb ("catch the glow").
  Fallback if GLOW is taken: $FLY (firefly short) / $LUMEN.
- Launch route (PROPOSED): Ascend on Elysium, same as the memecoin check — free to launch, network fee only, supply fixed 1B, no bonding curve, liquidity locked. Pair HYPE, fee 1% Pays creator.
- Supply (PROPOSED): 1,000,000,000 $GLOW fixed.
  Allocation draft (PROPOSED, not locked):
  - 33.3% — Fireflies NFT holders (333,000,000) — 100,000 $GLOW per NFT held at snapshot, weighted by tier: Common 1x, Rare 2x, Legendary 3x, Aurora 5x. Snapshot after the NFT mint sells out OR 7 days after mint opens, whichever is first — holder must still hold at snapshot.
  - 40% — Liquidity Pool (400,000,000)
  - 15% — Treasury / future quests and Fireflies v2 (150,000,000)
  - 7% — Team (70,000,000, vested, not sold at launch)
  - 5% — Marketing / launch (50,000,000)
- Claim (PROPOSED): NFT holders claim on the Fireflies site after TGE, 1 claim per wallet, gas only. No claim before snapshot is published. Unclaimed after 90 days returns to treasury.
- Why this changes the NFT: a token promise WILL attract farmers — 1/wallet is now a limit, not protection. Mitigations: snapshot + hold-time weighting, Aurora/Legendary multipliers reward real holders, no airdrop to wallets that minted and dumped before snapshot.
- Timing (PROPOSED, not a promise): NFT mint first, token "in the near future" — after NFT mint + site + verification, and after the Fireflies holder count is known. No date locked.
- Decisions still needed to lock §9: ticker GLOW vs fallback, allocation %s, tier multipliers, snapshot rule, claim window, team vesting.

## 10. Decisions needed from the user (before code starts)
1. Name: Elysian Fireflies — LOCKED 2026-11-09 ~18:43 GMT. Symbol ELFY — team research to check collision.
2. Gate: A open — LOCKED 2026-11-09 ~18:42 GMT.
3. Art: NEW fireflies art — LOCKED in principle (no fox reuse); exact variants/traits — team research.
4. Supply: keep 3,333, or change it? — STILL OPEN (default 3,333 in §1)
5. Team research in progress 2026-11-09 ~18:44 GMT (coordinator + 3 teams: ecosystem/collisions, concept/art, viability/risks) — report to ~/workspace/elysium-free-mint/TEAM-RESEARCH.md, no build/deploy until it returns.
