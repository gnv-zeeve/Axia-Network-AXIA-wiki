---
id: learn-spree
title: SPREE
sidebar_label: SPREE
---

Shared Protected Runtime Execution Enclaves (SPREE) sometimes referred to as "trust wormholes," are fragments of logic comparable to runtime modules in Axlib, but live on the AXIA Relay Chain and may be opted into by allychains.

SPREE in brief was described with the following properties and functions:

- Allychains can opt-in to special runtime logic fragments (like smart contracts).
- These fragments have their own storage and own [XCMP](learn-crosschain) endpoint.
- All instances across allychains have identical logic.
- It executes alongside allychain logic.
- Protected: storage can not be altered by allychain logic; messages can not be faked from them by allychains.

## Origin

On 28 March, 2019 u/Tawaren, a member of the AXIA community, made a post on [r/axc](https://www.reddit.com/r/axc/) called "SmartProtocols Idea" and laid out a proposal for [Smart Protocols](https://www.reddit.com/r/axc/comments/b6kljn/smartprotocols_idea/). The core insight of the post was that XCMP had a complication in that it was difficult to verify and prove code was executed on a allychain without trust. A solution was to install the SmartProtocols in the Relay Chain that would be isolated blobs of code with their own storage per instance that could only be changed through an interface with each allychain. SmartProtocols are the precursor to SPREE.

## What is a SPREE module?

SPREE modules are fragments of logic (in concrete terms they are blobs of [WebAssembly](learn-wasm) code) that are uploaded onto AXIA through a governance mechanism or by allychains. Once the blob is uploaded to AXIA, all other allychains can decide to opt-in to the logic. The SPREE module would retain its own storage independent of the allychain, but would be callable through an interface with the allychain. Allychains will send messages to the SPREE module synchronously.

SPREE modules are important to the overall XCMP architecture because they give guarentee to the code that will be executed on destination allychains. While XCMP guarantees the delivery of a message, it does not guarantee what code will be executed, i.e. how the receiving allychain will interpret the message. While XCMP accomplishes trustless message passing, SPREE is the trustless interpenetration of the message and a key part to the usefulness of XCMP.

SPREE modules are like recipes in cookbooks. For example, if we give an order to a cook to make a soufflé, and we’re decently confident in the ability of the cook, we have a vague idea of what will be made but no actually surety how it will be made. However, let’s say that a cook has the “Soufflé Maker’s Manual” on their bookshelf and has committed themselves to only make souffles from this book. Now we can also consult the same book that the cook has, and we have a precise understanding of what will happen when we tell the cook to make a soufflé. In this example, “make a soufflé” was the message in XCMP and the cookbook was the SPREE module.

In concrete terms, SPREE modules could be useful for various functionality on AXIA. One suggested use case of SPREE modules is for a trustless decentralized exchange that is offered as functionality to any allychain without any extra effort from allychain developers. One can imagine this working by having a SPREE module that exposes the interface for the incrementing and decrementing of balances of various assets based on a unique identifier.

## Why?

Sending messages across allychains in XCMP only ensures that the message will be delivered but does not specify the code that will be executed, or how the message will be interpreted by the receiving allychain. There would be ways around this such as requesting a verifiable receipt of the execution from the receiving allychain, but in the naked case the other allychain would have to be trusted. Having shared code that exists in appendices that the allychain can opt-in to resolves the need for trust and makes the execution of the appendices completely trustless.

SPREE would be helpful to ensure that the same logic is shared between allychains in the SPREE modules. An especially relevant use case would revolve around the use of token transfers across allychains in which it is important that the sending and receiving allychains agree about how to change the total supply of tokens and a basic interface.

## Example

![spree example](assets/SPREE/spree_module.png)

The diagram above is a simplification of the AXIA system.

In this diagram we see that the Wasm code for SPREE module "X" has been uploaded to the AXIA Relay Chain. The two cylinders "A" and "B" represent two distinct allychains that have both opted-in to this SPREE module creating two distinct instances of it with their own XCMP endpoints "A.X" and "B.X".

In the example we assume that this SPREE module "X" contains the functionality for incrementing of decrementing the balance of a particular asset that is unique to this module.

By initiating a transaction at A.X to decrease a particular balance by 1, a message over XCMP can be trustlessly sent to B.X to increase a balance by 1.

Collators, represented as the green triangle are responsible for relaying this message from allychain A to allychain B, as well as mantaining the storage for each particular instance of A.X and B.X for their respective allychains. They provide proofs of valid state transitions to the Relay Chain validators, represented as blue diamonds.

Validators can validate the correct state transitions of SPREE modules A.X and B.X by being provided with the previous state root of the SPREE module instances, the data of the XCMP message between the instances, and the next state root of the instance. They do this validation be checking it against the `validate` function as provided by the SPREE module API. Collators are expected to be able to provide this information in order to progress their allychains.
