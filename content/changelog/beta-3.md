---
title: "Beta-3 changelog"
date: 2020-12-25T00:00:00+00:00
draft: false
---

Features:

- MorphToken integration was removed (they now block all Tor traffic).

Wallet:

- Wallet is now saved immediately upon finishing wallet refresh
- Feather will no longer sometimes display subaddresses belonging to non-primary accounts for wallets that were created with the GUI
- Websocket connection is now kept alive

UI:

- Wizard: Open wallet page was redesigned
- Wizard: New banner design
- Wizard: "Open wallet" is now autoselected if any wallets are detected
- Status: Wallet refresh shows blocks remaining instead of absolute values
- Status will now show an animated message during transaction construction
- Home: Text in table widgets is now word wrapped
- DebugInfo: Websocket status message is more concise
- DebugInfo: A new entry was added that shows if the wallet only contains funds in the primary account
- History: Formatting of fiat amounts is now locale-aware
- History: It's no longer possible to rebroadcast incoming transactions
- TxInfo: Tx proofs now have clickable help labels with information about what each proof proves
- Contacts: It's no longer possible to add duplicate addresses or labels
- Send: "Pay to", "Description" and "Amount" are now clickable help labels
- Amounts are now justified in the transaction confirmation dialog
- External link warning no longer warns about not using Tor on Tails/Whonix
- Non-breaking spaces were removed from currency strings
- Fully funded CCS proposals are no longer hidden
- Fixed a bug that could cause the send button to remain disabled after a new wallet is opened
- Attempting to send a transaction when the wallet is not connected to a daemon will now show an error
- Nodes: double click on a node to connect
- Clicking the balance label will now pop up a dialog with a more detailed balance overview
- The default wallet directory can now be changed

Misc:

- Minor build speedup
