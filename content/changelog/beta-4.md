---
title: "Beta-4 changelog"
date: 2021-02-06T00:00:00+00:00
draft: false
---

Features:

- Multi-destination transactions
- Windows release binaries are now reproducible
- AppImages are now reproducible
- Seed: enable erasures (replace a word with "xxxx" and recover the full seed)
- XMR.to exchange integration was removed

Wallet:

- RandomX is no longer linked (should help mitigate against AV false positives)
- Speedup wallet restores by skipping unneeded blocks in fast refresh
- Include unconfirmed payments in balance calculation

UI:

- Color scheme was improved
- The Home tab is now hideable
- Statusbar: Balance is displayed more concicely
- Wizard: sort wallets by last modified
- Wizard: seed display was improved
- Wizard: allow double clicking to open wallet
- Wizard: add button to copy seed to clipboard
- History: a notice is shown when the wallet is still synchronizing
- Settings: You can now select the Reddit frontend to use when opening a link
- Reddit: Right click -> Copy link
- Whonix: whonix version is detected and shown in the debug dialog
- TxConf: show message when churn transaction detected
- TxInfo: UI was improved
- History: allow filtering by subaddress label

Bugfixes:

- Send: don't lose precision on amounts
- Clear all tables when wallet is closed
- Nodes: fallback to hardcoded list if no nodes were previously cached
- Nodes: don't show the exhaustion warning in some scenarios
- Coins: fix an issue that could cause freeze/thaw to select the wrong index
- Fix a crash that could occur when the wizard is closed after trying to open a wallet
- Update balance immediately after sending a transaction
- Always store wallet on exit
- Update the Tor binary on filesystem if embedded version is higher

Misc:

- Monero updated to v0.17.1.9
- Tor updated to 0.4.5.5-rc
- Wallet cache debug dialog added
- Various build system improvements
- Reduced Windows binary size by 60%
