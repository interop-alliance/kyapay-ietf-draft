---
title: "KYAPay Profile"
#abbrev: ""
category: info

docname: draft-dz-kyapayprofile
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
#number: 
date:
consensus: true
v: 3
area: AREA
workgroup: TODO
keyword:
 - agent
venue:
  github: "interop-alliance/kyapay-ietf-draft"
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
#  github: USER/REPO
#  latest: https://example.com/LATEST

author:
 -
    fullname: Dmitri Zagidulin
#    organization: Your Organization Here
#    email: your.email@example.com

normative:

informative:

...

--- abstract

This document defines a profile for agent identity and payment tokens in
JSON web token (JWT) format. Authorization servers and resource servers from
different vendors can leverage this profile to consume identity and payment
tokens in an interoperable manner.

--- middle

# Introduction

As agents evolve from pre-orchestrated workflow automations to truly autonomous
or semi-autonomous assistants, they need the ability to identify themselves and
more importantly identify their human principals.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# KYAPay Token Schemas

## KYA Token

The following informative example displays a decoded KYA type token.

~~~
{
  "kid": "<JWK Key ID>",
  "alg": "ES256",
  "typ": "kya+JWT"
}.{
  "iss": "<URL of the token issuer>",
  "iat": 1742245254,  // 'issued at' timestamp
  "exp": 1773867654,  // 'expiration' timestamp
  "jti": "b9821893-7699-4d24-af06-803a6a16476b", // Unique token ID
  "sub": "<Buyer Agent Account ID>",
  "aud": "<Seller Agent Account ID>",

  // Claims common to both 'pay' and 'kya' types
  "env": "<Issuer environment (sandbox, production, etc)>",
  "ver": "1", // Version of the token
  "ssi": "<Seller Service ID>", // Seller service that this token was created for
  "btg": "<Buyer Tag (Buyer's internal reference ID)>",

  // Skyfire-defined claims for 'kya' (Know Your Agent) type tokens
  "bid": {  // Buyer Identity claims
    "email": "buyer@buyer.com”, 
    ...
  },
  "apd": { // Agent Platform Identity claims
  },
  "aid": { // Agent Identity claims
  }
}
~~~
{: #example-decoded-kya-token align="left" title="A KYA type token"}

## PAY Token

The following informative example displays a decoded PAY type token.

~~~
{
  "kid": "<JWK Key ID>",
  "alg": "ES256",
  "typ": "pay+JWT"
}.{
  "iss": "<URL of the token issuer>",
  "iat": 1742245254,  // 'issued at' timestamp
  "exp": 1773867654,  // 'expiration' timestamp
  "jti": "b9821893-7699-4d24-af06-803a6a16476b", // Unique token ID
  "sub": "<Buyer Agent Account ID>",
  "aud": "<Seller Agent Account ID>",

  // Claims common to both 'pay' and 'kya' types
  "env": "<Issuer environment (sandbox, production, etc)>",
  "ver": "1", // Version of the token
  "ssi": "<Seller Service ID>", // Seller service that this token was created for
  "btg": "<Buyer Tag (Buyer's internal reference ID)>",

  // Claims for 'pay' (Agent Payment) type tokens
  "spr": "0.010000", // Seller Service Price
  "sps": "PAY_PER_USE", // Seller Service Pricing Model
  "amount": "15.000000", // Token Amount in Currency units
  "cur": "USD", // Currency unit
  "value": "15000000", // Token Amount in Settlement Network
  "mnr": "1500", // Maximum number of requests when sps == "PAY_PER_USE"
  "stp": "<COIN | CARD | BANK>", // Settlement type
  "sti": { // Meta information for payment settlement depending on the settlement type
    "type": "<'type' is dependant on 'sti'>", // for COIN - USDC | x402; for CARD - VISA_VIC;
    // For type == USDC - No more sub-claims
    // For type == VISA_VIC
    "dynamicDataExpiration": "<Timestamp when DTVV expires>",
    "dynamicDataId": "<Visa specific identifier>",
    "dynamicDataType": "DAVV",
    "dynamicDataValue": "<DAVV value>",
    "paymentToken": "<16 Digit Virtual Payment Card Number>",
    "tokenExpirationMonth": "<Expiration Month Number>",
    "tokenExpirationYear": "<Expiration Year>"
  }
}

~~~
{: #example-decoded-pay-token align="left" title="A PAY type token"}

## KYA+PAY Token

The following informative example displays a decoded KYA+PAY type token.

~~~
{
  "kid": "<JWK Key ID>",
  "alg": "ES256",
  "typ": "kya+pay+JWT"
}.{
  "iss": "<URL of the token issuer>",
  "iat": 1742245254,  // 'issued at' timestamp
  "exp": 1773867654,  // 'expiration' timestamp
  "jti": "b9821893-7699-4d24-af06-803a6a16476b", // Unique token ID
  "sub": "<Buyer Agent Account ID>",
  "aud": "<Seller Agent Account ID>",

  // Claims common to both 'pay' and 'kya' types
  "env": "<Issuer environment (sandbox, production, etc)>",
  "ver": "1", // Version of the token
  "ssi": "<Seller Service ID>", // Seller service that this token was created for
  "btg": "<Buyer Tag (Buyer's internal reference ID)>",

  // Claims for 'kya' (Know Your Agent) type tokens
  "bid": {  // Buyer Identity claims
    "email": "buyer@buyer.com”, 
    ...
  },
  "apd": { // Agent Platform Identity claims
  },
  "aid": { // Agent Identity claims
  },

  // Claims for 'pay' (Agent Payment) type tokens
  "spr": "0.010000", // Seller Service Price
  "sps": "PAY_PER_USE", // Seller Service Pricing Model
  "amount": "15.000000", // Token Amount in Currency units
  "cur": "USD", // Currency unit
  "value": "15000000", // Token Amount in Settlement Network
  "mnr": "1500", // Maximum number of requests when sps == "PAY_PER_USE"
  "stp": "<COIN | CARD | BANK>", // Settlement type
  "sti": { // Meta information for payment settlement depending on the settlement type
    "type": "<'type' is dependant on 'sti'>", // for COIN - USDC | x402; for CARD - VISA_VIC;
    // For type == USDC - No more sub-claims
    // For type == VISA_VIC
    "dynamicDataExpiration": "<Timestamp when DTVV expires>",
    "dynamicDataId": "<Visa specific identifier>",
    "dynamicDataType": "DAVV",
    "dynamicDataValue": "<DAVV value>",
    "paymentToken": "<16 Digit Virtual Payment Card Number>",
    "tokenExpirationMonth": "<Expiration Month Number>",
    "tokenExpirationYear": "<Expiration Year>"
  }
}

~~~
{: #example-decoded-kya-pay-token align="left" title="A KYA+PAY type token"}

# Security Considerations

TODO Security

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
