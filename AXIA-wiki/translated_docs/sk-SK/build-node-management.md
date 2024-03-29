---
id: build-node-management
title: Node Management
sidebar_label: Node Management
---

This page contains basic information about running a Axia AXIA client. There are a lot of ways to obtain/run a client, e.g. compiling from source, running in Docker, or downloading a binary. This guide will always refer to the executable as `AXIA`.

**Always refer to the client's help `AXIA --help` for the most up-to-date information.**

> Other client implementation teams: Feel free to make a PR to this page with instructions (or a link to instructions) for your client.

## Basic Node Operations

**Selecting a chain**

Use the `--chain <chainspec>` option to select the chain. Can be `AXIA`, `axialunar`, `alphanet`, or a custom chain spec. By default, the client will start AXIA. Watch [How a single codebase can power four different blockchains](https://www.youtube.com/watch?v=i9vNCHz6wO4) to learn more about how the chain selection works internally.

**Archive node**

An archive node does not prune any block or state data. Use the `--archive` flag. Certain types of nodes like validators must run in archive mode. Likewise, all [events](build-protocol-info#events) are cleared from state in each block, so if you want to store events then you will need an archive node.

> To upgrade a node, please refer to this [video](https://www.youtube.com/watch?v=5LtcdBR9F40&list=PLOyWqupZ-WGuAuS00rK-pebTMAOxW41W8&index=5)

**Exporting blocks**

To export blocks to a file, use `export-blocks`. Export in JSON (default) or binary (`--binary true`).

```bash
AXIA export-blocks --from 0 <output_file>
```

**RPC ports**

Use the `--rpc-external` flag to expose RPC ports and `--ws-external` to expose websockets. Not all RPC calls are safe to allow and you should use an RPC proxy to filter unsafe calls. Select ports with the `--rpc-port` and `--ws-port` options. To limit the hosts who can access, use the `--rpc-cors` option.

**Execution**

The Axia AXIA client implements a [AXIA Host](learn-AXIA-host) and a native runtime. The runtime must compile to WebAssembly and is stored on-chain. If the client's runtime is the same spec as the runtime that is stored on-chain, then the client will execute blocks using the client binary. Otherwise, the client will execute the Wasm runtime from the chain.

Therefore, when syncing the chain, the client will execute blocks from past runtimes using their associated Wasm binary. This feature also allows forkless upgrades: the client can execute a new runtime without updating the client.

Axia's AXIA client has two Wasm execution methods, interpreted (default) and compiled. Set the preferred method to use when executing Wasm with `--wasm-execution <Interpreted|Compiled>`. Compiled execution will run much faster, especially when syncing the chain, but is experimental and may use more memory/CPU. A reasonable tradeoff would be to sync the chain with compiled execution and then restart the node with interpreted execution.

## File Structure

The node stores a number of files in: `/home/$USER/.local/share/AXIA/chains/<chain name>/`. You can set a custom path with `--base-path <path>`.

**`keystore`**

The keystore stores session keys, which are important for validator operations.

- [AXIA documentation](learn-keys#session-keys)
- [Axlib documentation](https://axlib.dev/docs/en/knowledgebase/learn-axlib/session-keys)

**`db`**

The database stores blocks and the state trie. If you are running a validator node, it also stores GRANDPA pre-votes and pre-commits and the offchain-worker DB. Use caution when [migrating validator nodes](maintain-guides-how-to-upgrade) to avoid equivocation. If you want to start a new machine without resyncing, you can stop your node, back up the DB, and move it to a new machine.

To delete your DB and re-sync from genesis, run:

```bash
AXIA purge-chain
```

An alternative database implementation, AxiaDB, is available and activated with the `--database axiadb` option. This database uses more disk space than the default RocksBD, but is more efficient in reads and writes.

## Deployment Tools

AXIA Foundation maintains [AXIA Deployer](https://github.com/axia-tech/AXIA-deployer), which allows you to create local or remote cloud deployments of AXIA nodes. See the README for instructions.

Validators, see the [secure setup guide](maintain-guides-how-to-use-AXIA-secure-validator) for information specific to deploying validator nodes.

## Monitoring and Telemetry

**Node status**

You can check the node's health via RPC with:

```bash
curl -H "Content-Type: application/json" --data '{ "jsonrpc":"2.0", "method":"system_health", "params":[],"id":1 }' localhost:9933 
```

**Logs**

The AXIA client has a number of log targets. The most interesting to users may be:

- `afg` (Al's Finality Gadget - GRANDPA consensus)
- `babe`
- `telemetry`
- `txpool`
- `usage`

Other targets include: `db, gossip, peerset, state-db, state-trace, sub-libp2p, trie, wasm-executor, wasm-heap`.

The log levels, from least to most verbose, are:

- `error`
- `warn`
- `info`
- `debug`
- `trace`

All targets are set to `info` logging by default. You can adjust individual log levels using the `--log (-l short)` option, for example `-l afg=trace,sync=debug` or globally with `-ldebug`.

**Telemetry & Metrics**

The Axia AXIA client connects to telemetry by default. You can disable it with `--no-telemetry`, or connect only to specified telemetry servers with the `--telemetry-url` option (see the help options for instructions). Connecting to public telemetry may expose information that puts your node at higher risk of attack. You can run your own, private [telemetry server](https://github.com/axia-tech/axlib-telemetry) or deploy a `axlib-telemetry` instance to a Kubernetes cluster using [this Helm chart](https://github.com/axia-tech/axlib-telemetry-chart).

The node also exposes a Prometheus endpoint by default (disable with `--no-prometheus`). Axlib has a [vizualizing node metrics tutorial](https://axlib.dev/docs/en/tutorials/visualize-node-metrics/) which uses this endpoint.
