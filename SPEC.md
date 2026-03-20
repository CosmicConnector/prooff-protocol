# Prooff Protocol Specification v0.1
## Open Nostr Trust Layer for AI Agents
**Date:** March 20, 2026  
**License:** Apache 2.0  
**Status:** Draft → MVP

### 1. Purpose
Prooff turns any AI agent into a verifiable, human-scoped, reputable entity using only native Nostr events. Merchants verify in <100 ms with zero custody or new chain.

### 2. New Event Kinds
- **30040** – Agent Manifest (replaceable event)  
  Tags:  
  - `d` = agent-npub (unique ID)  
  - `owner` = human owner npub  
  - `name` = human-readable agent name  
  - `capabilities` = JSON array of allowed actions  
  - `manifest` = free-text description  

- **30041** – Prooff Attestation (ephemeral or replaceable)  
  Contains signed NIP-26 delegation + attached badges.

### 3. NIP-26 Scoped Delegation (standard, no change needed)
Human owner creates a delegation tag exactly as NIP-26 specifies:  
`["delegation", agent_npub, "kind=30041&created_at>1730000000&content contains 'book flight' limit=300", sig]`

### 4. NIP-58 Badges (standard, no change)
- Kind 30009: Badge Definition (“verified_traveler”, “high_rep”, etc.)  
- Kind 8: Badge Award (issued by merchants or Prooff oracle after successful tx)

### 5. Verification Flow (middleware)
1. Receive event ID in MPP 402 response  
2. Query any relay for event 30040 + 30041  
3. Verify owner signature + delegation conditions  
4. Count NIP-58 badges + recent zaps → reputation score  
→ Approve payment in <100 ms

### 6. First SDK Functions (see prooff_sdk.py)
- `create_agent_manifest(...)`  
- `sign_scoped_delegation(...)`  
- `attach_to_mpp_response(...)`

Next version (v0.2) adds Lightning staking and OpenTimestamps anchoring.
