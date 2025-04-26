+++
title= "OpenCerts 2.0 Decentralised Issuer Identity Verification"
date= "2019-07-25"
tags=["post", "opencerts", "open-attestation", "crypto", "blockchain", "identity", "decentralisation"]
draft=false
+++

(This was originally posted on [Medium](https://medium.com/singapore-gds/opencerts-2-0-decentralised-issuer-identity-verification-fb7e2cae8295))

![](../1_o4wh__moYmbX-r3Fya3jYQ.webp)

## Introduction

In OpenCerts 2.0 we are further expanding the reach of the OpenCerts ecosystem by allowing the Domain Name System (DNS) to be used as an identity registry — in addition to our current SkillsFuture Singapore Accreditation Registry. A one-liner introduction to the DNS system can be summarised as: “Phonebook for the Internet”. It is indispensible and used by almost all services involving the Internet.

By allowing the DNS system to be used as an identity registry, we let domain name owners claim ownership of an OpenCerts Document Store smart contract on the Ethereum Blockchain.

## Rationale

OpenCerts 1.0 worked on a centralised registry model, governed by SkillsFuture Singapore and administered by GovTech. This created not just a single point of failure but also an unsustainable bottleneck when it came to approvals and identity verification.

OpenCerts 2.0 will continue to use this centralised registry model for the institutions that require a higher level of identity assurance. For issuers without this requirement they will be able to simply tie their issuance to their domain name, (e.g `example.openattestation.com`). When a user views a certificate issued under this model, they will see "Certificate issued by `example.openattestation.com`".

## How it works

Under [IETF RFC 1464](https://tools.ietf.org/html/rfc1464), it is possible to store arbitrary string attributes as part of a domain’s record set. This method is currently widely used for [email server authentication](https://en.wikipedia.org/wiki/Email_authentication) (SPF, DMARC, DKIM). Our DNS identity proof technique was largely inspired by [Keybase DNS proofs](https://github.com/keybase/keybase-issues/issues/367).

Only domain name owners (and the registrar that they trust) have the authority to make changes to the records associated with that domain name. Thus when a DNS record endorses a certain fact, it transitively asserts that this fact is believed to be true by the domain name owner.

In an OpenCerts 2.0 DNS-TXT identity proof, we record an OpenCerts Document Store address and the network (e.g Ethereum, Main Net) it is on. In the OpenCert document itself, we declare the domain name to search for the record as well as the Document Store Ethereum address. This forms a bi-directional trust assertion, and if the OpenCert’s cryptographic proof is issued on that Document Store — we can say that the domain name owner has endorsed the issuance of this OpenCert document.

![](../1_fYvXfU_eNjzBjaEFQFFrNA.webp)

A deeper technical discussion of this topic can be found at [OpenCerts 2.0 DNS-TXT Architecture Decision Record](https://github.com/OpenCerts/adr/blob/master/decentralized_identity_proof_DNS-TXT.md)

## Actions to Implement

## Create DNS TXT Record

The issuer will need to add a DNS TXT record to his domain name, the exact steps to achieve this can be confirmed with their domain name registrar.

The TXT record should look like

```
openatts net=ethereum netId=1 addr=0x9178F546D3FF57D7A6352bD61B80cCCD46199C2d
```

Optionally, the issuer may also publish an A record at the same address so that the if the certificate viewer clicks on the URL, they can see some helpful text regarding the issuer’s OpenCerts program.

## Certificate Schema Changes

In addition to Decentralised Rendering which will also require schema changes in OpenCerts 2.0, DNS verification adds a field `identityProof` to the issuer object. The issuer will need to already have deployed a Document Store contract as well as created the DNS TXT record as above.

The identityProof object will look like:

```
"issuers": [  
  {  
      "network": "ETHEREUM",  
      "documentStore": "0x9178F546D3FF57D7A6352bD61B80cCCD46199C2d",  
      "identityProof": {  
          "type": "DNS-TXT",  
          "location": "openattestation.com"  
      }  
  }  
]
```

## Closing

To sum up, OpenCerts 2.0 is going to be awesome for independent issuers! It allows them to now use OpenCerts completely permissionlessly.
