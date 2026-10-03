# <u>1: Introduction to Cryptography</u>
Foundational security principles:
1. **Know your threat**; assume that:
	- The attacker can interact with your systems without anyone noticing
	- The attacker has some general information about your system; OS, software vulnerabilities, etc.
	- The attacker is persistent and always lucky; ex. if an attack is successful each 100 attempts, the attacker will try 100 times
	- The attacker is willing to devote time and resources to the attack, though this may vary; see "security is economics" below 
	- The attacker can coordinate several complex attacks across various systems, is not limited to to attacking only a single device, and can attack your entire network at the same time or chain multiple attacks together
	- Every system is a potential target, ex. unnecessary IoT bloatware devices
2. **Consider human factors**; users themselves are likely to undermine their own security if the security systems cause them inconvenience
3. **Security is economics**; systems can only be feasibly secured against a certain level of attacks, and thus the cost of security against a given attack must scale proportionally to the expected damages of a said attack
	- As such, it is often best to secure the weakest links or lowest hanging fruit first
4. **Detect if you cannot prevent**
5. **Defense in depth**; while not a panacea, several layers of defense can be helpful
6. **Least privilege**; give any actor the set of access privileges that it legitimately needs to do its job, and nothing more
7. **Separation of responsibility**
8. **Complete mediation**; check every access to every object
9. **Shannon's Maxim:** do not rely on "security through obscurity"
10. **Fail-safes**; if the security system fails in a way that simply prevents it from being used further, then it fails safely
11. **Design security in from the start**
12. **Know the TCB**; the trusted computing base is the set of unbypassable, tamper-resistant, verifiable (correct) system primitives that we assume are given when designing a security system 

Important definitions:
- **Confidentiality:** adversaries cannot read private data, even if they have the ciphertext
- **Integrity:** adversaries cannot tamper with private data without being detected
- **Authenticity:** we can determine who if a given message was indeed authored by the claimed author (note this property assumes integrity)
- **Kerckhoff’s Principle:** cryptosystems should remain secure even when the attacker knows all internal details of the system; the key should be the only thing that must be kept secret, and the key should be easy to change
	- If your secrets are leaked, it is usually a lot easier to change the key than to replace every instance of the running software
	- This principle is of course closely related to Shannon’s Maxim

**Threat Models:**
- **Known Ciphertext Only:** the adversary has access to only the ciphertext
- **Known Plaintext:** the adversary has access to the ciphertext and partial or full access to the concomitant plaintext
- **Replay:** the adversary has access to the ciphertext and is able to resend it to the intended recipient
- **Chosen Plaintext:** the adversary can have the sender encrypt plaintexts of their choice and observe the resulting ciphertexts, in an attempt to gain information to be used to recover plaintexts from other ciphertexts
- **Chosen Ciphertext:** the adversary can have the receiver decrypt ciphertexts of their choice and observe the resulting plaintexts, in an attempt to gain information to be used to recover plaintexts from other ciphertexts
- **Chosen Plaintext / Ciphertext:** a combination of the two above; in the non-trivial case the adversary is not able to choose the plaintext to be encrypted and then choose that same resulting ciphertext to be decrypted
# <u>6: Symmetric-Key Encryption</u>
A more formal definition of **confidentiality** is that an adversary should not learn any information about a plaintext from seeing its ciphertext, ie. the probability of an adversary guessing which plaintext was sent is the same regardless of whether they can see the ciphertext or not

**IND-CPA:** indistinguishability-under-chosen-plaintext
1. An adversary sends two plaintexts $M_{1},M_{2}$ to the challenger, who randomly selects one $b\in \{ 0,1 \}$ uniformly and send a ciphertext $E(K,M_{b})=C$ back
2. The adversary can then send any arbitrary amount of plaintexts 
3. Finally, the adversary attempts to guess which of $M_{1},M_{2}$ was the plaintext of $C$
Clearly this is a chosen plaintext attack; then, a symmetric-key security system is IND-CPA secure iff the adversary's probability of guessing the correct plaintext after the game is the same as it was before; with some caveats:
- The messages $M_{1},M_{2}$ must be the same length, since the encryption **leaks the length of the plaintext** within bounds of one blocksize (more on this later) and thus would make it trivial for the adversary otherwise; but this doesn't indicate that a scheme is not IND-CPA secure in practice, so we force $|M_{1}|=|M_{2}|$
- In practice there is a limit the number of encryption requests of their choosing and receive their ciphertexts, in terms of computational feasibility
- The adversary only wins if the advantage gained is **non-negligible**; generally where the original probability of guessing the correct plaintext is $\frac{1}{2}$ and the probability after running the game is $\frac{1}{2}+n$, the adversary only wins when $n$ is some **non-negligible function**

**XOR:** the exclusive-or operation $\oplus:\{ 0,1 \}\times \{ 0,1 \}\to \{ 0,1 \}$ is defined by the following mappings:
- $0\oplus 0\mapsto0$
- $0\oplus 1\mapsto 1$
- $1\oplus 0\mapsto 1$
- $1\oplus 1\mapsto0$
Then, if we extend the XOR operation to bitstrings $a\neq b\neq c$ of any length $n$, we can easily derive:
- $a\oplus \{ 0 \}^n=a$; so $0$ is the **identity** element
- $a\oplus a=\{ 0 \}^n$; so any bitstring is its **own inverse**
- $a\oplus b=b\oplus a$; so XOR is **commutative**
- $(a\oplus b)\oplus c=a\oplus (b\oplus c)$; so XOR is **associative**
Then from these properties we can also derive some more useful ones:
- $a\oplus b\oplus a=b$; we can cancel out terms by XORing them by themselves, then $a\oplus 1=0\implies a=0\oplus 1$
- A
- Reusing the same OTP gives the adversary $C_{1}=M_{1}\oplus K$ and $C_{2}=M_{2}\oplus K$, then they can calculate $C_{1}\oplus C_{2}=(M_{1}\oplus K)\oplus(M_{2}\oplus K)=M_{1}\oplus M_{2}$ so they recover the XOR of any two plaintexts - not a full decryption but still a big leak of info (and if they somehow get any one plaintext, it becomes a full decryption since they can easily XOR back into the key)


MACs provide security against chosen-plaintext/ciphertext attacks, the strongest threat model.



