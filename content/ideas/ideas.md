---
title: "Ideas"
date: 2023-02-14T00:00:00+00:00
draft: false
---

# Ideas

In no particular order.

---

**Offline transaction signing using animated QR codes**

Export/import outputs, key images and transactions using animated QR Codes based on Blockchain Common's [Uniform Resources](https://github.com/BlockchainCommons/bc-ur).

**Full node manager applet**

Bundle a tray applet that allows setting up and managing a full node.

**Better protection against memory attacks**

See: https://github.com/feather-wallet/feather/issues/72#issuecomment-1405602142

**Get ready for the migration to Seraphis**

See: https://github.com/seraphis-migration/wallet3

**Improve packaging for Linux distributions**

Write a document that should help maintainers package Feather for their distribution.

Debian package, Flatpak, Guix.

**Support monero:// URIs**

If multiple wallets are opened the user should be able to select a wallet.

**Move third-party integrations to a plugin system**

Allow third party integrations through a plugin system and host them on a plugin "store" available from within the app / Flatpak extensions.

**Add more settings that accomodate users with esoteric threat models**

Such as: lock wallet on session lock, prevent taking screenshots from app, "ephemeral mode".

**Set up a bug bounty program for issues that affect privacy or security**

Use donation funds to create an incentive for bug hunters. Set up payout tiers depending on severity.

This should give users more confidence that Feather is secure and functions properly.

**Add an advanced transaction details view**

Show inputs, rings, stealth addresses, other info.

**Generate seeds using external entropy**

Such as dice rolls.

**Allow skipping synchronization**

POV: You just opened your wallet on Tails after leaving it unopened for 3 months. You proceed to wait an hour or more for your wallet to synchronize knowing that there has been no activity. An advanced option to skip synchronization will eliminate the wait and allow you to send transactions almost immediately.

**Switch the Tor client implementation to Arti**

See: https://blog.torproject.org/announcing-arti/
