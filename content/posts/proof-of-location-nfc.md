+++
title= "Proof of location using NFC"
date= "2025-04-24"
tags=["post"]
draft= true
+++


- talk about using signed NFC payload to prove location
- the NFC emulator must
	- have a time-varying payload
	- payload must be signed with a trusted key
	- private key of course must be safe but must be present on the nfc emulator
	- public key has to be somehow trusted on the other end
	- other end can verify signature and time validity
	- if the interval between payload changes is small enough, it can even distinguish individual users
	- payload would be a url with queryparams