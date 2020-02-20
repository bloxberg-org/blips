# bloxberg Improvement Proposals

| ID                         | Title                  | Type   | Status |
| ------------------------------ | ---------------------- | ------ | ------ |
| [BLIP-0001](blips/blip-0001-researchcertificate.md)        | Research Object Certification | Stream | ![Draft](http://rfc.unprotocols.org/spec:2/COSS/draft.svg) |

## Why BLIPs?

In order to standardize and collaborate on various common features in a scientific blockchain, bloxberg improvement proposals are issued. This is to ensure a high degree of compatibility and quality of the use cases developed for bloxberg.

These can include proposals such as modifications to the governance model, adjustments to the consensus algorithm, or standardization of specific contracts.

## Working on BLIPs

All **BLIPs** are written in [markdown](https://en.wikipedia.org/wiki/Markdown)
and managed by the community via github version control in the 
[BLIPS repository](https://github.com/bloxberg-org/blips). The markdown content is than 
built and published by the maintainers with [mkdocs](http://www.mkdocs.org/).

In order to submit a BIP, simply clone the repository and create a PR with a draft of the proposal which will then be assigned a number.

If you have some basic command line skills you can build and run the 
[BLIPs website](https://blips.bloxberg.org) on your own computer. Make sure 
you have the [Git](https://git-scm.com/) and [Python](https://www.python.org/) 
installed on your system and  follow these steps on the command line:

```bash
git clone https://github.com/bloxberg-org/blips.git
cd blips
pip install -r requirements.txt
mkdocs serve
```

## Usage

All BLIP documents can be found in the `blips` subfolder of this repository. The 
recommended editor for the markdown files is [typora](https://typora.io/). If 
you have commit rights to the [main repository](https://github.com/bloxberg-org/blips) 
you can deploy the site with a simple `mkdocs gh-deploy`.
