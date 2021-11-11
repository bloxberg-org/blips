This BLIP is a draft: the collaborative document we use to write it is https://hackmd.io/QtoxlUimQNSJByIRKm0IwQ?edit

# Peer Review

| BLIP:     | 4                                                        |
| -------- | ------------------------------------------------------------ |
| Title:   | bloxberg Consensus Upgrade                                      |
| Owner(s):  |                                                            |
| Author(s):  | James Lawton (lawton@mpdl.mpg.de)                       |
| Status:  | ![Raw](http://rfc.unprotocols.org/spec:2/COSS/raw.svg) |
| Created: | 2020-06-09                                                   |
| License: | BSD-2-Clause                                                 |

## Motivation

On the genesis of bloxberg, we utilised the Proof of Authority AuRa (citation needed) algorithm for supporting research organisations as central to the governance, security, and infrastructure of bloxberg. With an upgraded consensus algorithm, we aim to ensure the continued emphasis of scientific organisations in the governance and security of bloxberg. We want to retain the core focus of bloxberg of a blockchain network that is run by scientists for science. This means ensuring that the infrastructure is always governed by research institutes and organisations at its core. However, this paradigm can limit the scalability in terms of onboarding and authority selection in classical PoA consensus algorithms such as AuRa (need citation) or GETH version (citation needed). By introducing a distinction between total candidates eligible for block proposal and a proportion of those candidates that validate blocks during an epoch, we can achieve much higher scalability than traditional PoA. For our implementation, we utilise the architecture created on the xDAI chain for POSDAO consensus (citation needed)

Furthermore, we introduce a dual-token design to bloxberg to support future incentives for actively participating in bloxberg. These take the form of BERGs as a form of governance or utility token for administering the chain which are emitted on block validation to eligible validators. Secondly, gBergs are emitted in a fixed interval per BERG held in a wallet.
## Scaling PoA
bloxberg currently utilizes the AuRa algorithm for consensus coupled with a RelaySetContract that specifies a ValidatorSet. The validator set consists of all bloxberg members that are voted in and are running a node on-chain with a specified address. The ValidatorSet contains a list of bloxberg blockchain addresses n, at each height h that participate in consensus. We don't define a max n value in order to enable the consortium to consistently grow, however this quickly becomes intractable organizationally and technically as bloxberg grows. To this end, we introduce a distinction between the total number of bloxberg members eligible to be selected for partaking in consensus during a given epoch and the current list of validators selected to validate blocks within the current epoch. This distinction ensures that a reasonable but sufficiently decentralized number of validators are securing the chain at any given epoch while ensuring that the consortium can continue to grow.
### Candidates
We create a candidate pool via a governance smart contract that consists of every bloxberg member voted in by the consortium according to the bloxberg governance rules. The candidate pool can consist of N number of nodes with no max threshold.
### Validators
In order to create a validator set, we 
randomly sample using on-chain random number generation 
a subset of validators from the current bloxberg candidate pool for each epoch. 
The selected validator set confirms blocks in round robin style according to the currently used AuRa consensus algorithm for the duration of the epoch. 
Each validator during receives a static block reward (or fixed percentage based on inflation rate based on total supply?) for each block validated.

Proposed constants:

block reward - 8 bergs per block, have to model how much gBergs this generates per block and inflation schedule
Epoch, e - 1 week
Number of Validators - 23
Size of candidate pool - Unlimited

### Random Number Generation
Generating pseudorandom numbers in a blockchain environment is quite difficult. For this, we utilize the methodology outlined from the RandomAura RNG (citation needed) smart contract (https://github.com/poanetwork/posdao-contracts/blob/master/contracts/RandomAuRa.sol) of a sequence of commit and reveal phases 

## Dual-token Design
Bergs are governance tokens that used to secure bloxberg as well as vote on various governance proposals to maintain the bloxberg infrastructure. 

gBergs (suggestions for better name welcome) are elicited at a constant speed based on the amount of bergs held which are used for transaction costs on the bloxberg blockchain. The aim is to ensure that security is retained on bloxberg while still ensuring the cost of utilizing bloxberg is kept to a minimum, especially for scientific and research focused applications. Thus, by decoupling the cost of computation on bloxberg from regular Bergs, we will ensure that the use cases will be feasible well into the future.

### Bergs

TBD: Economic modeling of bergs based on desired inflation rate, bergs on genesis, amount of gBergs desired to be elicited.
TBD: Define initial supply, suggested 1 billion Bergs with a fixed rate of inflation for block rewards.
### gBergs (gas bergs)

For this purpose, we can define the generation rate of gBergs. Let B be the amount of bergs in the system, t is time in terms of blocks validated, and b is generation speed. 
Generation rate = b * B * t

We can set b to a constant 5x10^-8 of gBergs per block which means if you had 10k Bergs, it would generate approximately 4.32 gBergs per 24 hours (https://docs.vechain.org/thor/learn/two-token-design.html).
