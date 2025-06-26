
# sBTC-NFTrek - Geotagged NFT Rewards Smart Contract

This Clarity smart contract implements a **location-based rewards system** using geotagged NFTs on the Stacks blockchain. Users complete real-world activities at verified GPS locations to earn points, which can later be redeemed to mint unique NFTs as proof of accomplishment.

---

## 🧱 Contract Overview

* **NFT Type**: `geotagged-nft`
* **Reward Mechanism**: Earn points by visiting and completing activities at real-world locations.
* **NFT Minting**: Redeem earned points to mint a non-fungible token as a collectible or proof-of-completion.
* **Admin Controls**: Only the contract owner can add/update locations.

---

## 📦 Key Features

* ✅ Geolocation-validated activity completions
* ✅ Point-based reward system
* ✅ Secure and controlled NFT minting
* ✅ Prevents duplicate submissions and self-point transfers
* ✅ Location status toggling (enable/disable)
* ✅ Transferable user points

---

## 🗂 Data Structures

### 📍 `locations`

Stores metadata for a location.

```clojure
(id) => {
  name: string,
  lat: uint,
  long: uint,
  activity: string,
  reward-points: uint
}
```

### 🧾 `user-completions`

Tracks whether a user has completed a location activity.

```clojure
({ user, location-id }) => {
  completed: bool,
  timestamp: uint
}
```

### ⭐ `user-points`

Tracks the reward points of each user.

```clojure
(user) => points (uint)
```

### 🛑 `location-status`

Tracks whether a location is currently active.

```clojure
(location-id) => true | false
```

### 🏁 `minted-nfts`

Tracks which NFT token IDs have been minted.

```clojure
(token-id) => true | false
```

---

## 🛠 Admin Functions

### `add-location(id, name, lat, long, activity, points)`

Adds a new geotagged location.

* 🔒 **Owner-only**
* 📍 Max 1000 locations
* 🚫 Fails if ID already exists

---

### `set-location-status(location-id, active)`

Enable or disable a location.

* 🔒 **Owner-only**

---

## 🎮 Core User Functions

### `complete-activity(location-id, user-lat, user-long)`

Validate proximity to a location and record completion.

* 🌍 User must be within `min-distance` (100 units)
* ✅ Adds reward points to user

---

### `mint-nft(token-id)`

Mints a geotagged NFT if user has ≥ 100 points.

* 🚫 Fails if NFT ID already used
* 💰 Deducts `points-threshold` (100 points)

---

### `transfer-points(recipient, amount)`

Transfers points to another user.

* 🚫 Self-transfers and zero-amounts are rejected
* 💼 Useful for collaborative team goals

---

## 🔍 Read-only Functions

### `get-location(id)`

Returns metadata for a location.

---

### `get-user-points(user)`

Returns point balance for a user.

---

### `get-completion-status(user, location-id)`

Returns whether the user completed the activity.

---

### `is-nft-minted(token-id)`

Checks if a given NFT has been minted.

---

### `get-location-status(location-id)`

Returns active/inactive status of a location.

---

### `get-user-stats(user)`

Returns:

```clojure
{
  points-balance: uint,
  can-mint-nft: bool
}
```

---

## ⚠️ Error Codes

| Code                          | Error                          |
| ----------------------------- | ------------------------------ |
| `err-owner-only`              | Caller is not contract owner   |
| `err-invalid-location`        | Location doesn't exist         |
| `err-already-completed`       | Activity already completed     |
| `err-insufficient-points`     | Not enough points for NFT      |
| `err-invalid-coordinates`     | Lat/Long out of bounds         |
| `err-location-limit-exceeded` | Too many locations             |
| `err-nft-already-minted`      | NFT ID already used            |
| `err-zero-amount`             | Transfer amount is zero        |
| `err-self-transfer`           | Cannot transfer points to self |
| `err-invalid-user`            | Invalid recipient for transfer |
| `err-location-disabled`       | Location is currently inactive |

---

## 🛡️ Validation Logic

* Latitude must be ≤ 90M; Longitude ≤ 180M (scaled for precision)
* Token IDs must be in range `1–1000`
* Points per activity ≤ 1000
* Name and activity must not be empty
* Transfers must not be to self or owner

---

## 🌟 Use Cases

* Geo-based scavenger hunts
* Fitness challenges with map checkpoints
* Environmental cleanup missions (e.g., “clean this park”)
* Cultural exploration apps
* Tourism incentive systems

---
