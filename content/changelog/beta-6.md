---
title: "Beta-6 changelog"
date: 2021-05-05T00:00:00+00:00
draft: false
---

Ledger support:

- The long awaited Ledger support is finally here!
- Ledger wallet synchronization is 6-7x faster than the GUI
- Wallet creation and subaddresses generation are instantaneous
- The view key is stored in the encrypted wallet file, therefore will only have to export your view key once during wallet creation
- If the device is disconnected during operation, the wallet will show a dialog and ask if you want to try to reconnect
- If an error occurs, a descriptive message is shown on how to resolve the issue
- Trezor support will be added in a future update

Updater:

- A message is shown in the statusbar when a new Feather update is available
- The download is verified against a hash signed with the release signing key
- The update will be extracted and stored in the same directory as the current binary

LocalMonero:

- Search for LocalMonero listings based on amount / currency / location / payment method
- Double-click on a listing to view offer details.

Tor networking improvements:

- The Tor info dialog was redesigned
- You can now set the host and port of the local Tor daemon
- Select between three different modes:

"Route all traffic over Tor, except traffic to node"

All traffic to the Monero Daemon RPC is routed over clearnet. This option is automatically enabled when Feather is connected to a local node.

"Route all traffic over Tor, except initial wallet synchronization"

This is the default. The wallet will sync over clearnet and automatically switch to a .onion node after this is finished. Synchronization requires a lot of data transfer is therefore very slow over Tor.

"Route all traffic over Tor"

This option is automatically enabled on Tails and Whonix or when Feather is started using Torsocks.

- When Feather is started for the first time, you will have the chance to configure the Tor settings and node before any network connections are made


Misc:

- Lots of refactoring
- Monero updated to v0.17.2.0
- Balance displayed in the status bar is no longer rounded
- Fixed an issue that could cause the History tab to be incomplete after wallet sync
- Fixed an issue that could cause Feather to crash after opening a wallet
- Libwallet: The client secret is reset upon connecting to a different node
- Libwallet: fixed an issue that could cause Feather to get stuck while creating a transaction
