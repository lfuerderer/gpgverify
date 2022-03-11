Check gpg signatures in an automated way, for example in scripts.

# Motivation

What could possibly go wrong?

```sh
gpg --verify somefile.gpg && echo VALID || echo INVALID
```

There are a few problems with this approach:

- A signature made with an expired key is still accepted.
- A signature made with a revoked key is also accepted.
- Any key in your keyring is accepted, you cannot specify who you expect to have signed the document.
- Also unvalidated keys in your keyring (not validated by Web-of-Trust) are accepted.

The problem here is, that `gpg` acutally **shows warnings** about these issues, but still exits with code `0`, indicating that everything is fine.

## What about gpgv?

The [gpgv](https://www.gnupg.org/documentation/manuals/gnupg/gpgv.html) command is shipped along gpg, so you likely have it already installed. `gpgv` relies on an additional keyring `trustedkeys.gpg` to contain all keys you like to accept signatures from. But `gpgv` also does not check for expired, revoked or unvalidated keys, so maintaining this keyring is fully up to you.

# Using gpgverify

```sh
gpgverify --accept-fp 0123456789abcdef0123456789abcdef01234567 somefile.gpg && echo VALID || echo INVALID
```

Simple as that, specify the fingerprint of the key you expect to have signed and tell the filename. If the given key is expired or unvalidated, the signature is considered invalid.

## Detached signature

For detached signature, you give two arguments: First the signature file, second the data file. (This is the same syntax as in `gpg --verify`)

```sh
gpgverify --accept-fp 0123456789abcdef0123456789abcdef01234567 somefile.gpg somefile && echo VALID || echo INVALID
```
