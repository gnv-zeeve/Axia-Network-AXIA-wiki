---
id: learn-cryptography
title: Security and Cryptography
sidebar_label: Security and Cryptography
description: An overview of the cryptographic functions used in AXIA
slug: ../learn-cryptography
---

## Security
Some of the intricacy of the AXIA Network  is fulfilling the goal of ensuring that security is fault tolerant . With the properties of PoP the stakeholders elect a set of validators that has  more than ⅔ of honest members. This should far exceed that figure as this is a bare minimum threshold. The elected validators are responsible for assistance with maintaining  the CoreChain. To achieve good scalability the number of validators in each of these AllyChains is small. Nonetheless, thanks to the PoP guarantee that every validator is well backed, the availability and validity scheme can ensure that any attack on the legitimacy of AXIA is extremely expensive. In fact, the entirety of AXIA’s economic security backs every AllyChain.
This is in stark contrast to having, say, 100 independent blockchains with an equivalent sum total of stake, where on average each blockchain is backed by 1/100th of the stake, and thus only benefits from 1/100th the level of security. AXIA guarantees availability by using erasure coding or a forward error correction of each AllyChain block to make the validators collectively responsible for the availability of these blocks without breaking scalability. For this to work, there is a requirement to revert the chain until it is known with high probability that all activity on AllyChains are accurate and authentic. Therefore for a chain to be capable of forking there could be the need to reorganize. Thus the AXIA Network utilizes one of the latest widely used and proven offers a block production mechanismsmechanism known as BABE (Blind Assignment of Blockchain Extension). More specifically, the longest chain rule is part of the consensus, so the next block producer is not known in advance.
On its own BABE could cause a long wait time from the moment a block is produced to the moment it is finalised, i.e. a high probability the block will never be reverted. Slow finality occurs on other blockchains in many circumstances when dealing with challenges to availability. Even though users would prefer blocks to be created and finalised much faster. The AXIA Network avoids this latency hurdle due to congestion as validators will be provided with the means to finalize blocks using GRANDPA (GHOST-based Recursive Ancestor Deriving Prefix Agreement). The GRANDPA is very adaptive as it separates block production which ensures that all challenges are dealt with without slowing block production. GRANDPA obtains systematic agreement on the finalized blocks allowing for proof to an entity that keeps track of the validator set.
It is important f that all validators have large amounts of stake. The favourable and adaptive reward schedule for validators and the nominators ensures that overall participation in the PoP remains high and only continues to grow.  Furthermore, there is reassurance that the stake of the validators are evenly distributed.
Validators are rewarded or slashed on a per executed action basis. They will extend the same reward or punishment onto the nominator proportionally. This makes certain that the most rational strategy is compatible with honest and ethical behaviour. . 
Updates may be needed for the logic of the CoreChain. The governance model allows AXIA Coin holders to participate in the decision-making process rather than having any changes to the system be imposed by a central authority – or in the case of some decentralized systems, by a team of developers. As seen too often with existing blockchains a contentious code change leads to an impasse and/or a permanent fork The AXIA Network balances being able to make consensus driven changes in a form of general agreement with significant speed while also being able to deal with less prefered options in a decisive and fair manner. The ultimate arbiters of AXIA are the AXIA Coin holders and so all important decisions, such as code changes, are made through  state-weighted election. Therefore there is no manner that changes can be made when not elected by the majority. 
## Cryptography
AXIA and AXIA AllyChains use cryptographic keys and networking schemes.
AXIA will extend the peer-to-peer network that is standard in single chain permissionless blockchains to a next generation multi-chain system, where any network traffic should not scale with the total data of the system. AXIA distinguishes among different permissions and functionalities with different keys and key types, respectively. These are categorized into account keys with which users interact as well as session keys that nodes manage without operator intervention beyond a certification process.