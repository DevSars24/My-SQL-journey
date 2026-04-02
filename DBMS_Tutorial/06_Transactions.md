# 6. Transactions and Concurrency Control

## 📖 The Core Concept
A **Transaction** is a logical unit of work.  
- **Scenario:** Transferring \$100 from Alice's bank account to Bob's.
  - Step 1: Deduct \$100 from Alice.
  - Step 2: Add \$100 to Bob.
If the database crashes exactly after Step 1, Alice lost money, and Bob never got it! A transaction ensures that *either both steps happen perfectly, or neither happens at all.*

---

## 🛡️ ACID Properties (The Holy Grail of DB reliability)

### 1. **A**tomicity ("All or Nothing")
- **Scenario:** The bank transfer. If it fails halfway, roll it back like it never happened. No partial executions.

### 2. **C**onsistency ("The Rules Still Apply")
- **Scenario:** If the bank rule says "Accounts cannot have a negative balance," the transaction must guarantee this rule is still intact before and after the transfer.

### 3. **I**solation ("Stay in your lane")
- **Scenario:** While Alice is making her transfer, Bob shouldn't be able to peek at Alice's balance half-way through the transaction. Concurrent transactions shouldn't mess each other up. 

### 4. **D**urability ("Set in Stone")
- **Scenario:** If the screen says "Transfer Successful", and someone pulls the power cord to the server 1 second later, the data must still be there when the server reboots. It is permanently saved to the hard drive.

---

## 🚦 Concurrency Control (Traffic Lights for Data)
What happens if two people try to buy the very last concert ticket at the exact millisecond?

### Locks
- **Shared Lock (Read):** Multiple people can read a file at once, but nobody can edit it.
- **Exclusive Lock (Write):** Only ONE person can hold it. They can read and edit. Nobody else can even look until they are done.

### Deadlocks
- **Scenario:** Two narrow cars in an alleyway. Car A needs Car B to back up. Car B needs Car A to back up. Neither moves. 
- Databases handle this by either forcing one transaction to "die" (abort) or using **Timestamp Ordering** (older transactions get priority).

---
## 🧠 Memorization Cheat Sheet
- **A**tomicity: All or nothing.
- **C**onsistency: Rules aren't broken.
- **I**solation: Invisible to others until done.
- **D**urability: Written permanently to disk.
