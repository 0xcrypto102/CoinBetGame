# 🪙 Memecoin Contest Platform

A decentralized contest platform built on the **Solana** blockchain where meme tokens battle for dominance. Built with **Rust** and **Anchor**, the platform allows users to stake their favorite tokens in head-to-head contests and win token rewards based on real-time liquidity pool valuations.

---

## 🚀 What is Memecoin Contest?

**Memecoin Contest** (name subject to change) is a smart contract-based competition between two meme tokens (e.g., Alicecoin vs Bobcoin). Users deposit tokens to back one side, and at the end of the contest, the winning side's supporters receive both their deposit and a portion of the losing side’s tokens.

---

## 🧩 How It Works

### Contest Parameters
- Two competing tokens (`Alicecoin`, `Bobcoin`)
- Contest end time (e.g., March 15, 00:00 UTC)
- Platform wallet and fee percentage (e.g., 2%)

### Contest Lifecycle

1. **Initialization**
   - Admin sets up a new contest with token mints, LP sources, caps, and duration.

2. **Participation**
   - Users deposit tokens to support one of the two sides.
   - Each deposit is capped per contest rules.
   - A **bet coefficient** is calculated:
     ```
     bet = SOL_value_of_deposit * (1 + value_opposite_side) / (1 + value_same_side)
     ```

3. **End & Evaluation**
   - Token prices are fetched via token vaults (LPs).
   - Values are calculated:
     ```
     Value = Price * Quantity
     ```
   - The higher value side wins.

4. **Redemption Phase**
   - 2% of all tokens go to the platform wallet.
   - Winning side:
     - Withdraws their tokens (minus fee)
     - Claims a share of the losing side’s tokens proportionally to their bet

5. **Final Withdrawal**
   - Admin may withdraw unclaimed tokens after a wait period.

---

## 🛠 Features

- Built with Anchor framework on Solana
- Dynamic LP integration (no reliance on Pyth oracles)
- Secure token vault management
- Fair reward calculation using bet coefficient
- Contest lifecycle management (initiate, deposit, finalize, claim)
- Admin update for fee and ownership

---

## 📦 Program Architecture

### 🧠 Core Modules

- **lib.rs** – Entry point, routing instructions
- **state.rs** – Contest, global state, and user data models
- **instructions.rs** – All business logic for contest creation, participation, claiming, and finalization
- **constants.rs** – Seeds and constants used across program
- **errors.rs** – Custom error codes for validation and constraints

### 🧮 Bet Formula

The smart contract determines winners and allocations based on a custom betting formula:
```rust
bet = sol_value_of_deposit * (1 + value_opposite_side) / (1 + value_same_side)
```

### 🔐 Winner Determination

At contest end:

- Total value of both token pools is compared
- Winner is declared based on higher value
- Ties resolved randomly using timestamp parity

### 🔐 🧪 Running Locally

Install the required Solana and Anchor dependencies:
```
anchor build
anchor deploy
```
You can also run tests and validate local logic using:
```
anchor test
```

### 📘 Example Logic (Simplified)

```
if Value(Alicecoin) > Value(Bobcoin) {
    winner = Alicecoin;
    reward = BobcoinPool * (user_bet / total_alice_bets);
} else {
    winner = Bobcoin;
    reward = AlicecoinPool * (user_bet / total_bob_bets);
}
```
### ⚙️ Configurable Parameters

- Contest duration
- Maximum deposit per user
- Platform fee percentage
- Token vaults for price tracking
- Admin address




