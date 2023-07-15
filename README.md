Libsodium ChaCha20 Encryption Oracle
=========

This code uses libsodium's ChaCha20 implementation to show that an attacker that can leak memory, can also leak the secret symetric key used.

## The Vulnerable Service

To start the network service, you can use docker-compose:

```
cd stapelspass
docker-compose up -d
```

The service will then be build and deployed. It is now reachable on localhost, port 55432.
A comfortable way to interact with it is by using netcat:

```
nc localhost 55432
```

The service uses a signed variable for indexing, which is a common cause for memory corruption bugs. Due to this bug it is possible to access stack elements above the intended region (e.g. the secret key leaked stored on the stack by libsodium). 


## The Exploit

To leverage the programs memory corruption into leaking the secret key and decrypting the given ciphertext, an exploit is prepared in exploit/decrypt.py.
This exploit is written in Python and annotated. It requires the [PyCryptoDome](https://pypi.org/project/pycryptodome/) and [Pwntools](https://github.com/Gallopsled/pwntools) Python packages to be installed. 

The exploit can be started with:

```
python3 decrypt.py localhost 55432
```

