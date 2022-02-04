---
title: "Beta-2 changelog"
date: 2020-12-05T00:00:00+00:00
draft: false
---

Features:

- Failed transactions can now be rebroadcast (as long as the wallet remains open)
- Rescan spent was added to Wallet -> Advanced

Wallet:

- Wallet is now saved immediately after sending a transaction (instead of waiting up to 60 seconds)

UI:

- Home widgets can now be switched between using tabs
- Exchange integrations have been consolidated to a new "Exchange" tab
- XMRig settings UI was squished to take up less vertical space
- XMRig tab was renamed to "Mining"
- Icon for the Calc tab was changed
- Seed dialog UI was improved
- Coins: labels can now be copied from the context menu
- Percentage label is no longer displayed on balance ticker
- Message boxes now have a single button when no choice has to be made
- MorphToken: some missing trade states are now accounted for
- TransactionInfoDialog: Blockheight no longer shows zero for unconfirmed transactions
- Fiat conversion label now updates when the Send page is autofilled
- Improved Tor status on Tails in Debug info dialog
- Menu: disabled "Open wallet" action was removed

Security:

- OpenSSL updated to 1.1.1i (CVE-2020-1971)

Misc:

- Monero updated to v0.17.1.7
- Feather no longer comes bundled with a XMRig binary
