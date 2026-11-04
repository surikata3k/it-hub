# Asymmetric encryption: how it works

## Introduction and objectives

Asymmetric encryption uses two related keys: whatever one of them encrypts, only the other can decrypt. This idea solves the big problem of symmetric encryption (how to share the key) and makes HTTPS, SSH, digital signatures and certificates possible.

By the end of this material, students should be able to:

1. Explain the difference between symmetric and asymmetric encryption and when each one is used.
2. Describe the role of the public key and the private key in confidentiality and in signatures.
3. Work out an RSA example by hand using small numbers.
4. Explain why hybrid encryption is used in practice and what a PKI provides.
5. Generate keys, encrypt, sign and verify with OpenSSL and GPG.

## The problem: distributing the key

In symmetric encryption (AES, ChaCha20) the same key is used to encrypt and to decrypt. It is very fast, but the sender and the receiver must share that key before they communicate, and doing so over an insecure channel is exactly what we want to avoid.

On top of that, the number of keys grows very quickly: n people who want to talk privately in pairs need n(n−1)/2 keys. A class of 30 students would need 435 different keys.

|  | Symmetric | Asymmetric |
| --- | --- | --- |
| Keys | One shared key | One pair per person (public + private) |
| Keys for n users | n(n−1)/2 | 2n |
| Speed | Very fast | Hundreds or thousands of times slower |
| Typical use | Encrypting large volumes of data | Exchanging keys, signing, authenticating |
| Examples | AES, ChaCha20, 3DES | RSA, ECC (ECDSA, Ed25519), Diffie-Hellman |

The practical solution combines both: asymmetric to agree on a key and symmetric to encrypt the data (see the hybrid encryption section).

## The key pair

Each user generates a pair of mathematically linked keys: a **public key**, which can be shared with everyone, and a **private key**, which never leaves their machine. Whatever is encrypted with one key can only be decrypted with the other key of the same pair.

- **Public key**: like an open padlock you hand out to everyone. Anyone can snap it shut, but only you have the key that opens it.
- **Private key**: the key to the padlock. If it is lost or leaked, all the security collapses.

### Why can't the private key be calculated from the public key?

It relies on **trapdoor one-way functions**: operations that are easy in one direction and practically impossible to reverse without a piece of secret information.

- **RSA**: multiplying two large primes is instant; factoring the product (a 617-digit number for RSA-2048) is not feasible with today's computers.
- **Diffie-Hellman and ECC**: computing a power (or a multiple of a point on a curve) is easy; finding the exponent from the result (the *discrete logarithm*) is very hard.

Security is computational, not absolute: it depends on the key size and on the available computing power. That is why recommended key lengths increase over time, and why quantum computers are a long-term threat (post-quantum cryptography).

## Confidentiality: encrypting for someone

To send a message that only Bob can read, Alice encrypts it with **Bob's public key**; only **Bob's private key** can decrypt it.

1. Bob generates his key pair and publishes the public key.
2. Alice obtains Bob's public key.
3. Alice encrypts the message with that public key and sends the ciphertext.
4. An attacker who intercepts the message can't do anything with it: they don't have the private key.
5. Bob decrypts it with his private key.

Rule to remember: **for confidentiality, encrypt with the recipient's public key.** Note that this does not prove who sent the message: anyone can use Bob's public key.

## Digital signature: proving who sent it

A signature uses the keys the other way round: Alice signs with her **private key** and anyone can verify it with her **public key**. If verification succeeds, it can only have been Alice.

The whole document is not signed, only its **digest (hash)**, which is short and has a fixed size:

1. Alice computes the hash of the document (for example, SHA-256).
2. Alice encrypts that hash with her private key: this is the signature.
3. She sends the plaintext document + the signature.
4. Bob computes the hash of the received document himself.
5. Bob decrypts the signature with Alice's public key and obtains the original hash.
6. If the two hashes match, the signature is valid.

| Property | What it guarantees | Why |
| --- | --- | --- |
| Authenticity | The message comes from Alice | Only she has the private key |
| Integrity | The message has not been modified | Changing a single bit gives a different hash |
| Non-repudiation | Alice cannot deny having signed it | The signature cannot be generated without her private key |

A signature does **not** provide confidentiality: the document travels in plaintext. If both are needed, the message is signed with the sender's private key and encrypted with the recipient's public key.

&#91;embedded content: encryption vs signature · which key is used at each step\]

In encryption, the private key acts at the end (the receiver); in a signature, at the beginning (the sender).

## Main algorithms

### RSA (1977)

RSA is the classic asymmetric algorithm: it can be used both to encrypt and to sign, and its security depends on how hard it is to factor large numbers.

**Key generation**

1. Choose two prime numbers p and q.
2. Compute n = p × q (the modulus, which is public).
3. Compute φ(n) = (p − 1)(q − 1).
4. Choose e, coprime with φ(n) (usually 65537).
5. Compute d, the inverse of e modulo φ(n): e × d ≡ 1 (mod φ(n)).

Public key = (e, n). Private key = (d, n).

```latex
C = M^{e} \bmod n \qquad\qquad M = C^{d} \bmod n
```

**Example with small numbers** (checked with Python)

| Step | Calculation | Result |
| --- | --- | --- |
| Primes | p = 5, q = 11 |  |
| Modulus | n = 5 × 11 | 55 |
| φ(n) | (5 − 1)(11 − 1) | 40 |
| Public exponent | e = 3 (gcd(3, 40) = 1) | 3 |
| Private exponent | 3 × d ≡ 1 (mod 40) → 3 × 27 = 81 = 2 × 40 + 1 | d = 27 |
| Encrypt M = 7 | 7³ mod 55 = 343 mod 55 | C = 13 |
| Decrypt | 13²⁷ mod 55 | M = 7 |
| Sign M = 7 (with d) | 7²⁷ mod 55 | S = 28 |
| Verify (with e) | 28³ mod 55 | 7 = M, valid |

You can check it with Python in class: `pow(7, 3, 55)` returns 13 and `pow(13, 27, 55)` returns 7. To compute d: `pow(3, -1, 40)` returns 27.

In real life, p and q are hundreds of digits long and RSA is not applied directly to the message, but with padding (OAEP for encryption, PSS for signatures).

### Diffie-Hellman (1976)

It neither encrypts nor signs: it lets two parties **agree on a shared secret key** over a public channel, without ever sending it.

|  | Alice | Public (everyone can see it) | Bob |
| --- | --- | --- | --- |
| Parameters |  | p = 23, g = 5 |  |
| Secret | a = 6 |  | b = 15 |
| Sends | A = 5⁶ mod 23 = 8 | A = 8, B = 19 | B = 5¹⁵ mod 23 = 19 |
| Computes | 19⁶ mod 23 = **2** |  | 8¹⁵ mod 23 = **2** |

Both obtain the key 2 without ever transmitting it. An attacker sees 23, 5, 8 and 19, but would have to solve the discrete logarithm to find a or b. Without authentication, DH is vulnerable to a man-in-the-middle (MITM) attack; that is why it is combined with certificates.

### Elliptic curve cryptography (ECC)

ECC offers the same security as RSA with much shorter keys, which makes it faster; it is what TLS 1.3 and SSH use by default today.

| Approximate security | RSA | ECC |
| --- | --- | --- |
| 128 bits | 3072 bits | 256 bits |
| 192 bits | 7680 bits | 384 bits |
| 256 bits | 15360 bits | 521 bits |

Common names: **ECDH / X25519** (key agreement), **ECDSA** and **Ed25519** (signatures).

## Hybrid encryption and PKI

### Hybrid encryption

Since asymmetric encryption is slow, in practice it is only used to protect a symmetric **session key**, and the data is encrypted with AES or ChaCha20. This way you get the best of both systems.

1. The sender generates a random session key (for example, AES-256).
2. They encrypt the data with that session key.
3. They encrypt the session key with the recipient's public key (or agree on it with Diffie-Hellman).
4. The recipient recovers the session key with their private key and decrypts the data.

This is what PGP/GPG, S/MIME, TLS and SSH do.

### The trust problem: whose public key is this?

If an attacker manages to make us believe that their public key is the bank's, they can read everything (MITM attack). We need a way to bind a public key to an identity.

- **Digital certificate (X.509)**: a document containing the public key, the holder's name (e.g. the domain), the validity dates and the **signature of a certificate authority**.
- **Certificate authority (CA)**: a trusted entity that verifies identities and signs certificates (e.g. Let's Encrypt, or in Catalonia the Consorci AOC for idCAT).
- **Chain of trust**: root CA (preinstalled in the system or browser) → intermediate CA → server certificate. The client verifies each signature until it reaches a root it trusts.
- **Revocation**: a compromised certificate can be invalidated before it expires (CRL, OCSP).
- **Alternative without a CA**: PGP's *web of trust*, where users sign each other's keys; or the *fingerprint* SSH asks you to accept the first time you connect.

### Putting it all together: an HTTPS connection (TLS 1.3, simplified)

1. The client and the server perform an ephemeral Diffie-Hellman exchange (ECDHE) to agree on a key.
2. The server sends its certificate and signs the exchange with its private key.
3. The client verifies the certificate against the CA chain and checks the signature: the server is who it claims to be.
4. From the agreed key, both derive symmetric keys, and the data travels encrypted with AES-GCM or ChaCha20-Poly1305.

Asymmetric to authenticate and agree on keys, symmetric for the data: hybrid encryption in action.