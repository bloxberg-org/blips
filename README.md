# bloxberg Improvement Proposals

| ID                         | Title                  | Type   | Status |
| ------------------------------ | ---------------------- | ------ | ------ |
| [BIP-0001](bips/bip-0001-researchcertificate.md)        | Research Data Certificates | Stream | ![Draft](http://rfc.unprotocols.org/spec:2/COSS/stable.svg) |


## Working on BIPs

All **BIPs** are written in [markdown](https://en.wikipedia.org/wiki/Markdown)
and managed by the community via github version control in the 
[BIPS repository](https://github.com/bloxberg-org/bips). The markdown content is than 
built and published by the maintainers with [mkdocs](http://www.mkdocs.org/).

If you have some basic command line skills you can build and run the 
[BIPs website](https://bips.bloxberg.org) on your own computer. Make sure 
you have the [Git](https://git-scm.com/) and [Python](https://www.python.org/) 
installed on your system and  follow these steps on the command line:

```bash
git clone https://github.com/bloxberg-org/bips.git
cd bips
pip install -r requirements.txt
mkdocs serve
```

## Usage

All BIP documents can be found in the `bips` subfolder of this repository. The 
recommended editor for the markdown files is [typora](https://typora.io/). If 
you have commit rights to the [main repository](https://github.com/bloxberg-org/bips) 
you can deploy the site with a simple `mkdocs gh-deploy`.
