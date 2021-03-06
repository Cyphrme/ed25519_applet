# Ed25519 Applet

Live demo: https://cyphr.me/ed25519_applet/ed.html

## This tool can run locally and offline

`git clone` this project to a local directory.  

```
git clone https://github.com/Cyphrme/ed25519_applet.git
```

Then, in your web browser, use `file://` to load `ed.html`

```
file:///pathToDirectory/ed.html
```

## Signing and verification tool for Ed25519


- Sign a message.
- Verify a signature.
- Generate a public key from seed.  
- Generate a new random public/private key pair and seed.

Supported formats:

Messages: Base64, Hex, and Text (Bytes).
Keys:     Base64 and Hex.



# Naming Differences in Implementations
Many libraries, including this tool, refer to what the RFC calls "private key"
as the "seed" (like [Go ](https://pkg.go.dev/crypto/ed25519)). The 32 byte seed
is used to generate the private component "secret scalar s", the public key, and
the "prefix" (nounce).

The ["actual" private component
](https://github.com/paulmillr/noble-ed25519/blob/ffdc7026d70297754a825f6e991426188891d1de/index.ts#L903)
(secret scalar s as [named by the RFC (Section 5.1.5.3))
](https://datatracker.ietf.org/doc/html/rfc8032#section-5.1.5") is typically
regenerated from seed on signing, although it is possible to use secret scalar s
and prefix to sign without the seed. The public component is computed from
"secret scalar s", but prefix is generated from seed and is used for signing.
  NaCL used to return the [private key as secret scalar s concatenated with
prefix
](https://blog.mozilla.org/warner/2011/11/29/ed25519-keys/#:~:text=%20is%20the%20private%20scalar).
Instead of requiring  secret scalar s and prefix for signing, most libraries
required seed and regenerate both secret scalar s and prefix from seed, and
optionally cache the public key.

What some libraries call the "private key" (64 bytes) is the seed (32 bytes)
concatenated with the public key (32 bytes). (Caching the public key precludes
relatively slow regeneration when signing.)


# TODO
#### Ed25519ph
https://github.com/paulmillr/noble-ed25519/issues/63

Paul's Noble library currently only supports "PureEdDSA" and does not support
Ed25519ph ("pre-hashed").  We are waiting for it to be supported before we can
implement it. 


#### Generate from seed "secret scalar s" and permit input from `sss || prefix`
It would be nice to output `"secret scalar s" || "prefix"` and accept it as
input as well.  See https://github.com/paulmillr/noble-ed25519/issues/64.  It
would require additional code to Noble since sss || prefix is not a possible
input, assuming seed is not given.  

We  might never do this if there's no use for it among all modern tools.  


# Dist
`noble-ed25519.js` is taken directly from Noble and may be used in other
applications. See also `join.js`.

## Other ed25519 resources:

- https://ed25519.cr.yp.to/
- https://en.wikipedia.org/wiki/EdDSA
- https://ianix.com/pub/ed25519-deployment.html


# Attribution
Implemented using noble/ed25519: https://github.com/paulmillr/noble-ed25519

"Cyphr.me" is a trademark of Cypherpunk, LLC. The Cyphr.me logo is all rights
reserved Cypherpunk, LLC and may not be used without permission.

# Keywords
Ed25519 test page, Ed25519 online tool.  


