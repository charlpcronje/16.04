# Checksums and how to verify now

- How to verify downloaded Ubuntu `ISO` image checksum step by step instructions
- First step is to download Ubuntu `ISO` image. Most likely you have already completed this step. In this tutorial we will use and download `Ubuntu 20.04 ISO` image. Before you proceed to the next step you should have Ubuntu `ISO` image available.

## Example

```sh
ls
```

`focal-desktop-amd64.iso`

From the same Ubuntu server location you have downloaded the actual ISO image you will also need to download relevant `SHA256SUMS` checksum and `SHA256SUMS.gpg` signature files.

## DID YOU KNOW?

That you can verify the `Ubuntu ISO` image checksum using either `SHA1SUMS` or `SHA256SUMS` or `MD5SUM` message digests. Any of these verification methods are valid and you should pick the one which best suits your needs. The verification method procedure is the same for all three.

## checksum and signatures

Available `SHA256SUMS` checksum and `SHA256SUMS.gpg` signature files along with the actual Ubuntu ISO image.
Once ready the content of your directory should at this stage contain the following files:

```sh
ls
# focal-desktop-amd64.iso  SHA256SUMS  SHA256SUMS.gpg
```

Next, we need to obtain the correct signature key in order to authenticate the content of the SHA1SUMS checksum file. To do so execute the below gpg command:

```sh
gpg --verify SHA256SUMS.gpg SHA256SUMS
# gpg: Signature made Mon 09 Mar 2020 18:58:10 AEDT
# gpg:                using RSA key D94AA3F0EFE21092
# gpg: Can't check signature: No public key
# The above output indicates that the signature key used is D94AA3F0EFE21092 and that our system currently does not have 
```

This key available. To import the missing signature key run:

```sh
gpg --keyserver hkp://keyserver.ubuntu.com --recv-keys D94AA3F0EFE21092
gpg: key D94AA3F0EFE21092: public key "Ubuntu CD Image Automatic Signing Key (2012) " imported
gpg: Total number processed: 1
gpg:               imported: 1
```

Inspect the output of the above import command and check for the public key owner.

With the Ubuntu CD Image Automatic Signing Key imported we are ready to validate the content of the SHA1SUMS checksum file:

```sh
gpg --verify SHA256SUMS.gpg SHA256SUMS

gpg: Signature made Mon 09 Mar 2020 18:58:10 AEDT
gpg:                using RSA key D94AA3F0EFE21092
gpg: Good signature from "Ubuntu CD Image Automatic Signing Key (2012) " [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 8439 38DF 228D 22F7 B374  2BC0 D94A A3F0 EFE2 1092
```

The ouput of the above command should produce the Good signature message.

Last step is to verify the digest checksum of the Ubuntu ISO image and compare it with the content of the SHA1SUMS checksum file. To do so execute:

```sh
sha256sum -c SHA256SUMS
# focal-desktop-amd64.iso: OK
```

Alternatively, you can simply generate the checksum first and compare the ouput with the content of the checksum file manually. Both checksum’s should match:

```sh
sha256sum focal-desktop-amd64.iso 
8807ddb1927e341c97031c20da88368276be4e3601c31846db41e32cb44027ef  focal-desktop-amd64.iso

cat SHA256SUMS
8807ddb1927e341c97031c20da88368276be4e3601c31846db41e32cb44027ef *focal-desktop-amd64.iso
```
