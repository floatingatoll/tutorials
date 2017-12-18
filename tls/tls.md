# TLS training

Last updated: July 2017

## What are SSL and TLS?

* The terms "SSL/TLS", "SSL", and "TLS" are typically used to mean the same thing.
* They mean "OpenSSL-compatible SSL/TLS-based wire encryption".
* Unless it refers to a specific version: "SSL 3.0", "TLS 1.x", "TLS 1.3".
* SSL protocol was replaced by TLS protocol decades ago, but the name "SSL" stuck.

## What's the SSL/TLS protocol?

* SSL/TLS communications use an extensible packet protocol originally defined by SSL.
* The client and server negotiate crypto-math algorithms and secure the link.
* Crypto algorithms are used for: initial handshake, private keys, signatures.
* Cipher strings define the set of crypto algorithms the client and server will permit.

## What are 'cipher strings'?

* Everyone has different ways of configuring which crypto-math to use for which phases.
* OpenSSL, GnuTLS, Zeus each have different mechanisms of crypto-math selection.
* These are all shorthanded to the phrase 'cipher string' because naming is hard.
* Writing cipher strings by hand is like writing CSP `*-src` headers, so use the EIS generator.

## How do we do SSL in AWS?

* Typically we configure ALB/ELB/CloudFront to decrypt incoming traffic.
* Amazon Certificate Manager issues certificates usable only with ALB/ELB/CF.
* Digicert issues certificates that we deploy by hand via any method (Consul, scp, etc.)
* We still should try to configure those to reencrypt traffic to/from backend nodes.

## How do we do SSL in datacenters?

* Typically we configure the Zeus 'virtual server' to enable 'SSL Decrypt' mode.
* Zeus can handle decryption of most SSL/TLS connections, with extra features for HTTPS.
* Autocert prepares and uploads SSL certificates to the Zeus SSL catalog.
* Zeus virtual servers are configured to use one or more certificates from the SSL catalog.

## Why do we use Zeus instead of Apache for SSL?

* Trafficscript and logging requires decrypted requests and responses to work well.
* We can pass through without decrypting to backend nodes on security-critical sites.
* We still should configure Zeus and Apache to reencrypt traffic to/from backend nodes.
* Decryption in AWS could occur in many places, based on the app's AWS architecture.

## How does Zeus use multiple certificates on one traffic IP?

* A human configures the Zeus virtual server to map SNI hostnames to SSL catalog objects.
* SSL clients tell the server using the TLS SNI extension the hostname they're requesting
* The server looks up that hostname in the virtual server map and selects a certificate to use.
* Zeus can also be instructed to present both RSA and ECDSA certificates.

## How do SAN certificates work?

* A new property `serverAltNames` was added to the extensible X.509 certificate file format.
* A new property `serverNameList` was added to the extensible SSL/TLS wire protocol.
* Clients tell the server which hostname they're trying to reach and get back a certificate.
* Then they verify that the certificate contains a `serverAltNames` matching what they tried.

## How do certificate signatures work?

* You generate, in order: a private key, public key, signature request, and signed certificate.
* Private keys are a very long base64'd password that is used solely by the crypto math.
* Public keys use crypto math to safely represent your private key without divulging it.
* Signature requests and signatures define a set of X.509 properties (e.g. `serverAltNames`).

## What is a signing authority?

* Signed certificates have a "signature", usually from someone's `CA:true` X.509 certificate.
* The `CA:true` X.509 property is used to identify "Certificate Authority" certificates.
* The `CA:true` certificate is used with crypto math and the `CSR` to create a signed certificate.
* We have used many signing authorities: GeoTrust, Digicert, Let's Encrypt, Mozilla Root CA.

## What is a chained intermediate certificate?

* CAs use their browser-trusted `CA:true` certificate to sign one or more 'intermediate' certs.
* The intermediate certs are `CA:true`, and are used to sign certificate requests every day.
* Each CA-signed cert is signed by exactly one of those intermediate certs.
* We publish both signed cert and chained cert to help browsers see the chain of trust.

## Is it safe to share any of the keys, requests, or certificates publicly?

* It is DANGEROUS to share the private key (`KEY`). If leaked, you must revoke its certificates.
* It is UNCOMMON to share the public key. It's safe, and it's embedded in all signed certs.
* It is COMMON to share the signature request (`CSR`). It's a safe way to get a signed cert.
* It is EVERYDAY to share the signed certificate (`CRT`). It's sent as part of every TLS request.

## What's a RSA modulus?

* Your RSA private key, certificate request, and signed certificate share a 'modulus'.
* It's trivial to prove that a key generated a request or signature, they all share the 'modulus'.
* It's difficult to guess the RSA private key from a 'modulus', as prime factoring is hard.
* Signed certificates are public as their 'modulus' binds them with crypto-math to your key.

## What's the difference between RSA and ECDSA?

* RSA depends on the inability to factor large numbers efficiently.
* ECDSA depends on the inability to find a specific point on an ellipse.
* Many smart people think ellipse math is more future-proof than prime number math.
* Zeus can serve dual RSA/ECDSA sites if Autocert implements it.

## Why is ECDSA going to replace RSA someday?

* Given a 'modulus', you can eventually determine the private key, breaking the encryption.
* Breaking RSA requires factoring a large number into two prime numbers, which is hard.
* Breaking ECDSA requires guessing a point on an ellipse, which is much harder.
* It only costs millions of dollars and little time to break RSA. No feasible process for ECDSA.

## Why are SHA-1 signatures deprecated?

* The crypto-math algorithms SHA-1, SHA-2 are used to make 'signatures' from your key.
* SHA-1 has had well-known mathematical defects for years. SHA-2, its replacement, hasn't.
* The authority over 'certificate authorities' set an end-of-life date for SHA-1 to resolve this.
* Browsers will eventually no longer trust SHA-1 certificates, ending any widespread use.
