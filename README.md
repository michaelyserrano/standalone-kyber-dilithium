# Standalone Kyber and Dilithium Packages
This is a self enclosed package for the CRYSTALS Kyber key encapsulation mechanism and the CRYSTALS Dilithium digital signature scheme. The purpose of this project is to provide for simpler implementations into future protocols.

# Kyber
## Usage/Examples
The key size is dependent on the code in the params.h file. Don't edit this file, but call it for any implementations.

To generate a static key, define a public and secret key and run the crypto_kem_keypair function:
```c
uint8_t pk[CRYPTO_PUBLICKEYBYTES];
uint8_t sk[CRYPTO_SECRETKEYBYTES];
crypto_kem_keypair(pk, sk);
```
To encapsulate and decapsulate a file, define output files and run the respective programs:
```c
#define cypher text
uint8_t ct[KYBER_CIPHERTEXTBYTES];
#define secret share
uint8_t ss[KYBER_SSBYTES];

crypto_kem_enc(ct,ss,pk);
crypto_kem_dec(ss,ct,sk);
```


## Demo

The test_kex.c file is a good example of the aforementioned processes in action together. To share the key between Alice and Bob, it makes use of separate Unilaterally Authenticated Key Exchange (UAKE) and AKE functions. Any of these methods work for the KEM, so consult this file for proper implementation.

# Dilithium
## Usage/Examples
The key size is similarly defined in the params.h file.

To generate a public key signature, define a public and secret key and run the crypto_sign_keypair function:
```c
uint8_t pk[CRYPTO_PUBLICKEYBYTES];
uint8_t sk[CRYPTO_SECRETKEYBYTES];
crypto_sign_keypair(pk, sk);
```

To sign a message m with length MLEN, define output message and message length variables and run the crypto_sign function:
```c
# input
uint8_t m[MLEN + CRYPTO_BYTES];
# output
uint8_t sm[MLEN + CRYPTO_BYTES];
size_t smlen;
# assign value to m (demo)
randombytes(m, MLEN);

crypto_sign(sm, &smlen, m, MLEN, sk);
```
Notice that the length of the actual output message (as well as the input) is CRYPTO_BYTES more than the listed length.

To verify a signed message, run the crypto_sign_open function with the following organization with m and mlen as outputs:
```c
crypto_sign_open(m2, &mlen, sm, smlen, pk);
```
Finally, to compute and verify just a signature, use the crypto_sign_signature and crypto_sign_verify functions respectively:
```c
crypto_sign_signature(sig,siglen,m,mlen,sk);
crypto_sign_verify(sig,siglen,m,mlen,pk);
```
crypto_sign_verify will return 0 if the signature is verified and -1 otherwise. 

**It is also worth noting that crypto_sign_open already runs this verification inside the function, so this step is not needed for pure implementation purposes.**
## Demo
The test_dilithium.c file showcases the aforementioned processes together. Consult this file for proper organization of the signature verification.
## Acknowledgements

Acknowledgements go to CRYSTALS for creating Kyber and Dilithium.
