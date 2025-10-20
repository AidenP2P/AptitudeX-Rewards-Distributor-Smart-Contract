# SpecialRewardsDistributor — One-Time APX Rewards (Quiz/Social/Contests)

`SpecialRewardsDistributor` lets you create and manage **one-time** APX rewards (e.g., quizzes, social actions, contests).  
Admins configure timed rewards with optional claim caps; users can claim **once per reward** if eligible.

> Complements a daily/weekly claim flow (e.g., `ClaimDistributor`) with ad-hoc campaigns.

---

## Features

- Create time-boxed rewards with amount, type, JSON requirements, and optional max claims
- One claim per user per reward; contract tracks total claimed
- Admin progress updates for quizzes/challenges (scores, etc.)
- Toggle reward active/inactive, update reward details
- Provision contract with APX and emergency withdraw
- Secured by `Ownable` and `ReentrancyGuard`

---

## Contracts & Versions

- Solidity `^0.8.0`
- OpenZeppelin:
  - If **OZ v5+**: `Ownable(initialOwner)` exists and `ReentrancyGuard` is in `utils/`.
  - If **OZ v4**: `Ownable` has no constructor arg and `ReentrancyGuard` is in `security/`.

**This repo uses:** `Ownable(_owner)` → **OZ v5+** is recommended.  
If you stay on OZ v4, change the imports and adapt the constructor accordingly.

---

## Data Model

```solidity
struct SpecialReward {
  uint256 amount;       // APX wei amount per claim
  uint256 startTime;    // campaign start (unix)
  uint256 endTime;      // campaign end (unix)
  bool    isActive;     // enabled/disabled
  string  rewardType;   // "quiz" | "social" | "contest" | "base_batches" | ...
  string  requirements; // JSON payload describing rules/metadata
  uint256 totalClaimed; // number of successful claims
  uint256 maxClaims;    // 0 = unlimited; else hard cap
}

struct UserProgress {
  uint256 totalSpecialClaimed;   // total APX (wei) claimed via specials
  uint256 completedRewardsCount; // number of rewards claimed
}
