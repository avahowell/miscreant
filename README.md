# SIVChain

[![Build Status][build-image]][build-link]
[![MIT licensed][license-image]][license-link]

[build-image]: https://secure.travis-ci.org/zcred/sivchain.svg?branch=master
[build-link]: http://travis-ci.org/zcred/sivchain
[license-image]: https://img.shields.io/badge/license-MIT-blue.svg
[license-link]: https://github.com/zcred/sivchain/blob/master/LICENSE.txt

> The best crypto you've never heard of, brought to you by [Phil Rogaway]

Advanced symmetric encryption using the [AES-SIV] ([RFC 5297]) and [CHAIN] constructions,
providing easy-to-use (or rather, hard-to-misuse) encryption of individual
messages or message streams.

[Phil Rogaway]: https://en.wikipedia.org/wiki/Phillip_Rogaway
[RFC 5297]: https://tools.ietf.org/html/rfc5297
[CHAIN]: http://web.cs.ucdavis.edu/~rogaway/papers/oae.pdf


## What is SIVChain?

**SIVChain** is a set of interoperable libraries implemented in several
languages providing a high-level API for hard-to-misuse symmetric encryption.
Additionally, it provides streaming support, allowing large messages
to be incrementally encrypted/decrypted while still providing
[authenticated encryption].

The following constructions are provided by **SIVChain**:

* [AES-SIV]: (standardized in [RFC 5297]) combines the [AES-CTR]
  ([NIST SP 800-38A]) mode of encryption with the [AES-CMAC]
  ([NIST SP 800-38B]) function for integrity.
  Unlike most [authenticated encryption] algorithms, **AES-SIV** uses a
  MAC-then-encrypt construction, first using **AES-CMAC** to derive an
  IV from a MAC of zero or more "header" values and the message
  plaintext, then encrypting the message under that derived IV.
  This approach provides not just the benefits of an authenticated
  encryption mode, but also makes it resistant to accidental reuse
  of an IV/nonce, something that would be catastrophic with a mode
  like **AES-GCM**. **AES-SIV** provides [nonce reuse misuse resistance],
  considered the gold standard in cryptography today.

 * [CHAIN]: a construction which provides streaming [authenticated encryption]
   when used in conjunction with a cipher like **AES-SIV** that supports
   [nonce reuse misuse resistance]. Though not yet described in an RFC,
   **CHAIN** was designed by Phil Rogaway (who also created **AES-SIV**)
   and the paper contains a rigorous security analysis proving it secure.

[authenticated encryption]: https://en.wikipedia.org/wiki/Authenticated_encryption
[AES-SIV]: https://www.iacr.org/archive/eurocrypt2006/40040377/40040377.pdf
[AES-CTR]: https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Counter_.28CTR.29
[AES-CMAC]: https://en.wikipedia.org/wiki/One-key_MAC
[nonce reuse misuse resistance]: https://www.lvh.io/posts/nonce-misuse-resistance-101.html


## Comparison with other symmetric encryption algorithms

| Name              | [Authenticated Encryption] | [Misuse Resistance] | Passes | Standardization   |
|-------------------|----------------------------|---------------------|--------|-------------------|
| AES-SIV           | :green_heart:              | :green_heart:       | 2      | [RFC 5297]        |
| AES-GCM-SIV       | :green_heart:              | :yellow_heart:†     | 2      | Forthcoming‡      |
| AES-GCM           | :green_heart:              | :broken_heart:      | 2      | [NIST SP 800-38D] |
| AES-CCM           | :green_heart:              | :broken_heart:      | 2      | [NIST SP 800-38C] |
| AES-CBC           | :broken_heart:             | :broken_heart:      | 1      | [NIST SP 800-38A] |
| AES-CTR           | :broken_heart:             | :broken_heart:      | 1      | [NIST SP 800-38A] |
| ChaCha20+Poly1305 | :green_heart:              | :broken_heart:      | 2      | [RFC 7539]        |
| XSalsa20+Poly1305 | :green_heart:              | :broken_heart:      | 2      | None              |

† [According to the authors] of the AES-GCM-SIV specification, it does not have
  the actual properties of nonce reuse misuse resistance, although it does
  provide improvements over other AES modes of operation.

‡ Work is underway in the IRTF CFRG to provide an informational RFC for AES-GCM-SIV.
  For more information, see [draft-irtf-cfrg-gcmsiv][AES-GCM-SIV].

When standardization work around [AES-GCM-SIV] is complete, it will be seriously
considered for inclusion in this library. **AES-GCM-SIV** has the advantage of
the [GHASH] (technically **POLYVAL**) function being able to run in parallel,
versus **AES-CMAC**'s sequential operation.

**AES-SIV** has the advantages of stronger security guarantees, simplicity,
and that it can be implemented using the AES encryption function alone, making
it a better choice for environments where a hardware accelerated version of the
**GHASH** function is unavailable, such as low-powered mobile devices and
so-called "Internet of Things" embedded use cases.

[Misuse Resistance]: https://www.lvh.io/posts/nonce-misuse-resistance-101.html
[NIST SP 800-38A]: https://dx.doi.org/10.6028/NIST.SP.800-38A
[NIST SP 800-38B]: https://dx.doi.org/10.6028/NIST.SP.800-38B
[NIST SP 800-38C]: https://dx.doi.org/10.6028/NIST.SP.800-38C
[NIST SP 800-38D]: https://dx.doi.org/10.6028/NIST.SP.800-38D
[RFC 7539]: https://tools.ietf.org/html/rfc7539
[key recovery attacks]: https://mailarchive.ietf.org/arch/attach/cfrg/pdfL0pM_N.pdf
[According to the authors]: https://mailarchive.ietf.org/arch/msg/cfrg/CVfp8_sy2sNIj_LXCwQ1vbdhpqQ
[AES-GCM-SIV]: https://datatracker.ietf.org/doc/draft-irtf-cfrg-gcmsiv/
[GHASH]: https://en.wikipedia.org/wiki/Galois/Counter_Mode#Mathematical_basis


## Language Support

Packages implementing **SIVChain** are available for the following languages:

| Language               | Version                              |
|------------------------|--------------------------------------|
| [Go][go-link]          | N/A                                  |
| [JavaScript][npm-link] | [![npm][npm-shield]][npm-link]       |
| [Python][pypi-link]    | [![pypi][pypi-shield]][pypi-link]    |
| [Ruby][gem-link]       | [![gem][gem-shield]][gem-link]       |
| [Rust][crate-link]     | [![crate][crate-shield]][crate-link] |

[go-link]: https://github.com/zcred/sivchain/tree/master/go
[npm-shield]: https://img.shields.io/npm/v/sivchain.svg
[npm-link]: https://www.npmjs.com/package/sivchain
[pypi-shield]: https://img.shields.io/pypi/v/sivchain.svg
[pypi-link]: https://pypi.python.org/pypi/sivchain/
[gem-shield]: https://badge.fury.io/rb/sivchain.svg
[gem-link]: https://rubygems.org/gems/sivchain
[crate-shield]: https://img.shields.io/crates/v/sivchain.svg
[crate-link]: https://crates.io/crates/sivchain


## AES-SIV

This section provides a more in-depth exploration of how the **AES-SIV**
function operates.

### Encryption

<img src="https://camo.githubusercontent.com/3c23577a845b2ce86554dfc69b18cbbd691fd7cb/68747470733a2f2f7777772e7a637265642e6f72672f736976636861696e2f696d616765732f7369762d656e63727970742e737667" data-canonical-src="https://www.zcred.org/sivchain/images/siv-encrypt.svg" width="410px" height="300px">

#### Inputs:

* **AES-CMAC** and **AES-CTR** *keys*: *K<sub>1</sub>* and *K<sub>2</sub>*
* Zero or more message *headers*: *H<sub>1</sub>* through *H<sub>m</sub>*
* Plaintext *message*: *M*

#### Outputs:

* Initialization vector: *IV*
* *Ciphertext* message: *C*

#### Description:

**AES-SIV** first computes **AES-CMAC** on the message headers *H<sub>1</sub>*
through *H<sub>m</sub>* and messages under *K<sub>1</sub>*, computing a
*synthetic IV* (SIV). This IV is used to perform **AES-CTR** encryption under
*K<sub>2</sub>*

### Decryption

<img src="https://camo.githubusercontent.com/b2b2da0d26fccb4397e30b7555c7a0ace9df7737/68747470733a2f2f7777772e7a637265642e6f72672f736976636861696e2f696d616765732f7369762d646563727970742e737667" data-canonical-src="https://www.zcred.org/sivchain/images/siv-decrypt.svg" width="410px" height="368px">

#### Inputs:

* **AES-CMAC** and **AES-CTR** *keys*: *K<sub>1</sub>* and *K<sub>2</sub>*
* Zero or more message *headers*: *H<sub>1</sub>* through *H<sub>m</sub>*
* Initialization vector: *IV*
* *Ciphertext* message: *C*

#### Outputs:

* Plaintext *message*: *M*

#### Description:

To decrypt a message, **AES-SIV** first performs an **AES-CTR** decryption of
the message under the provided synthetic IV. The message headers
*H<sub>1</sub>* through *H<sub>m</sub>* and candidate decryption message are
then authenticated by **AES-CMAC**. If the computed `IV’` does not match the
original one supplied, the decryption operation is aborted. Otherwise, we've
authenticated the original plaintext and can return it.

## CHAIN

The **CHAIN** construction, originally described in the paper
[Online Authenticated-Encryption and its Nonce-Reuse Misuse-Resistance][CHAIN],
provides a segmented [authenticated encryption] scheme while still providing
[nonce reuse misuse resistance]. This makes it suitable for use cases that
require incremental processing, such as large file encryption, transport
encryption, or other "streaming" use cases.

![CHAIN Diagram](http://www.zcred.org/sivchain/images/chain.svg)

_NOTE:_ **CHAIN** support is forthcoming! None of the libraries in this
project presently implement CHAIN, but they will soon.

## Frequently Asked Questions (FAQ)

### Q: If AES-SIV is so great, why have I never heard of it?

A: Good question! It's an underappreciated gem in cryptography.

### Q: What does "SIV" stand for?

A: SIV stands for "synthetic initialization vector" and refers to the process
of deriving/"synthesizing" the initialization vector (i.e. the starting counter)
for AES-CTR encryption from the given message headers and plaintext message.

Where other schemes might have you randomly generate an IV, SIV modes
pseudorandomly "synthesize" one from the key, plaintext, and additional message
headers including optional associated data and nonce.

### Q: What's the tl;dr for why I should use this?

A: It provides stronger security properties at the cost of a small performance
hit as compared to **AES-GCM**. We hope to have benchmarks soon so we can show
exactly how much performance is lost, however the scheme is still amenable to
full hardware acceleration and should still remain very fast.

The **CHAIN** construction provides the only "streaming" misuse resistant
authentication encryption scheme with a rigorous security proof.

There are other libraries that try to solve this problem, such as [saltpack],
however these libraries do not provide constructions with security proofs,
nor do they provide misuse resistant authenticated encryption. In particular
[saltpack] is a rather complicated amateur construction which does many
repitious and redundant HMAC operations with little justification as to why
or if all relevant data is actually cryptographically bound, much less a
rigorous security proof.

[saltpack]: https://saltpack.org/

### Q: I saw this has "chain" in the name. Is it a cryptocurrency?

A: No.

### Q: When is your ICO and how do I buy into it?

A: Go away!

### Q: Are there any disadvantages to AES-SIV?

A: Using the AES function as a MAC (i.e. **AES-CMAC**) is more expensive than
faster hardware accelerated functions such as **GHASH** and **POLYVAL** (which
use _CLMUL_ instructions on Intel CPUs). Additionally **AES-CMAC** relies on
chaining and therefore cannot run in parallel. This makes **AES-SIV** slower
than **AES-GCM-SIV** on Intel systems, however **AES-SIV** provides better
security guarantees and will be faster on systems that do not have hardware
acceleration for **GHASH**. We hope to post benchmark numbers soon.

Due to the 128-bit size of the AES block function, **AES-SIV** can only be
safely used to encrypt up to approximately 2<sup>64</sup> messages under the
same key before the "birthday bound" is hit and repeated IVs become probable
enough to be a security concern. Though this number is relatively large, it is
not outside the realm of possibility.

### Q: Are there any disadvantages to the SIV approach in general?

A: SIV encryption requires making a complete pass over the input in order to
calculate the IV. This is less cache efficient than modes which are able
to operate on the plaintext block-by-block, performing encryption and
authentication at the same time. This makes SIV encryption slightly slower
than non-SIV encryption.

However, this does not apply to SIV decryption: since the IV is (allegedly)
known in advance, SIV decryption and authentication can be performed
block-by-block, making it just as fast as the corresponding non-SIV mode
(which for **AES-SIV** would be **AES-EAX** mode).

### Q: Isn't MAC-then-encrypt bad? Shouldn't you use encrypt-then-MAC?

A: Though SIV modes run the MAC operation first, then the encryption function
second, they are a bit different from what is typically referred to as
"MAC-then-encrypt". SIV modes cryptographically bind the encryption and
authentication together by using the authentication tag as an input to the
encryption cipher, making them provably secure for all the same classes of
attacks as encrypt-then-MAC modes.

Another common source of problems with MAC-then-encrypt is padding oracles,
which are commonly seen with CBC modes. **AES-SIV** is based on CTR mode, which
is a stream cipher and therefore doesn't need padding, making it immune to
padding oracles by design.

Authenticating the decrypted data does involve decrypting it, however. This
means decrypted data is, at one point in time, in memory before it is
authenticated. This increases the risk that attacker-controlled plaintext
might wind up being used due to authentication bugs.

These libraries attempt to ensure unauthenticated plaintext is never exposed.
Furthermore some libraries will perform the **AES-CTR** portion of **AES-GCM**
decryption without checking the GCM tag, so encrypt-then-MAC is not a
bulletproof solution to preventing exposure of unauthenticated plaintexts.
To some degree you will always be trusting the implementation quality of a
particular library to ensure it operates in a secure manner.

### Q: Is this algorithm NIST approved / FIPS compliant?

A: **AES-SIV** is the combination of two NIST approved algorithms:
**AES-CTR** encryption as described in [NIST SP 800-38A], and
**AES-CMAC** authentication as described in [NIST SP 800-38B].

However, while **AES-SIV** was [submitted to NIST] as a [proposed mode],
it has never received official approval from NIST.

If you are considering using this software in a FIPS 140-2 environment, please
check with your FIPS auditor before proceeding. It may be possible to justify
the use of **AES-SIV** based on its NIST approved components, but we are not
FIPS auditors and cannot give prescriptive advice here.

[submitted to NIST]: http://csrc.nist.gov/groups/ST/toolkit/BCM/documents/proposedmodes/siv/siv.pdf
[proposed mode]: http://csrc.nist.gov/groups/ST/toolkit/BCM/modes_development.html

### Q: Are there any patent concerns around AES-SIV mode?

A: No, there are [no IP rights concerns] with **AES-SIV** mode. To the best of
our knowledge, the algorithm is entirely in the public domain. 

[no IP rights concerns]: http://csrc.nist.gov/groups/ST/toolkit/BCM/documents/proposedmodes/siv/ip.pdf

### Q: Why not wait for the winner of the CAESAR competition to be announced?

A: The CAESAR competition (to select a next generation authentication encryption
cipher) seems to be taking much longer than was originally expected. Even when
it concludes, it will be some time before relevant standards are written as to
the usage and deployment of its winner.

Meanwhile [RFC 5297] is nearly a decade old, and **AES-SIV** has seen some
organic usage. While not entirely optimal by the metrics of the CAESAR
competition, it's a boring, uncontroversial solution we can use off-the-shelf today.

### Q: Do you plan on supporting AEZ in this library?

A: Maybe! [AEZ] is a newer, faster, parallelizable alternative to **AES-SIV**
with improved security properties, co-designed by [Phil Rogaway] who also
designed **AES-SIV**.

[AEZ]: http://web.cs.ucdavis.edu/~rogaway/aez/

### Q: Do you plan on supporting HS1-SIV in this library?

A: Maybe! [HS1-SIV] is an interesting SIV mode authenticated encryption cipher.
We are certainly keeping an eye on its development.

[HS1-SIV]: https://competitions.cr.yp.to/round2/hs1sivv2.pdf

### Q: This project mentions security proofs several times. Where do I find them?

A: Please see the paper
[Deterministic Authenticated-Encryption: A Provable-Security Treatment of the Key-Wrap Problem](http://web.cs.ucdavis.edu/~rogaway/papers/keywrap.pdf).


## Copyright

Copyright (c) 2017 [The Zcred Developers][AUTHORS].
Distributed under the MIT license. See [LICENSE.txt] for further details.

Some language-specific subprojects include sources from other authors with more
specific licensing requirements, though all projects are MIT licensed.
Please see the respective **LICENSE.txt** files in each project for more
information.

[AUTHORS]: https://github.com/zcred/zcred/blob/master/AUTHORS.md
[LICENSE.txt]: https://github.com/zcred/sivchain/blob/master/LICENSE.txt
