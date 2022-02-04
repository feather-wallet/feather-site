---
title: "Beta-7 changelog"
date: 2021-05-27T00:00:00+00:00
draft: false
---

Features:

- Multiple wallets can now be opened at once (File -> Open wallet)
- Added support for subaddress accounts (Wallet -> Account)
- Tray icons are now enabled on Windows and macOS
- A list of recently opened wallets was added to the File menu
- Fiat and cryptocurrencies shown in the Calc tab are now configurable
- Choose between 200+ fiat currencies and 100+ cryptocurrencies.
- Balance display in status bar is now configurable in settings
- The seach bar on the History, Send and Receive tabs can now be toggled with Ctrl+F
- A threshold for clearnet synchronization can now be set for the "All traffic over Tor, except initial wallet sync" option in Tor settings

Bugfixes:

- Fixed an issue that could cause newly created wallets to sync from genesis
- Fixed an issue that could cause the 25 word seed to be missing from the seed dialog
- Fixed an issue that could affect the reproducibility of AppImage builds
- Manual transaction import and broadcasting should now work when connected to a local node
- The definition of a local node was expanded to include nodes on local networks
- Balance shown in the statusbar is now affected by the amount precision setting
- Sorting the History tab by date should now properly sort the items
- LocalMonero: disable search button during search
- The amount field in the Send tab is no longer cleared when the Pay to field is edited

Misc:

- Added wallet creation date to debug info
- The title now shows the active subaddress account index
- Use white icons on the Calc and LocalMonero tab if dark mode is active
- Added a button to copy the address in the Transaction Proof dialog
- Hovering over the Qr code on the Receive tab now shows a pointer cursor to indicate that it is clickable
- Added a link with instructions on how to setup udev rules for Ledger users on Linux
- Fixed an issue that could cause buttons to squish in dark mode
- Fixed a compilation issue with GCC 11
- Fixed various compiler warnings
