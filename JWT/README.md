

## What is a JWT?
JSON web tokens are a standardized format for sending cryptographically signed JSON data between systems. Unlike classic session tokens, all of the data that a server needs is stored client-side within the JWT itself.

JWT can either use a **JSON web signature (JWS)** or **JSON Web Encryption (JWE)** for protection of data. The format of a JWT consists of a Header, Payload, and Signature separated by dots (.).  

JWT is a stateless session which means that the server does not retain any session state between client requests. Each requests from the client must contain all necessary information, for the server to process it independently, meaning it does not rely on prior interactions.


## Basic Checklists
- [ ] Identified the web application is using stateless JWT for authentication
- [ ] Decode the JWT and remove the signature 
- [ ] Attack the JWT via a `None` signing attack
- [ ] Attack the signing algorithm if the JWT is using `HS256`
	- [ ] Forge the JWT and modify it's contents
```bash
# Brute Force with hashcat module 16500
hashcat -m 16500 jwt.txt /opt/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
- [ ] Conduct `Algorithm Confusion` via `rsa_sign2n` (no exposed public key)
> <details>
> <summary>rsa_sign2n and jwt_forgery</summary>
>  
>This is a JWT attack that forces the web application to use a different algorithm to verify the JWT's signature than the one used to create it.
>
>If the web app uses asymmetric algorithm `RS256`, a private key is used to compute the signature, and the public key is used to verify the signature. If we create a token with the symmetric algorithm `HS256`, the token's signature can be verified using the same public key.
>
>This attack arises when the server accepts any algorithm listed in the JWT header. When this happens and we can change it to HMAC, one shared secret is used for both signing/verifying.
>1. Run the `rsa_sign2n` docker container
>```bash
>cd rsa_sign2n/standalone/
>docker build . -t sig2n
>docker run -it sig2n /bin/bash
>```
>2. Obtain the public key
>```bash
> #Requires two JWTs
>python3 jwt_forgery.py <JWT> <JWT>
>```
>3. Send each token to the web application to confirm algorithm confusion
>4. Use Cyberchef `JWT Sign` feature to forge a JWT with a custom payload using the outputted public key
>	1. Sign with the public key `.pem`
>	2. Add a newline (`\n)` at the end of the public key.
> </details>


- [ ] Conduct `Algorithm Confusion` via Burp Suite (exposed public key)



## Tools
* [jwt.io](https://jwt.io/) - Encoder/Decoder
* [ticarpi/jwt_tool](https://github.com/ticarpi/jwt_tool) - Toolkit for validating, forging, scanning and tampering JWTs
* [silentsignal/rsa_sign2n](https://github.com/silentsignal/rsa_sign2n) - Tool used to compute the public key to sign the JWTs

