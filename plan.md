BOK‑coin Alpha Release: Required Code Changes

This document outlines all changes required to transform the Monero v0.18.4.4 codebase into a functional BOK‑coin alpha release suitable for internal testing among premine participants. These changes ensure network isolation, unique identity, predictable consensus behavior, and a clean foundation for future development.

1. Network Identity Changes

1.1 P2P Network Identifiers

Update the network UUIDs to prevent accidental connections to Monero nodes.

Modify src/cryptonote_config.h:

MAINNET_NETWORK_ID

TESTNET_NETWORK_ID

DEVNET_NETWORK_ID

FAKECHAIN_NETWORK_ID

1.2 Seed Nodes

Remove Monero seed nodes and leave the list empty for alpha.

File: src/p2p/net_node.inl

Set SEED_NODES to an empty list.

2. Port Reassignments

Assign unique ports to avoid conflicts with Monero.

Mainnet P2P port

Mainnet RPC port

ZMQ port

Testnet and regtest equivalents

Modify in src/cryptonote_config.h:

P2P_DEFAULT_PORT

RPC_DEFAULT_PORT

ZMQ_RPC_DEFAULT_PORT

TESTNET_P2P_PORT, etc.

3. Address Prefixes

Define unique address prefixes so BOK‑coin addresses cannot be mistaken for Monero.

File: src/cryptonote_config.h

Update:

CRYPTONOTE_PUBLIC_ADDRESS_BASE58_PREFIX

CRYPTONOTE_PUBLIC_INTEGRATED_ADDRESS_BASE58_PREFIX

CRYPTONOTE_PUBLIC_SUBADDRESS_BASE58_PREFIX

Choose prefixes that encode into a distinct starting letter.

4. Ticker and Branding

Update all user‑visible identifiers.

File: src/cryptonote_config.h

CRYPTONOTE_NAME → "bokcoin"

File: src/version.cpp

Update release name

GUI branding:

Application name

Icons and logos

Default node label

5. Emission and Premine Logic

Define the premine and emission schedule.

5.1 Premine Block

Choose a premine height (typically block 1).

Hardcode premine reward.

Hardcode premine address.

Modify block reward logic in:

src/cryptonote_basic/cryptonote_format_utils.cpp

src/cryptonote_core/blockchain.cpp

5.2 Emission Schedule

Adjust constants in src/cryptonote_config.h:

MONEY_SUPPLY

EMISSION_SPEED_FACTOR

Tail emission parameters

6. Genesis Block

Create a new genesis block unique to BOK‑coin.

6.1 Generate New Genesis Transaction

Use genesis_tx generator script or manual construction.

6.2 Update Genesis Hash

Modify in src/cryptonote_config.h:

GENESIS_TX

GENESIS_NONCE

GENESIS_BLOCK_HASH

6.3 Rebuild and verify

Ensure daemon starts with height 0.

Confirm no sync attempts to Monero.

7. Difficulty and Mining Parameters

Tune mining behavior for alpha testing.

Lower difficulty for early blocks.

Modify:

DIFFICULTY_TARGET

DIFFICULTY_WINDOW

DIFFICULTY_LAG

DIFFICULTY_CUT

8. Fee Structure

Define BOK‑coin’s fee policy.

Update constants in src/cryptonote_config.h:

Base fee

Fee per byte

Dynamic fee parameters

9. Checkpoint Removal

Remove Monero checkpoints.

File: src/checkpoints/checkpoints.json

File: src/checkpoints/checkpoints.cpp

Leave empty for alpha.

10. GUI Integration

Ensure GUI works with the modified daemon.

Update default ports

Update coin name and ticker

Replace Monero branding assets

Adjust restore height defaults

11. Build and Packaging

Prepare alpha binaries.

Build daemon and CLI

Build GUI

Package for Linux (and optionally Windows/macOS)

Provide:

monerod equivalent

bok-wallet-cli

bok-wallet-gui

12. Internal Alpha Test Checklist

Before distributing to premine participants:

Confirm daemon syncs from genesis

Confirm mining works

Confirm premine block is correct

Confirm wallets generate valid addresses

Confirm GUI connects to daemon

Confirm transactions propagate across test nodes

13. Documentation for Testers

Provide:

Instructions for running the daemon

Instructions for mining

Instructions for using the GUI

Known issues

Expected behavior

This document defines the complete set of changes required to produce a functional BOK‑coin alpha release suitable for internal testing among premine participants. All modifications should be applied to the bok-mainnet branch derived from Monero v0.18.4.4.
