1. #### Which of the following are standards used in Federated Identity Management? (select all that apply)
&nbsp - [x] SAML
  - [x] XML
  - [ ] OAuth
  - [x] SOAP
  - [ ] OpenID



2. #### Kerberos ... (select 3)
- [ ] exists because local authentication cannot be trusted
- [ ] requires strong passwords
- [x] uses asymmetric encryption for user authentication
- [x] is an open standard for distributed authentication
- [x] provides authentication service to distributed services



3. #### Needham-Schroeder uses nounces and a central authenticator.
- [x] True
- [ ] False



4. #### Which of the following are used to defend remote authentication against replay attacks? (select all that apply)
- [ ] static biometric
- [ ] password
- [x] sequence number
- [x] nounce
- [x] timestamp



5. #### Which of the following is the NIST recommended symmetric key encryption algorithm for highest level security?
- [ ] RSA
- [ ] ECC
- [ ] 3DES
- [ ] DES
- [x] AES



6. #### Cryptographic systems are distinguished by which of the following characteristics? (select 3)
- [ ] whether the encryption algorithm is known or not
- [ ] whether they provide confidentialy or not
- [x] the type of transformation used
- [x] the way plain text is processed
- [x] the number of keys used



7. #### Match each use to the best public key algorithm
| Use | Public Key Algorithm |
| --- | ----------|
| Only for Key Enchange | **``Diffie-Hellman``** |
| Only for Digital Signature | **``DSS``** |
| For Encryption/Decryption, Digital Signature, and Key Exchange | **``RSA``** |



8. #### Match each to the best description
| Term | Description |
| --- | ----------|
| OSSTMM | **``A manual for security testing produced by institute for Security and Open Methodologies (ISECOM)``** |
| CISSP | **``A security certification focused on policy and procedures``** |
| SANS | **``An organization that monitors and produces training and certification based on common exploits``** |
| CEH | **``A certification that focuses on ethical hacking``** |



9. #### Which of the following is a Feistel Cipher? (select all that apply)
- [ ] Vernam
- [x] DES
- [ ] AES
- [ ] RSA
- [x] Triple DES



10. ##### A good stream cypher should have which of the following characteristics? (choose all that apply)
- [x] fater than block cyphers
- [ ] key must be smaller than 128 bits
- [x] output appears random
- [ ] Key must be larger than 128 bits
- [x] a large period



11. Which algorithm is described as follows?
**C = M^e mod n**

- [ ] 3DES
- [ ] PGP
- [ ] AES
- [ ] Caesar
- [x] RSA



12. Which algorithm is described in the following:
**C = E(k, p) = (p + k) mod 26**

- [x] Caesar
- [ ] Vigenere
- [ ] Rail Fence
- [ ] Vernam
- [ ] Row Transposition



13. Which of the following alternates substitutions and permutations?
- [ ] Vernam
- [ ] PRNG
- [x] Feistel Cipher
- [ ] RC4
- [ ] Caesar



14. Elliptical Curve may be preferred rather than RSA due to which of the following characteristics? (select 3)
- [x] ECC is recommended up to top secret level by NSA and NIST
- [x] ECC is computationally less intensive than RSA
- [ ] ECC is useful for encryption, signatures, and autentication but RSA is limited to as encryption function only.
- [ ] ECC has been more extensively tested than RSA
- [x] ECC is equally effective even with smaller key sizes than RSA

15. Explain the structure and function of x.509 certificates.  (More points will be awarded for more complete answers.)



16. Decentralized Key Distribution is impractical because . . .
- [x] as the size of the parties grows, the required number of master keys become excessive to manage
- [ ] session keys replace master keys in the distribution
- [ ] each end system must generate a unique master key for itself
- [ ] it isn't possible to distinguish betweeen key usage options



17. Which of the following are possible options for symmetric key distrubution as discussed in Stallings? (select all correct answers)
- [x] use a key distribution service
- [ ] post the key to a public server
- [x] careful physical delivery
- [x] transmit a new key that has been encrypted with an already known shared key



18. CVE
- [ ] determines the value of vulnerability research
- [ ] is a national vulnerability database maintained by mitre.org
- [x] enables automated sharing of information regarding vulnerabilities
- [ ] clarifies the damage done by common vulnerabilities



19. Which of the following tools test for vulnerabilities?
- [x] Acunetix
- [x] Openvas
- [x] Saint
- [ ] Wireshark
- [ ] Snort



20. Describe the process used to secure communications using IPSec. (More points will be awarded for more complete answers.)
> One type of vulnerability that I would like to talk about is Buffer Overflow.  Overall, when an attacker uses buffer overflow, their intent is to prevent the current program from ending normally, while passing control to another program. Â First, we need to understand the stack to know what an attacker is attempting to do. Â Just like an ordinary stack, we can push items onto it and we can remove (or pop) from it. Â When a new function is called, at the top of the stack, new items will be pushed. Â Some of those items are arguments (specific to the new function), the return address, the stack pointer, and the actual buffer, which holds all the variables that are local to the new function. 
> 
> If we think of the stack as a tower, falling towards the left, then the buffer will be on the left side and the arguments will be on the right side. Â Therefore, the order, as we write into the memory will be from the left side to the right side. Â If the input to the buffer (the local variables) is not check, then some of it might spill into the stack pointer, the return, and/or the arguments, thus causing an overflow of the buffer. What an attacker is concerned about is overwriting the return address, hence buffer overflow. Â If he/she can place a valid address of a rogue program in the current function's return address, then the current program will not terminate, but the control will transfer to the rogue program, thus hijacking the system. Â More importantly, if the current function was part of a program running in kernel mode, then the rogue application will execute in kernel mode as well.
> 
> Fortunately, Linux provides protection, or countermeasures, against such attacks, specifically Address Space Randomization and StackGuard Protection Scheme. Â Address space randomization changes the address of the stack and heap, so that it's harder for an attacker to use constants to find it, though it is not safe from a brute-force attack. Â StackGuard protection scheme works differently by marking the stack as non-executable, so an execute instruction, such as jumping to another part of the memory space, will not be allowed, causing the operating system to terminate the current program. Â Both of these options help a lot. Â Programmers can do more by using safe functions. Â For example, instead of using strcpy() they can use strncpy(), so that we can set a limit of the number of bytes that can be transferred. Â Using all three methods will help minimizing vulnerabilities.

