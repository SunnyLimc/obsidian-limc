- **every trade-off is based on your security (threat) model**

- how much entropy is needed depends on your security model
	- entropy: $\log{_2}(possibility)$, with coin flip, the possibility is 2

### hash functions

- sha1 (bytes) to 160 bits

- **feature**
	- non-invertible - only input to output
	- collision resistance - hard to find two input cause same output
		- differentiate different identifier
	- fix size output - squeeze big input
- **usage**
	- commit schema
	- the precommitment ensure the origin data can not be tampered when reveal to it - **verification**
		- Anyone give you the file
		- If you are certainty what the hash **actually** is or **how** to construct the hash - just hash the file, and do compare the hash of the file
		- It named commit schema

- key derivation function (KDF)
	- PBKDF
	- **slow to calculate** - prevent attacker from brute-force original data
		- in this case, your data is small enough that everyone can hash it in instant
		- so people add a extra work to the cryptography make sure it's slow to calculate
		- to increase the calculation overhead of invert hacking


### symmetric key cryptography

- function
	- `keygen()` - generate key
	- `encrypt(plaintext, key)` - convert to ciphertext
	- `decrypt(cipertext, key)` - convert to plaintext
	- given ciphertext can not figure out plaintext without key
	- `decrypt(encrypt(m, k), k) = m`
- to avoid in case losing key, **key dividing** func can use the following
	- **passphrase** -> KDF -> key (with plaintext) -> encrypt -> ciphertext
	- a way is using `aes-256-cbc`
		- `openssl aes-256-cbc -salt -in file -out file.enc`
		- `openssl aes-256-cbc -d -in file -out file.enc`
- store highly random salt along side with password to avoid **rainbow table** attacking

### **A**symmetric key cryptography

- encrypt/decrypt
	- `keygen()` - generate **public** key and **private** key
	- `encrypt(ptext, public key)` 
	- `decrypt(ctext, private key)`
	- given a ciphertext, you can not figure out plaintext without **private key**
	- everyone can use **public key** to encrypt
- verification
	- `sign(message, private key)` produce signature
	- `verify(message, sign, public key)` return if OK?
	- given a message and sign with a public key, you can not reproduce the signed signature of any other message without the **private key**
	- everyone can use **public key** to verify
	- it basically seems apply **private key** to do decrypt for particular plaintext and **public key** to encrypt it and verify if the same #q
- verify public key
	- out-of-band communication (by offline exchange)
	- pinning public key (warn if it changed)
- GPG has a chain trusting model


### hybrid encryption

1. symmetric encryption is fast
2. asymmetric encryption is slow
- we can use the 1. generate **key** to encrypt files, and encrypt the **key** with 2. **public key**, and retriever and retrieve the **key** with **private key**, then decrypt the files with the retrieved **key**