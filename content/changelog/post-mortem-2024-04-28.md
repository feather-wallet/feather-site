---
title: "Incident report: Denial-of-Service (28 April 2024)"
date: 2024-05-01T00:00:00+00:00
draft: false
---

### Summary

**Feather versions `2.6.5` and below are no longer able to send transactions. Upgrade Feather to `2.6.6` or above to fix the issue.**

Users trying to construct a transaction on `2.6.5` and below are presented with the following error message: "*Internal error: Unrealistic number of outputs in output distribution*".

{{< figure src="/img/output_distribution_error.png" link="/img/output_distribution_error.png" class="featureimg" >}}

The bug was triggered when the number of RingCT outputs on the blockchain exceeded 100 million. This caused a sanity check to fail during transaction construction, resulting in the error shown above.

For approximately 5 hours on April 28, 2024, all available versions were affected by the bug, effectively preventing anyone from transferring funds using Feather during that time.

### Cause

On August 29, 2021, I committed [a patch](https://github.com/feather-wallet/monero/commit/f5b91c6b) titled 'Output distribution checks' to Feather's fork of the core Monero wallet library. This patch was first included in Feather `beta-9`, which was released on September 16, 2021.

The purpose of this patch was to provide better mitigations against an attack known as "output distribution poisoning".

The output distribution is a histogram of RingCT outputs and affects how the wallet selects decoys for rings. When a transaction is constructed, the wallet requests the output distribution from the node it is connected to. 

In the attack, a malicious remote node manipulates the output distribution in a way that reveals the *true spend* to the node with a high degree of certainty. One way a node could do this is by pretending there are significantly more outputs on the blockchain than there are in reality. This causes decoys, which are referenced by their global index, to skew towards this 'unrealistic' number.

Consider the following set of output indices which represent a ring, for which the wallet requests the associated public keys from the node:

```
{99655570, 50001410564, 50001585151, 50001818442, 50001853725, ...}
```

The node can clearly tell that the first output is the true spend, because it is the only 'real' output that exists on the blockchain.

This could allow the node to associate an output with an IP address. Note that, by default, Feather constructs transactions over Tor, and so the only information a node would learn is that "someone, using Tor, is currently trying to spend this output". By default, Feather connects to a node from a list of nodes operated by known members of the community. If a node from this list was reported to be malicious, it can quickly be removed and the operator shunned.

In this naive approach, the transaction would be rejected by the network because the ring contains invalid indices. The user would receive an error message upon broadcasting the transaction and promptly switch to a different node.

A more sophisticated attacker could present the output distribution in a way where decoy selection is skewed such that the true spend is detectable, but does not result in an invalid transaction.

For instance, [this](https://xmrchain.net/search?value=0254383417947c386cc58b1a8bed9c6264566992f1825000a39805f1df901ab6) transaction was constructed using a manipulated output distribution. Can you determine what the true spend is? Notice that all ring members are older than 1y 200d except for one 6-day-old output. Unless the user checks the ring on a block explorer and knows what to look out for, they would not notice that their transactions are being fingerprinted. 

Even subtler manipulation may give the malicious node only a statistical edge to guess the true spend, but not noticeably fingerprint the transaction on-chain to a casual observer. 

The patch attempted to mitigate this attack with two checks:

1. It hashes the output distribution returned by a node up until a checkpoint height and compares it against a hardcoded hash. If the hashes don't match, an error is returned. This prevents any tampering with the distribution before the check-pointed height. 
2. It [checks](https://github.com/feather-wallet/monero/commit/f5b91c6b#diff-04cf14f64d2023c7f9cd7bd8e51dcb32ed400443c6a67535cb0105cfa2b62c3cR3831-R3832) if the total number of outputs in the distribution does not exceed a predefined limit. This 'sanity check' is what ultimately caused transaction construction to fail for everyone when the real number of outputs on the blockchain exceeded this limit.

Upon further review, the first check appears to be an effective measure, as long as it is updated with each release. It limits how much an attacker is able to skew decoy selection. The second check appears to be ineffective, as an informed attacker could still skew the distribution to *just below* the limit. It would only have caught an attacker that was unaware of the limit.

I initially stated that "a large number of outputs were added last month in a spam attack", believing the patch was a more recent invention and thought the spam attack had contributed more significantly to expediting the timeline. After gathering the details, I found that the attack only contributed about 2.5 months' worth of outputs to the chain. Moreover, the possibility of such an attack should have been taken into account when considering if was a good idea to impose any such limit in the first place.

- At the time of the patch (Aug 2021) roughly 48.8 million RingCT outputs existed on the Monero blockchain. The limit was set to 100 million.
- The limit and the hardcoded hash were never updated from their initial values.
- By March 3, 2024, at the start of the March spam attack, the number of RingCT outputs on the Monero blockchain had already grown to 90.8 million.
- According to [research](https://github.com/Rucknium/misc-research/blob/main/Monero-Black-Marble-Flood/pdf/monero-black-marble-flood.pdf) by Rucknium, the spam attack added about 4.62 or 4.71 million outputs depending on which definition is used for the spam transactions.
- Updating the limit or hardcoded hash was not documented in the [release procedure](https://github.com/feather-wallet/feather/blob/master/RELEASE.md) or [maintenance priority](https://github.com/feather-wallet/feather/blob/master/MAINTENANCE.md) and the value was not actively monitored. These documents were added precisely to prevent something like this from happening, but were only introduced in Jan 2023. By then, the existence of the patch must not have been top of mind.
- On August 28, 2024, block 3137429 was mined which pushed the number of RingCT outputs over the 100 million limit, triggering the sanity check to fail during transaction construction.

Plans to clean up Feather's patchset with the goal of "upstreaming as much as possible" were on the agenda. I believe I would have removed the limit if I had worked on this sooner, however this obviously did not happen before the limit was reached. Following the [CCS hack](https://www.getmonero.org/2023/11/04/ccs-wallet-incident.html), I prioritised work to help ensure the continuity of the CCS, which included finalising support for offline transaction signing with animated QR codes and ongoing work to integrate multisig. This lead to delays in maintenance work.

I conclude that the sanity check should not have been added, and that my failure to properly document, monitor, and maintain the patch ultimately lead to the denial of service.

### Response Timeline

#### 2.6.6 

`20:40` - User `miracloe` joins the irc channel and is the first person to report that they encountered the error message.  
`20:46` - I am able to reproduce the error message by attempting to send a test transaction.

To prevent this from happening in the future, the sanity check is removed.

`21:41` - The `2.6.6` release is tagged and building begins.  
`00:04` - The builds are finished and transferred to the release signing machine.  
`00:13` - Builds are signed and uploading begins.  
`01:15` - Uploading is finished and the release is available on the site and in the built-in updater.

| Report ➛ Tag | Tag ➛ Release | Report ➛ Release |
| ------------ | ------------- | ---------------- |
| 1h 1m        | 3h 34m        | 4h 35m           |

'**Report to Tag**' is the amount of time it took to reproduce, debug and fix the issue.  
'**Tag to Release**' is the amount of time it took to release the update, which includes building, signing, and uploading the release artifacts and updating the website.

For most users this concluded the wait, others had to wait until `2.6.7`.
#### 2.6.7

`01:49` - User `rayven123` reports on irc they are unable to open `2.6.6` on Tails 5.  
`01:57` - I am able to reproduce the issue by attempting to open `2.6.6` on Tails 5. 

This problem turned out to be related to a compiler upgrade, and was unrelated to the fix for the output distribution issue. It had existed in the `master` branch since March 18, but went unnoticed because the issue did not affect my development machine. 

Since all versions prior to `2.6.5` were no longer able to send transactions and `2.6.6` did not run on some Linux machines, another release was needed.

During this period several other users reported that they are unable to start `2.6.6` on various Linux distributions, including Whonix and Fedora 40. Another user reported an issue with multi-destination transactions, which took a while to debug, but was ultimately unrelated to the release.

`04:37` - The `2.6.7` release is tagged and building begins.  
`04:50` - Exhausted, I announce that I'm going to head off to get a few hours of sleep.  
`08:18` - I wake up, and begin uploading builds.  
`08:48` - The `2.6.7` release is now available on the site and in the built-in updater.

| Report ➛ Tag | Tag ➛ Release | Report ➛ Release |
| ------------ | ------------- | ---------------- |
| 2h 48m       | 4h 11m        | 6h 59m           |

### Verifying reproducibility post-release

For typical releases, at least two people must submit a signed list of hashes, known as an [attestation](https://github.com/feather-wallet/feather-sigs). This is done to verify that builds are [reproducible](https://dl.gi.de/server/api/core/bitstreams/f8685808-2e51-4a53-acc0-2b45fa240e3b/content). If the release is not reproducible, the release process can't continue; all sources of nondeterminism must be removed and new release must be tagged.

For an emergency release, there is no time to wait for attestations from multiple people. The requirement was lifted for `2.6.6` and `2.6.7`.

Luckily, both releases were verified to be reproducible post-release. At the time of writing, `2.6.6` was built by `plowsof` and me, and `2.6.7` was built by `plowsof`, `MoneroArbo` and me, with all hashes matching.

This begs the question as to what would have happened if the builds turned out to be non-reproducible post-release. 

In simple cases, like where nondeterminism is trivially attributable to a benign source like an embedded timestamp, an erratum may be published in [ERRATA.md](https://github.com/feather-wallet/feather-sigs/blob/main/ERRATA.md). This document was introduced after `2.6.5`, which contained an issue with the attestation script.

To describe the defect, a tool like `diffoscope` may be used to show the binary difference between two builds. This gives verifiers confidence that the release was built from its source code.

If the binary diff is too complex to describe in an erratum, a warning must be issued and the release must be pulled immediately after a new version is available.

### Lessons learned

#### On sanity checks

The main takeaway is to never add sanity checks that may fail at some unknown time in the future, especially when that could prevent essential wallet functionality from working properly. To my knowledge, there are no checks like that in the latest version.

While this was not the intent of the sanity check, I do think it may be useful to have a built-in 'expiration date' for a release. Newer releases include bug fixes, improve privacy and security, and patch vulnerabilities (in Feather, Monero, or any of its statically linked dependencies). It is important that users keep the software up-to-date.

A warning could be shown if a fixed amount of time (e.g. 1 year) has passed since the release and prompt the user to consider updating. The user could choose to hide the warning, and continue using the wallet if they do not want to update for whatever reason.

#### On emergency releases

This was the first emergency release in Feather's history.

Emergency releases are reserved for two scenarios:

- A critical vulnerability that could result in a loss of funds.
- A denial of service of essential wallet functionality (sending and receiving) that affects the latest Feather version on at least one major platform.

Occasionally, there has been a need for 'fast' releases. Like, `2.6.3` which fixed an issue that prevented Feather from running properly on Tails 6, or `2.6.4` which fixed automatic fee adjustment during the aforementioned spam attack. In both cases, there was some sense of urgency, but the problems were not severe enough to qualify for an emergency release. Fast releases must still adhere to the [release procedure](https://github.com/feather-wallet/feather/blob/master/RELEASE.md), but may be announced 'on the fly' when a situation arises.

An emergency release *should* branch off from the previous release, and only include the critical fix and potentially other *important* fixes that were pending for the next release.

This minimizes the chance of introducing new bugs. Typical releases are tested before they are tagged to check if the complete set of changes does not create any new issues. While this must also happen for an emergency release to ensure that the fix actually works, there is not enough time to test all pending changes.

In case any build system changes or packages updates were pending to go into the next release, it also minimizes the chance of introducing nondeterminism into the build process.

Regrettably, I made the decision to base the `2.6.6` emergency release on `master`, instead of `2.6.5`. The `2.6.6` release contained 20 commits since `2.6.5`, which other than the critical fix were all pending changes for the next release and included build system changes, low to medium priority fixes, and a new feature that I had recently tested.

The [critical fix](https://github.com/feather-wallet/monero/commit/376fb747ea262cf6cd773cc169bbd3e84670d733) removed three lines of code (including an empty line and a comment). Had only this fix been included, it would also have been easier for others to compare the source code of `2.6.5` and `2.6.6` and be confident that the emergency release wasn't used as an opportunity to introduce malicious code into the source code.

The build system changes ended up causing the bug that resulted in the need for a second emergency release and could also have easily caused nondeterminism. Should an emergency release be needed in the future, I will not make this mistake again.  On the upside, much requested fee changes made it into `2.6.6`. 

#### On the release procedure

Let's break down how long each stage of the release procedure for `2.6.6` took and discuss ways in which this it can be optimized.

| Building | Signing | Uploading | Total  |
| -------- | ------- | --------- | ------ |
| 2h 23m   | 0h 9m   | 1h 2m     | 3h 34m |

{{< figure src="/img/release_process_gh.png" link="/img/release_process_gh.png" class="featureimg" >}}

The total size of release artifacts for any release is about 1000 MB. Signed hashlists and signatures only make up 150 KB of that. 

To maintain anonymity, releases are uploaded over Tor to the webserver from a dedicated "upload machine". Uploading a gigabyte worth of data over Tor can take a while and upload speed can vary widely. The `2.6.6` release took over an hour to upload, and the `2.6.7` release 'only' took 30 minutes. If the Tor network has [degraded performance](https://status.torproject.org/issues/2022-06-09-network-ddos/), uploading a release could take hours or days. Clearly, in scenarios where time to release must be minimized, any added wait is undesirable.

When a release is tagged, a GitHub CI job begins building it. Thanks to reproducible builds, the artifacts produced by GitHub will match local builds bit-by-bit (unless a source of non-determinism was introduced).

It took the CI job 2h 20m to finish the builds, which is roughly the same amount of time it takes me to build locally. Despite the low thread count available to GitHub runners and lack of caching (due to storage constraints), it greatly benefits from parallelization by building each target on a different runner. In contrast, local builds are sequential, meaning one target is built after another. Any moment where the local build process is not able to utilize all available cores (e.g. during package configure stages) it loses out to parallelized builds.

To decrease the time it takes to complete the upload stage of the release process, only the signed hashlists and signatures could be uploaded to the webserver. It can then fetch the artifacts from GitHub and verify the signatures. Since it can do this over clearnet, this is expected to take only a minute or two. This would have saved nearly an hour on the time it took to get the `2.6.6` release out.

A lower bound of 2h 20m for Tag to Release still isn't ideal. Breaking down the CI run for the slowest target, we find that time is spent on the following tasks:

| Step                        | Time   | Cacheable |
| --------------------------- | ------ | --------- |
| Setting up the runner       | 0h 2m  | No        |
| Setting up Guix             | 0h 12m | Maybe     |
| Building Guix derivations   | 1h 00m | Yes       |
| Building `depends` packages | 0h 48m | Yes       |
| Building Feather            | 0h 18m | No        |

Most time is spent re-building stuff that could have been cached. We should be able to fit all cached data within GitHub's 10 GB cache limit, if we only keep caches for the latest tag. With caching, the lower bound becomes 20 - 44 minutes.

We can't rely on GitHub solely for builds, so a way to parallelise local builds must also be developed. Typical releases would also benefit from this. 

#### On communication

Information about the incident could have been communicated to users more effectively.

Here is where information could have been posted:

- A notice on the website / RSS feed
- The description of the irc / matrix channel
- An issue on GitHub
- A post on [monero.town](monero.town) and X

I did not have an automated way to quickly post an update across these channels and did not want to detract time from getting the release out, so I did not post an update everywhere. A script on the "upload machine" could have taken care of posting a short message to all channels, so I will work to set that up.

Thanks to SummerBreeze for creating a thread on monero.town and x011 for creating an issue on GitHub during the incident.