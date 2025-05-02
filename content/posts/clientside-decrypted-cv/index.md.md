+++
title= "Sharing confidential documents in plain sight"
date= "2025-05-02"
tags=["post", "encryption", "cryptography", "serverless"]
draft=false
+++
## Context 

As a scrappy team working on OpenAttestation (back in 2018) with minimal funding, we often had to come up with creative solutions to minimise the amount of infrastructure that we had to deploy, while maintaining confidentiality and integrity of content. 

One of the tools in our toolkit we reached for frequently was applied cryptography. It allowed us to come up with non-conventional architectures that could be statically hosted without a login wall, user database or API calls. 

This past weekend it occurred to me that I might want to share my CV with certain recipients, with just a single URL that can be kept updated and also printed or rendered in a QR code.

Another desired property I would like to have is to deny free access to web crawlers and random passerbys, since there is a fair bit of personal information in the CV.

One of the ways to do that would be to encrypt the CV file and host it publicly, and share the decryption key with the individual.

This was indeed a method we used in OpenAttestation, with encryption and storage performed via [opencerts-functions](https://github.com/OpenCerts/opencerts-functions#storage) and decrypted using [oa-encryption](https://github.com/Open-Attestation/oa-encryption).

I decided to experiment with a modified version of this for hosting my CV publicly, with the ability to track which key has been used, and without the need for a server API call.

## System Design Overview

The system employs a symmetric encryption scheme (AES-GCM) to secure the payload using a randomly generated secret key (**KeyA**).

The resulting ciphertext (**EncryptedKeyA**) is publicly hosted. Due to its encryption, neither hosting providers nor transport intermediaries can access the contents of **KeyA**.  

Access control is enforced by encrypting **KeyA** with a user-specific key (**User1SecretKey**). Each user’s version of the encrypted key (**User1EncryptedKeyA**) is also publicly hosted but remains accessible only to its intended recipient.

This architecture enables key rotation for the primary encryption key (**KeyA**) and allows for selective revocation of user access just by deleting **EncryptedKeyA**. 

It must be noted, however, that revocation cannot retroactively prevent exfiltration of content that has already been decrypted and saved by a user.

Recipients are provided with a URL structured as follows:

```
https://cv.rjchow.com/cv.html?#key=<User1SecretKey>
```

Upon accessing the URL, the client-side application fetches the user-specific encrypted key (**User1EncryptedKeyA**) and decrypts it locally using JavaScript and the supplied **User1SecretKey**.

Once **KeyA** is recovered, the client proceeds to decrypt the main payload to retrieve the plaintext content (e.g., the CV).

---

## OMG did you just roll your own crypto?

No I didn't commit the cardinal sin of rolling my own crypto, ~ChatGPT did~. 

Kidding aside, this is not rolling my own crypto as I've not invented any of these cryptographic primitives. 

The methodology is described in [RFC 3394](https://datatracker.ietf.org/doc/html/rfc3394), and I performed the encryption/decryption using [WebCrypto's SubtleCrypto methods](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto) which are provided by the browser. 

## Implementation Details and Security Considerations

I'll write this up in a bit :-)

## Play with it here

[Demo](/cv-encryption-demo/index.html#{"userSecretKey":"a28e9b1d97a8d15479d774354f75f3488f957571cdcf6ca793ffc2200169cdb2"})

Open the DevTools and observe that is fetched, and you'll see that if you try to access the URL without the decryption key (or modify it), it fails to decrypt. 

Also notice that both cv.enc and encryptedKeyA.b64 are encrypted, and are decrypt-able using the provided decryption key.