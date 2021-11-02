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
## Scaling PoA

### Candidates

### Validators

### Random Number Generation

## Dual-token Design

### Bergs

### gBergs (gas bergs)
