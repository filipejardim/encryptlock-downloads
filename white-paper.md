# EncryptLock — White Paper

**EncryptLock (LOCK)** is a new, independent blockchain, running since 18 October 2026 (network `encryptlock-1`).

> *"The coin where money is not lost: not by mistake, not by theft, not by death, not by quantum computers."*

This document explains what EncryptLock is, how it works and what is still missing. Everything described here, unless marked as not built yet, runs on the network today.

---

## 1. Summary

| Topic | Rule |
|---|---|
| Coin | **EncryptLock**, symbol **LOCK**, 8 decimals |
| Maximum supply | **40,000,000 LOCK**, never more |
| At the start | 1,000 LOCK for the first LockKeepers + **400,000 LOCK (1%) for the creator**, public since block 1 |
| Release | 38,799,000 LOCK over 30 years, halving every 5 years; then an everlasting reserve of 800,000 LOCK |
| Mining | **None.** The network is run by LockKeepers (real home computers) |
| Who gets new coins | 100% the LockKeepers, every hour, by hours on × guarantee |
| Transfers | Must be **accepted** by the receiver within **1 hour**, or they come back |
| Accounts | Must be **registered** (free) before they can receive |
| Signatures | Every account signs with **Ed25519 + ML-DSA-65** (post-quantum); blocks are signed with ML-DSA-65 |
| Block time | About **5 seconds**, final at once |
| Transfer fee | 0.1% (min 0.0002, max 0.01 LOCK), paid by the sender |
| Trade fee (LOCK ↔ USDC) | 0.25%, paid by the seller |
| Fee split | Development fund / LockKeepers: 75/25 for 5 years, then 50/50 forever |
| Real computers only | **1 computer = 1 LockKeeper**, proven by the factory security chip (TPM 2.0); cloud servers and virtual machines refused |
| Governance | No voting and no company. The golden rules cannot be changed |

---

## 2. Vision

Digital money that is fast and cheap, with no name or personal data, made for ordinary people. What makes EncryptLock different is **protection of the user, written into the rules of the network itself**: mistakes can be undone, theft can be stopped, coins pass to your family, and the signatures are ready for quantum computers.

---

## 3. Launch and transparency

- **No company, no coin sale, no presale, no investors, no airdrop.** New coins only come from running a LockKeeper.
- **The creator's reward is public:** 1% of the maximum supply (400,000 LOCK) is in the creator's account since the first block. The creator also runs the first LockKeepers and earns like any other LockKeeper. All of this is visible on the chain.
- **Genesis (block 1):** 401,000 LOCK in total — 1,000 to start the first LockKeepers and 400,000 for the creator. Nothing else.
- **Honest note about the start:** while there are few LockKeepers, the creator's computers receive most of the new coins. The sooner other people run LockKeepers, the fairer the distribution.
- **The price is never promised.** LOCK's price is set only by people trading in the app.
- **The wallet is non-custodial:** the keys stay only on the user's device. Nobody, not even the creator, can reach anyone else's coins.
- **The code is private while the network starts** and will be published when about 20 LockKeepers of other people are running, so anyone can check that these rules are really there.

---

## 4. Coin and release

- **Maximum supply: 40,000,000 LOCK**, 8 decimals. No function can create more.
- **30 years of release (360 months), halving every 5 years:**
  - Month *n* releases `a × r^(n−1)`, with `r = 0.5^(1/60)`, so the amount halves every 60 months.
  - `a ≈ 452,718.2` LOCK, chosen so that the 360 months + genesis (401,000) + the everlasting reserve (800,000) add up to **exactly 40,000,000**.
  - About: month 1 ≈ 452,718 · month 60 ≈ 228,989 · month 120 ≈ 114,495 · month 240 ≈ 28,624 · month 360 ≈ 7,156.
  - The 360 values are fixed in the code as whole numbers (units of 10⁻⁸ LOCK), so every computer computes exactly the same.
- **Clock time, not blocks:** a "month" is 30 days of network time from the genesis block. Each block releases the part of the month that matches the time passed. The speed of blocks never changes how many coins are released.
- **Everlasting reserve for the LockKeepers (800,000 LOCK, 2%):** nothing during the first 30 years. From year 31, what is left halves every 30 years (years 31–60: 400,000; 61–90: 200,000; and so on). 99.9% is released by year 330; the last 0.00000001 LOCK around 1,440 years after the start. It does not depend on price.
- **Coins of cheaters are burned**, so the real supply can end up below 40,000,000, never above.
- After the 30 years, LockKeepers live from fees and the everlasting reserve.

---

## 5. The network: LockKeepers

A **LockKeeper** is a real computer that runs the EncryptLock blockchain: it keeps a copy of all accounts, checks every transfer, helps create the blocks and earns LOCK for it. There is no mining and no central server.

### 5.1 Who earns
- **Everyone switched on earns**, not only those signing. Every hour, the new coins and the LockKeepers' share of the fees are shared by **hours on × guarantee**, straight into the balance.
- The computer says "I am on" every 5 minutes. Saying it without working earns nothing.
- **Guarantee:** at least 1 dollar of LOCK (at the price of the last trades). It is not a payment: it stays in the owner's account, only locked. To stop, the owner asks for it back and it is free 21 days later.
- **Switching off costs nothing:** you just don't earn while off.
- **Only cheating costs:** signing two different blocks burns 5% of the guarantee.

### 5.2 Who signs the blocks
- Each block is signed by a **group drawn by lot** among the LockKeepers switched on without a break for at least 1 hour. Everyone has the same chance and, inside the group, **each computer is one vote** (the guarantee counts for earnings, not for signing).
- The group starts with **100 seats** and grows with the network (one seat per 10 LockKeepers on) up to **300**. With fewer LockKeepers on than seats, all of them sign.
- It changes a little at a time: every 6 minutes 1% of the seats change hands (~10% per hour), the longest-seated first, so everyone gets a turn. A member that misses half of the last 120 blocks (about 5 minutes) is replaced at once.
- The draw uses the block's own data, so every LockKeeper reaches the same group.
- **Start phase:** while there are fewer than **20 stable LockKeepers** (100 hours on in the last 7 days), the computer that started the network holds more than 2/3 of the signing power. So a home computer switched off without warning never stops the network. Earnings do not change. From 20 stable LockKeepers on, every computer counts the same again, by itself.

### 5.3 Real computers only
- **1 computer = 1 LockKeeper.** The computer proves it with its factory security chip (TPM 2.0: AMD, Intel, Infineon, STMicro, Nuvoton, Microsoft Pluton), checked with the maker's certificate and a secret test only the real chip can answer.
- The network keeps only a code derived from the chip (no name, no personal data); each code can be used once.
- Virtual chips (cloud servers, virtual machines) are refused. Ten LockKeepers need ten computers.

### 5.4 A light program
A LockKeeper must never fill people's disk or slow their computer:
- It keeps only the last **~40 minutes of whole blocks**. Of older blocks it keeps only the header, which proves cheating. What nothing reads is deleted while it runs.
- Disk: around half a gigabyte; about 1 GB at most with a full signing group.
- Internet: a few kilobytes per second today; about 1.5 Mbit/s at most with a full signing group.
- Low priority and a memory limit, so games and other programs come first. The computer does not fall asleep by itself while it runs.
- Every 10 minutes it keeps a copy of the network's state, so a new LockKeeper, or one that was off for a long time, starts from that copy in minutes.

### 5.5 Consensus and technology
- Instant finality (BFT): a block about every 5 seconds; a transfer is final as soon as its block is signed.
- Built on CometBFT and the Cosmos SDK (Go), as its own independent blockchain. Direct coin sending (it would bypass acceptance), on-chain voting and the standard emission module are switched off.

### 5.6 Cryptography
- **Accounts:** hybrid signatures, **Ed25519 and ML-DSA-65** (NIST post-quantum standard, FIPS 204). Both are required in every transaction. The hybrid public key is recorded once, when the account is registered.
- **LockKeepers' votes:** ML-DSA-65 from the first block. The network accepts no other key type for LockKeepers.

### 5.7 Privacy
- **No name or personal data:** an account is only a `lock1…` address. The app sends no data to anyone (no telemetry).
- **Balances and transfers are public** on the chain. EncryptLock is not anonymous. This is what makes the protections possible: acceptance, "It wasn't me", limits, inheritance and the vault.

### 5.8 Golden rules and updates
- **Golden rules (never change):** the 40,000,000 supply, the release curve and the everlasting reserve, nobody can freeze or take another person's coins, acceptance within 1 hour with automatic return, the fee split.
- Anyone could copy the code and start *another* network with different rules; that cannot be prevented. What protects the golden rules is that EncryptLock's LockKeepers refuse versions that change them. Every official version carries tests that fail if a golden rule changes.
- **Updates:** bug and security fixes come as new versions. Each LockKeeper decides whether to install. There is no vote and no owner.

---

## 6. Accounts and transfers

### 6.1 Registration
- An account exists only after it is **registered** on the network, which records its hybrid public key. Registration is free.
- **Coins can only be sent to registered accounts.** Every address carries a check: a typo is refused.

### 6.2 Acceptance
- **Every transfer between accounts waits until the receiver accepts it**, for up to **1 hour** of network time. If not accepted, the coins return to the sender automatically.
- No exceptions: normal transfers and QR codes alike.
- While it waits, the sender can cancel.
- Movements of the network itself (new coins, fee split, burning of cheaters, trades, inheritance, vault exits) follow their own rules.
- Unwanted "dust" from strangers never enters an account: it is simply not accepted.
- Note: automatic online shops do not work with this rule. This is accepted on purpose.

### 6.3 Who pays the fee
- The sender pays when creating the transfer. Accepting, cancelling and registering are free.
- If a transfer expires or is cancelled, the amount returns in full but **the fee does not** (so nobody can fill the network with free pending transfers).

---

## 7. Security shield

### 7.1 Theft protection
- **"It wasn't me":** freezes the account in one tap and returns every pending transfer it sent. The account can still receive. Unfreezing: both keys together → at once; one key alone → 7 days.
- **Second device:** another account linked to yours that can **only approve and freeze, never send**. Linking the first one is immediate; changing or removing it takes 7 days.
- **Extra approval** for a **new contact** (an address that never accepted a transfer from this account) or an amount **above your limit**: the second device has 24 hours; then the receiver has 1 hour to accept. Without approval, the amount returns.
- **Daily limit** (24-hour window): lowering it applies at once; raising it only after 48 hours.
- **On the device:** the wallet is encrypted with your password and the phone's security chip; fingerprint or face; a separate send code; it locks itself when not in use; repeated wrong passwords make you wait longer each time.

### 7.2 Never lost by death or a lost phone (both optional)
- **Inheritance:** you choose heirs (shares adding up to 100%) and how long without a sign of life, **at least 1 year**. Any transaction, or the "I'm here" button, is a sign of life and cancels a request at once. All heirs must confirm within 30 days of the first (you can choose that fewer are enough). Your app shows an alert while the request waits. When all have confirmed, 30 more days; only then the coins pass to the heirs.
- **Trusted people:** at least 2, and at least 2 must approve the same new account. Then 30 days in which the owner can cancel. A transaction of the account does not cancel a recovery (it could be a thief); only the owner's explicit cancel does.
- If the account was a LockKeeper, its guarantee goes too. The old account is closed: anything that reaches it later follows by itself to the heirs or the new account.
- Changing heirs or trusted people takes 7 days (the first plan applies at once), and the owner can cancel the change.

### 7.3 Your 24 words
- Both keys of an account (Ed25519 and ML-DSA-65) come from **24 recovery words**. Whoever has them always recovers the wallet.
- Without the 24 words and without the device, nobody — not even the creator — can recover a wallet. That is what makes it non-custodial.

### 7.4 Sovereign Vault (for large amounts)
- **Delay on exits:** every sending waits 7 to 365 days, and the vault can cancel during the wait; then the receiver has 1 hour to accept. Raising the delay applies at once; lowering it only after the current delay. "It wasn't me" cancels a pending lowering.
- **Locks:** lock coins until a date (up to 10 years); nobody can move them before, not even you; they come back by themselves on that date.
- **Proof of reserves:** anyone can check on the chain that "this account has at least X locked until date D", valid for the whole period. It is public.
- **Team accounts** (M of N members, each a hybrid account) exist on the network.

---

## 8. Fees

| | Fee | Who pays |
|---|---|---|
| Transfer | 0.1% (min 0.0002, max 0.01 LOCK) | Sender |
| Trade LOCK ↔ USDC | 0.25% | Seller |
| Register, accept, cancel | Free | — |

**Split:**

| Period | Development fund | LockKeepers |
|---|---|---|
| Years 1–5 | 75% | 25% |
| Year 6 on, forever | 50% | 50% |

- The development fund is **one normal EncryptLock wallet, the creator's**, with a **public address** recorded in the genesis block. Like every account it is post-quantum and uses the same protections.
- The same fees apply to everyone. No app gets a discount.
- A "year" is 365 days of network time from the genesis block.

---

## 9. Buying and selling LOCK

- **Buy / Sell** inside the app: an **order book** with real offers. To buy, someone has to be selling.
- Payment is in **USDC on Polygon**. The seller's LOCK are locked on the EncryptLock network; the buyer pays through the network's vault contract on Polygon, which passes the USDC to the seller. The LockKeepers watch Polygon, and once more than 2/3 of them have seen the payment, the LOCK go to the buyer. If the payment does not arrive in time, the LOCK return to the seller.
- **Nobody holds anyone's money.** The buyer also needs a little POL in the same wallet to pay Polygon's own network fee (cents per operation), as in every Polygon wallet.
- LOCK's price is the price of the trades made in the app.

---

## 10. Apps

- **Windows:** wallet + LockKeeper ("Make this computer a blockchain LockKeeper").
- **Android:** wallet. A phone cannot be a LockKeeper.
- **21 languages:** English, Portuguese (Portugal and Brazil), Spanish, French, German, Italian, Russian, Ukrainian, Chinese (Simplified and Traditional), Japanese, Korean, Arabic, Hindi, Dutch, Polish, Turkish, Indonesian, Vietnamese and Thai.
- The apps update themselves: when a new version is out, they show **Update** and check the file's fingerprint before installing.
- Website and explorer: [encryptlock.pages.dev](https://encryptlock.pages.dev).

---

## 11. Not built yet

To be clear about what does not exist today:
- **Final milestone:** the plan is that at 10,000,000 active wallets every fee halves forever and the split becomes 50/50 at once. How the network counts "active wallets" by itself is not decided yet, so this rule is not active.
- **iPhone app:** later.
- **Other coins** (BTC, ETH, USDT) and USDC on other networks: later, each with its own study.
- **Payments without internet, protected purchases, recurring payments, family accounts:** ideas, not confirmed.
- Margin trading, shorts and leverage are out of scope.

---

## 12. Legal notes (summary, not legal advice)

- No company, no coin sale and no custody of anyone's money.
- EncryptLock has no anonymous mode: balances and transfers are public, without names.
- Whoever converts coins to money pays the taxes of their own country.
- Nothing here is financial advice.

---

Contact: encryptlockoficial@proton.me · Website: [encryptlock.pages.dev](https://encryptlock.pages.dev)
