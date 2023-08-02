# Standalone Kyber and Dilithium Packages
This is a self enclosed package for the CRYSTALS Kyber key encapsulation method and the CRYSTALS Dilithium digital signature scheme.  The purpose of this project is to provide for simpler implementations into future protocols.

# Kyber
## Usage/Examples
The key size is dependent on the code in the params.h file. Don't edit this file, but use it 

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
