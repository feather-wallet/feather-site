---
title: "Beta-8 changelog"
date: 2021-06-11T00:00:00+00:00
draft: false
---

Features & Improvements:

- Trezor hardware wallet support
- Webcam QrCode scanner
   - Note: the scanner is not currently supported on Linux standalone binaries. Download the AppImage if you are on Linux and want to use this feature.
- You can now paste a QrCode image directly into the Pay To field and it will automatically fill in the address (and description, amount if it is a payment request):
- QrDialog: QrCode now scales with the size of the dialog.
- Multi-input sweeps: You can now select multiple coins in the Coins to sweep in a single transaction.
- Coins can now be labeled.
   - Note: labels are currently tied to the transaction description. Let me know if your use-case requires finer control.
- A search bar was added to the Coins tab.
- Unconfirmed transactions are now stored in the wallet cache and can be rebroadcast after the is re-opened. Prior to this update rebroadcasting was only possible as long as the wallet remained open after sending a transaction.
- During initial setup the presence of local node on the default port is detected. Feather will automatically suggest connecting to it instead of a third party node.
- Added a tool to check if an address: 1) is valid, 2) belongs to the currently opened wallet, 3) which subaddress account it belongs to. You can access it by going to Tools -> Address checker.
- The list of mining pools in the Mining tab can now be configured.
- Updated built-in Tor to v0.4.6.6 (Windows / Linux)
- Linux release builds should now be permanently reproducible
- A button to show transaction details was added to the dialog that appears after successfully sending a transaction.
- Password entry is now required to open the seed, keys dialog.
- Dummy outputs are now labeled in the advanced transaction confirmation dialog.
- Added an "Open link" button on various message boxes that include a link
- Updated the hardcoded node list: added more high performance nodes and removed dead nodes and v2 hidden services
- Contacts: switch name and address column
- The list of recently opened wallets can now be cleared.
- Reduced AppImage size by 22% (31 -> 24 MB)
- Reduced standalone Linux binary size by 35% (76 -> 49 MB)

Fixes:

- Fixed an issue that could cause the wizard to stay open after opening a wallet
- --stagenet and --testnet command line flags work again
- Closing a wallet will no longer cause the wizard to hang temporarily
- Fixed an issue that could cause a wallet to fail to open if the wallet cache was missing
- Minor UI fixes
