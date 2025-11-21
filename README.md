
# AirFlux - Airdrop Distribution Smart Contract**

A comprehensive and secure **Clarity smart contract** for managing token airdrops on the Stacks blockchain.
This contract enables the project owner to register eligible recipients, distribute fungible tokens, manage tiered rewards, reclaim unclaimed tokens, perform emergency withdrawals, and log all events on-chain.

---

## **📌 Features**

### **✔️ Airdrop Management**

* Activate/deactivate airdrops
* Pause and resume distribution
* Fixed per-recipient airdrop amounts
* Tier-based multipliers for custom claim sizes

### **✔️ Eligibility + Claiming**

* Add, remove, and bulk-add eligible recipients
* Prevent double-claims
* Track claimed amounts
* Tiered airdrop claiming with multipliers

### **✔️ Token Controls**

* Mints a fixed supply of `airdrop-distribution-token` at initialization
* Transfers tokens to users upon claim
* Burns unclaimed tokens after reclaim period expires

### **✔️ Security & Administration**

* Contract owner restricted operations
* Time-locked emergency withdrawal
* Emergency pause capability
* Validation for all state-changing functions
* On-chain event logging with sequential event IDs

---

## **📂 Contract Architecture**

### **Data Variables**

* `is-airdrop-active` — Global on/off switch
* `is-paused` — Prevents claims but keeps config intact
* `airdrop-start-block` — Used for reclaim timing
* `reclaim-period-length` — Duration before claim deadline
* `airdrop-amount-per-recipient` — Base claim amount
* `emergency-timelock` — Block height after which emergency withdrawal is allowed
* `total-tokens-distributed` — Accounting for claimed tokens

### **Maps**

* `eligible-airdrop-recipients` — Principal → bool
* `claimed-airdrop-amounts` — Principal → uint
* `tier-multipliers` — uint tier → uint multiplier
* `contract-events` — event-id → {event-type, data}

### **Token**

* `airdrop-distribution-token` — Fungible token minted at contract deployment

---

## **🛠 Public Functions**

### **Admin-Only**

| Function                           | Description                        |
| ---------------------------------- | ---------------------------------- |
| `add-eligible-recipient`           | Add a single eligible address      |
| `bulk-add-eligible-recipients`     | Add up to 200 at once              |
| `remove-eligible-recipient`        | Remove address eligibility         |
| `update-airdrop-amount`            | Change base claim value            |
| `update-reclaim-period`            | Adjust reclaim period              |
| `pause-airdrop` / `resume-airdrop` | Pause/unpause airdrop              |
| `set-tier-multiplier`              | Set rewards per tier               |
| `initiate-emergency-withdrawal`    | Starts timelock countdown          |
| `execute-emergency-withdrawal`     | Withdraw all tokens after timelock |

### **Recipient Functions**

| Function               | Description                 |
| ---------------------- | --------------------------- |
| `claim-airdrop-tokens` | Standard one-time claim     |
| `claim-tiered-airdrop` | Claim with multiplier level |

### **Reclaim Function**

| Function                   | Description                                     |
| -------------------------- | ----------------------------------------------- |
| `reclaim-unclaimed-tokens` | Burns all unclaimed tokens after reclaim period |

---

## **📡 Read-Only Functions**

| Function                           | Description                    |
| ---------------------------------- | ------------------------------ |
| `get-airdrop-active-status`        | Returns if airdrop is active   |
| `get-pause-status`                 | Returns if paused              |
| `get-tier-multiplier`              | Retrieve multiplier for a tier |
| `get-emergency-timelock`           | Returns timelock height        |
| `is-recipient-eligible`            | Check eligibility              |
| `has-recipient-claimed-airdrop`    | Has user claimed?              |
| `get-recipient-claimed-amount`     | How much user claimed          |
| `get-total-tokens-distributed`     | Global total distributed       |
| `get-airdrop-amount-per-recipient` | Base amount                    |
| `get-reclaim-period`               | Claim deadline window          |
| `get-airdrop-start-block`          | Start block                    |
| `get-event`                        | Retrieve logged events         |

---

## **🧪 Testing Suggestions**

Recommended test coverage:

* Airdrop claim success & failure paths
* Tiered claim logic
* Paused airdrop behavior
* Emergency timelock enforcement
* Token balance checks
* Reclaim procedure
* Bulk adding recipients
* Event logging integrity

---

## **⚠️ Important Notes**

* Only the contract owner (defined at deploy time) can modify airdrop configurations.
* Claiming is allowed only once per user, whether tiered or standard.
* Emergency withdrawal requires waiting until the timelock expires.
* This contract **burns** all unclaimed tokens after the reclaim period.

---

## **🚀 Deployment Checklist**

1. Deploy the contract
2. Confirm owner address
3. Minted tokens appear in owner balance automatically
4. Add eligible recipients
5. Set tier multipliers (if needed)
6. Activate (or resume) airdrop
7. Distribute tokens via claims

---
