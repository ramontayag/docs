---
title: Stellar.toml
---

# Introduction

The `stellar.toml` file is used to provide a common place where the Internet can find information about your domain's Stellar integration. Any website can publish Stellar network information. You can announce your validation key, your [federation](./federation.md) server, peers you are running, your quorum set, if you are a anchor, etc.

The stellar.toml file is a text file in the [TOML format](https://github.com/toml-lang/toml).

## Publishing stellar.toml

Given the domain "DOMAIN", the stellar.toml will be searched for at the following location:

`https://www.DOMAIN/.well-known/stellar.toml`

## Enabling cross-origin resource sharing (CORS)
You must enable CORS on the stellar.toml so people can access this file from other sites. The following HTTP header *must* be set for a HTTP response for `stellar.toml` file request.

```
Access-Control-Allow-Origin: *
```

**Important**: Only enable CORS for stellar.toml (or any files it references). For example, in Apache you would set something like:

```xml
<Location "/stellar.toml">
    Header set Access-Control-Allow-Origin "*"
</Location>
```

Or in nginx:

```
location /stellar.toml {
 add_header 'Access-Control-Allow-Origin' '*';
}
```

For other webservers, see: http://enable-cors.org/server.html

## Testing CORS

1. Run a curl command in your terminal similar to the following (replace www.stellar.org with the hosting location of your stellar.toml file):

  ```bash
  curl --head https://www.stellar.org/.well-known/stellar.toml
  ```

2. Verify the `Access-Control-Allow-Origin` header is present as shown below.

  ```bash
  curl --head https://www.stellar.org/.well-known/stellar.toml
  HTTP/1.1 200 OK
  Accept-Ranges: bytes
  Access-Control-Allow-Origin: *
  Content-length: 482
  ...
  ```

3. Also run the command on a page that should not have it and verify the `Access-Control-Allow-Origin` header is missing.

## Stellar.toml example

This file is UTF-8 with Dos-, UNIX-, or Mac-style end of lines.
Blank lines and lines beginning with '#' are ignored.
Undefined sections are reserved.
All sections are optional.
Many of these sections reflect what would be listed in your [stellar-core.cfg](https://github.com/stellar/stellar-core/blob/master/docs/stellar-core_example.cfg)

```toml
# Sample stellar.toml

#   The endpoint which clients should query to resolve stellar addresses
#   for users on your domain.
FEDERATION_SERVER="https://api.stellar.org/federation"

# The endpoint used for the compliance protocol
AUTH_SERVER="https://api.stellar.org/auth"

#   This section allows an anchor to declare currencies it currently issues.
#   Can be used by wallets and clients to trust anchors by domian name 
[[CURRENCIES]]
code="USD"
issuer="GCZJM35NKGVK47BB4SPBDV25477PZYIYPVVG453LPYFNXLS3FGHDXOCM"
display_decimals=2 # Hint fo how many decimal places should be displayed by clients to end users.

[[CURRENCIES]]
code="BTC"
issuer="$anchor"
display_decimals=7 # Maximum decimal places that can be repesented is 7


#### Lesser used options below


# convenience mapping of common names to node IDs.
# You can use these common names in sections below instead of the less friendly nodeID.
# This is provided mainly to be compatible with the stellar-core.cfg
NODE_NAMES=[
"GD5DJQDDBKGAYNEAXU562HYGOOSYAEOO6AS53PZXBOZGCP5M2OPGMZV3  lab1",
"GB6REF5GOGGSEHZ3L2YK6K4T4KX3YDMWHDCPMV7MZJDLHBDNZXEPRBGM  donovan",
"GBGR22MRCIVW2UZHFXMY5UIBJGPYABPQXQ5GGMNCSUM2KHE3N6CNH6G5  nelisky1",
"GDXWQCSKVYAJSUGR2HBYVFVR7NA7YWYSYK3XYKKFO553OQGOHAUP2PX2  jianing",
"GAOO3LWBC4XF6VWRP5ESJ6IBHAISVJMSBTALHOQM2EZG7Q477UWA6L7U  anchor"
]

#   A list of accounts that are controlled by this domain.
ACCOUNTS=[
"$sdf_watcher1",
"GAENZLGHJGJRCMX5VCHOLHQXU3EMCU5XWDNU4BGGJFNLI2EL354IVBK7"
]

#   Any validation public keys that are declared
#   to be used by this domain for validating ledgers and are
#   authorized signers for the domain.
OUR_VALIDATORS=[
"$sdf_watcher2",
"GCGB2S2KGYARPVIA37HYZXVRM2YZUEXA6S33ZU5BUDC6THSB62LZSTYH"
]

# DESIRED_BASE_FEE (integer)
# This is what you would prefer the base fee to be. It is in stroops.
DESIRED_BASE_FEE=100

# DESIRED_MAX_TX_PER_LEDGER (integer)
# This is how many maximum transactions per ledger you would like to process.
DESIRED_MAX_TX_PER_LEDGER=400

#   List of IPs of known stellar-core's.
#   These are IP:port strings.
#   Port is optional.
#   By convention, IPs are listed from most to least trusted, if that information is known.
KNOWN_PEERS=[
"192.168.0.1",
"core-testnet1.stellar.org",
"core-testnet2.stellar.org:11290",
"2001:0db8:0100:f101:0210:a4ff:fee3:9566"
]

# list of history archives maintained by this domain
HISTORY=[
"http://history.stellar.org/prd/core-live/core_live_001/",
"http://history.stellar.org/prd/core-live/core_live_002/",
"http://history.stellar.org/prd/core-live/core_live_003/"
]

#   Potential quorum set of this domain's validatos.
[QUORUM_SET]
VALIDATORS=[
"$self", "$lab1", "$nelisky1","$jianing",
"$eno","$donovan"
]


```
