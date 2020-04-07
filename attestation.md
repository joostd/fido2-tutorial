# Device Attestation

When registering authenticators, *device attestation* can be used to determine the make and model of the particular authenticator used. See FIDO TechNote [The Truth about Attestation](https://fidoalliance.org/fido-technotes-the-truth-about-attestation/) for details.

Device attestation can be useful, for instance to identify keys in case a vulnerability is discovered impairing the authenticator's security properties, or when you need to whitelist authenticators known to have some security properties.

This information is conveyed using the attestation certificate. Vendors are expected to use the same attestation key and certificate for large batches of authenticators for privacy reasons.

There are several ways to compile a whitelist:

1. using attestation certificates (or their fingerprints)
2. using CA certificates that issued attestation certificates
3. using AAGUID's
4. using OIDs
5. using attestationCertificateKeyIdentifier

Note: 1.3.6.1.4.1.45724.2.1.1 (id-fido-u2f-ce-transports)
is used for enumerating Transports

1.3.6.1.4.1.45724.1.1.1 (`id-fido-gen-ce-aaid`) and
1.3.6.1.4.1.45724.1.1.4 (`id-fido-gen-ce-aaguid`) are used for embedding the AAID oro AAGUID in a certificate extension. 

## Yubico

The attestation certificate contains an X.509 v3 extension with OID 1.3.6.1.4.1.41482.2  that you can match against 
[yubico-metadata.json](https://developers.yubico.com/U2F/yubico-metadata.json)

## Feitian

See the FIDO Alliance Metadata.

An [online viewer](https://fido-mds-parser.appspot.com) may be useful.


## Solokeys

See Solokeys' [metadata repository](https://github.com/solokeys/solo/tree/master/metadata) on github.

## Other

TO DO
