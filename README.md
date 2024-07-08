Practical Cryptography Engineering
==================================
This repository contains some practical code examples of using the following cryptography libraries:
* [libsodium](https://github.com/jedisct1/libsodium)
    * A modern, portable, easy to use crypto library written in C with a small number of high quality primitives
    * Focuses on making it easy to use cryptography correctly
* [mbedTLS](https://github.com/ARMmbed/mbedtls)
    * An ultra-portable crypto library written in C which should build anywhere
    * Provides a wide range of the most common cryptographic primitives and associated infrastructure
* [cryptography](https://github.com/pyca/cryptography)
    * Python's "standard" cryptographic library which is a wrapper around [OpenSSL](https://www.openssl.org)
    * Provides almost all cryptographic primitives you would want in Python
* [PyNaCl](https://github.com/pyca/pynacl)
    * Python bindings for libsodium (very partial wrapper around libsodium)
    * Provides a few nice cryptographic primitives not currently available in the cryptography module


File Contents
=============

Build-related and Miscellaneous
-------------------------------
* CMakeLists.txt
    * CMake file for building the mbedTLS C code projects
* mbedtls
    * Directory containing the mbedTLS C code
* sodium
    * Directory containing libsodium examples, headers, and Windows pre-compiled library
    * See the Readme.md in this directory for more info on these examples
    
Symmetric Encryption
--------------------
These code examples use an [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) block cipher with a 256-bit 
key in [Galois Counter Mode](https://en.wikipedia.org/wiki/Galois/Counter_Mode) (GCM).  The C code examples use the 
mbedTLS library, while the Python examples use the cryptography module.

* aes_gcm.c
    * Simple self-contained C code example of using AES-256 in Galois Counter Mode (GCM) using hard-coded everything
* aes_gcm_cryptography.py
    * Simple self-contained Python code example identical to the above
* aesgcm_file.c
    * C code example of file-based AES-256 GCM, works with aesgcm_file.py
    * Takes arguments on command line and produces output to file
* aesgcm_file.py
    * Python code example of file-based AES-256 GCM, works with aesgcm_file.c

The following example uses the PyNaCl wrapper around libsodium along with the **SecretBox** authenticated
encryption API.
* nacl_symmetric_gen.py
    * Generates a random 256-bit (32-byte) secret symmetric key and saves it to a file
    * Then uses it to encrypt a fixed message and verify that it can decrypt it and get the same message
    
Key Exchange
------------
These code examples use an Elliptic-curve Diffie-Hellman [ECDH](https://en.wikipedia.org/wiki/Elliptic-curve_Diffie–Hellman)
key agreement protocol to establish a shared secret over an insecure channel.  The C code examples use the mbedTLS 
library, while the Python examples use the cryptography module.

* ecdh.c
    * Elliptic Curve Diffie-Hellman key exchange C code example
* ecdh.py
    * Elliptic Curve Diffie-Hellman key exchange Python code example
    
Key Derivation
--------------
These code examples demonstrate how to use a Key Derivation Function [KDF](https://en.wikipedia.org/wiki/Key_derivation_function)
to derive one or more shared keys from a shared secret.

* kdf.c
    * Key Derivation Function (KDF) C code example
* kdf.py
    * Key Derivation Function (KDF) Python code example
    
Digital Signatures
------------------
These examples use the PyNaCl wrapper around libsodium to support public-key digital signatures using the Ed25519 algorithm.

* nacl_genkey.py
    * Generates a random ed25519 SigningKey/VerifyingKey key pair for use with a digital signature system 
* nacl_sign.py
    * Uses PyNaCl to sign a message using ed25519 digital signature algorithm
* nacl_verify.py
    * Uses PyNaCl  to verify an ed25519 signature for a given message

These examples use RSA-PSS digital signatures.  The C code examples use the mbedTLS library, while the Python examples 
use the cryptography module.
* rsa_signature.c
    * RSA Signature C code example
* rsa_signature.py
    * RSA Signature Python code example


Building
========

libsodium C examples
--------------------

The [libsodium](https://download.libsodium.org/doc/) C code examples are all in the **sodium** directory and can be 
built using the [Cmake](https://cmake.org) cross-platform build tool along with your platform default C compiler
installed on Windows, macOS, or Linux.

The first stage of building is the same on all platforms:

```bash
cd sodium
rm -rf build
mkdir build
cd build
cmake ..
```

The second stage of building is platform dependent and will create the following executable files:

* hello_sodium
* nacl_keygen
* nacl_sign
* nacl_verify
* symmetric_decrypt
* symmetric_encrypt
* symmetric_keygen
* test_ed25519
* test_pynacl_compatibility

### Linux or macOS
```bash
make
```

This produces the  executable files directly in the **build** directory.

### Windows
```bash
devenv hello_sodium.sln /build Debug
```
This creates the executable files under the **build\Debug** directory.

Python examples
---------------

The Python examples are located in the root directory and should work with Python 3.4 or newer.  The Python examples 
require a mix of the following Python packages:

* [cryptography](https://cryptography.io/en/latest/) - high-level wrapper around [OpenSSL](https://www.openssl.org)
* [pynacl](https://pynacl.readthedocs.io/en/stable/) - Python binding to [libsodium](https://libsodium.org)
* [colorama](https://github.com/tartley/colorama) - cross-platform colored terminal text

The required dependencies can easily be installed using [Pipenv](https://github.com/pypa/pipenv):
```shell script
pipenv install
```

Then a shell using the underlying virtual environment can be entered with:
```shell script
pipenv shell
```

Inside that Pipenv shell, any of the examples can be ran directly. e.g.:
```shell script
python ./aes_gcm_cryptography.py
```

The Python examples are intended to interoperate with either the libsodium or mbedTLS C code examples.  Thus encryption
or signing can be done in C and decryption or verifying can be done in Python or vice versa.

mbedtls C examples
------------------
The [mbedTLS](https://github.com/ARMmbed/mbedtls) C code examples are located in the root directory and build mbedTLS
from source from the **mbedtls** directory.

Build requires CMake and platform default C compiler installed and works on both Windows, macOS, and Linux.

The first stage of building is the same on all platforms:

```bash
rm -rf build
mkdir build
cd build
cmake ..
```

The second stage of building is platform dependent ...

### Linux or macOS
```bash
make
```

This produces the following executable files directly in the **build** directory:

* aes_gcm
* aesgcm_file
* ecdh
* kdf
* rsa_signature

### Windows
```bash
devenv mbed_AES.sln /build Debug
```
This creates the following executable files under the **build\Debug** directory:

* aes_gcm.exe
* aesgcm_file.exe
* ecdh.exe
* kdf.exe
* rsa_signature.exe


Where to learn more about cryptography
======================================

Books
-----

* [Cryptography Engineering](https://www.amazon.com/Cryptography-Engineering-Principles-Practical-Applications/dp/0470474246)
by Niels Ferguson, Bruce Schneier, and Tadayoshi Kohno
    * Extremely well written and easy to understand
    * Focuses on the practical aspects that often result in weak crypto when used incorrectly
    * Discusses how to build an entire cryptographic system from the ground up
* [Understanding Cryptography](https://www.amazon.com/Understanding-Cryptography-Textbook-Students-Practitioners/dp/3642041000)
by Christof Paar, Jan Pelzl, and Bart Preneel
    * Amazing book which makes it relatively easy to teach yourself cryptography
    * [Website](http://www.crypto-textbook.com)
    * YouTube lecture [videos](https://www.youtube.com/watch?v=2aHkqB2-46k&list=PL6N5qY2nvvJE8X75VkXglSrVhLv1tVcfy)
    * [Solutions](http://wiki.crypto.rub.de/Buch/en/download/Understanding_Cryptography_Odd_Solutions.pdf) Manual,
    Lecture [Slides](http://wiki.crypto.rub.de/Buch/en/slides.php)
    
Online Courses    
--------------

* [Cryptography I](https://www.coursera.org/learn/crypto)
    * Taught by Stanford University professor Dan Boneh
    * Available for free on Coursera
* [Applied Cryptography](https://www.udacity.com/course/applied-cryptography--cs387)
    * Taught by University of Virginia professor Dave Evans
    * Available for free on Udacity
    
   
Presentation
------------
This repository includes the following basic intro presentation:
* [Practical Cryptography Engineering](https://github.com/tleonhardt/practical_cryptography_engineering/blob/master/Practical_Cryptography_Engineering.pdf)
    
    
Disclaimer
==========
I am not an expert in cryptography.  I am just a software developer who wanted to learn more about how to use 
cryptography effectively.  If anyone looking at this **is** an expert in cryptography and happens to notice any 
weaknesses, inaccuracies, or mistakes and/or has constructive feedback for how to improve the examples then Pull 
Requests or Issues would be sincerely appreciated.

