---
title: "Beta-5 changelog"
date: 2021-03-24T00:00:00+00:00
draft: false
---

**Notice**:

- Windows users: The default config directory was changed to `%appdata%/FeatherWallet`. If you wish to retain your old config, move the contents of `%userprofile%\.config\feather` to the new location.

History page:

- Columns can now be hidden and reordered
- Right click the table header for options
- History layout is persisted in the config
- A txid column was added (hidden by default)
- Amount sort should now work as expected
- Tx description of pending transactions can now be changed
- Amount precision as displayed in the amount column can be changed in settings
- The fiat column once again shows the historical fiat price (instead of "?")
- Description can now be copied from the context menu
- Pressing Ctrl+C with a tx selected will now copy its txid to the clipboard
- Date and time format can now be configured in the settings

Transaction proofs:

- A new dialog to create transaction proofs was added
- To access it right click on a transaction -> "Create tx proof" or via the transaction information dialog.
- Proofs can now be output in a new PGP-like message format.
 - It contains all the necessary information to verify the proof signature, including an optional message.
- Formatted proofs can be verified easily in the "Tools -> Verify transaction proof" dialog.
- Descriptive messages will be shown when generating a particular tx proof is not possible due to missing tx key, or other issue.

Transaction info:

- Information shown in the tx info dialog is now more descriptive
- The old transaction proof creation widget was removed
- Multiple tx info dialogs can now be open at the same time

Receive page:

- Subaddresses can now be hidden
- Checkboxes were added to show used / hidden subaddresses

Send page:

- The "Pay to" field on the send tab should no longer have clipping issues
- Pressing tab on the "Pay to" field will now move the cursor to the next field instead of inserting a tab character

Sweep output dialog:

- When splitting an output a estimated output calculation is shown. e.g. "1 XMR ≈ 5x 0.2 XMR"

Networking:

- The network status is more responsive
- The dreaded "? blocks remaining" message should be gone for good
- Closing the wallet during synchronization will now immediately close the wallet (Network reads are cancelled on wallet close)
- Fixed an issue that could cause the wallet to erratically switch between nodes (common on Whonix)
- Refreshing is paused during transaction construction
- Transaction construction now requires an order of magnitude less bandwidth (The output distribution is cached and supplemented when needed)

Multibroadcasting:

- To further mitigate the "double spend" issue that is caused by poor transaction propagation, outgoing transactions are now broadcast to all available websocket nodes over Tor by default, in addition to the current node.
- This feature can be disabled in the settings

Wizard:

- The UI flow was improved
- It is now possible to specify a seed offset passphrase when restoring a seed
- A dark mode toggle was added to the menu
- Fixed an issue that could trigger a crash after pressing the Generate button on the create seed page
- Now automatically suggests a wallet name that hasn't been used when creating a new wallet
- Setting a password now requires entering it twice

Portable mode:

- To enable portable mode create a file named ".portable" in the same directory as the executable
- All configuration and wallet files will be saved relative to the executable in "./feather_data" by default

Torsocks:

- Fixed an issue that prevented Feather from loading Reddit/CCS/Fiat prices, etc.
- When Feather is started with Torsocks .onion nodes will be used

Misc:

- Tor was updated to 0.4.5.7
- Restore height for newly created wallets is set to latest blockheight (if available)
- Trying to open the seed dialog on view-only or non-deterministic wallets will now show a message that a seed is not available
- Fixed an issue that could cause debug info dialog to block UI
