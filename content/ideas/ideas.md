---
title: "Ideas"
date: 2023-02-14T00:00:00+00:00
draft: false
---

# Ideas

In no particular order.

---

**Practical Multisig UX**

Replace PyBitmessage as the message transport layer for the MMS with a new self-hostable message service, rethinking multisig wallet setup and abstracting away the MMS in wallets as much as possible.

**Integration testing**

Set up an extensive integration test suite for Feather that covers privacy & security guarantees and performance.

**Improve documentation**

Write accessible threat modeling documentation; mapping specific concerns to actions users can take.

**Monero Improvement Proposals**

Set up a BIP equivalent for Monero wallet standards.

**Full node manager applet**

Bundle a tray applet that allows setting up and managing a full node.

**Better protection against memory attacks**

See: https://github.com/feather-wallet/feather/issues/72#issuecomment-1405602142

**Add more settings that accomodate users with esoteric threat models**

Such as: lock wallet on session lock, prevent taking screenshots from app, "ephemeral mode".

**Add an advanced transaction details view**

Show inputs, rings, stealth addresses, other info.

**Allow skipping synchronization**

POV: You just opened your wallet on Tails after leaving it unopened for 3 months. You proceed to wait an hour or more for your wallet to synchronize knowing that there has been no activity. An advanced option to skip synchronization will eliminate the wait and allow you to send transactions almost immediately.
