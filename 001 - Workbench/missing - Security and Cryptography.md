- **every trade-off is based on your security model**

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
		- in this case, your data is small enough that everyone can hast it in instant
		- so people add a extra work to make it slow to calculate