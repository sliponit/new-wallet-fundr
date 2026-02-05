# new-wallet-fundr

Idea is inspired from this video https://youtu.be/ppRz6Hqoics and targeting li.fi bounty for hackmoney. 

- V1 CLI to bridge-in (LI.FI) → shield → wait for PPOI → unshield to fresh address → bridge-out. 
- V2 if time: could use a local LLM (Ollama) to interpret user input


```
┌─────────────────────────────────────────────────────────────────┐
│                         USER INPUT                              │
│  Source chain, token, amount, destination chain, fresh address  │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
              ┌───────────────────────┐
              │   1. BRIDGE IN        │
              │   LI.FI: Source →     │
              │   Railgun Chain       │
              │   (Arbitrum default)  │
              └───────────┬───────────┘
                          │ ~1-10 min
                          ▼
              ┌───────────────────────┐
              │   2. SHIELD           │
              │   Deposit into        │
              │   Railgun private     │
              │   pool (0.25% fee)    │
              └───────────┬───────────┘
                          │ immediate
                          ▼
              ┌───────────────────────┐
              │   3. WAIT FOR PPOI    │◄─────┐
              │   Poll every 5 min    │      │ ShieldPending
              │   Check balance       │──────┘
              │   bucket status       │
              └───────────┬───────────┘
                          │ ~1 hour (bucket = Spendable)
                          ▼
              ┌───────────────────────┐
              │   4. UNSHIELD         │
              │   Generate ZK proof   │
              │   (20-30 sec)         │
              │   Withdraw to fresh   │
              │   0x address          │
              └───────────┬───────────┘
                          │ ~30 sec
                          ▼
              ┌───────────────────────┐
              │   5. BRIDGE OUT       │
              │   LI.FI: Railgun      │
              │   Chain → Destination │
              └───────────┬───────────┘
                          │ ~1-10 min
                          ▼
              ┌───────────────────────┐
              │      COMPLETE         │
              │   On-chain link       │
              │   broken, funds on    │
              │   destination chain   │
              └───────────────────────┘
```
