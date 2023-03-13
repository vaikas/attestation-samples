# attestation-samples
This just contains various versions of attestations for testing things with
different versions of cosign.

In particular between different versions of cosign, some things changed. In
order to make testing interop between old/new versions, this repo simply
creates various predicateTypes and with different versions of cosign to make
them available for testing and visualizing what the changes were.

Some of the changes only affected the 'short' predicateType form provided by
cosign, but some had deeper changes, so I'll go through them here. Of course
for each of the cases below, the attestations did not go anywhere, and are still
there, but accessing them from cosign, or referring to them from
policy-controller is more cumbersome.

# SBOMs

## CycloneDX predicateType change
`https://cyclonedx.org/schema` => `https://cyclonedx.org/bom`

PredicateType was changed [here](https://github.com/in-toto/in-toto-golang/commit/86e515c6e15f0d9bc2cc9cb8253165f8176f6ce6)
which means that any attestations using CycloneDX stopped working with a newer
cosign unless you as a consumer knew which predicateType was used to create the
attestation. Attestations were still there of course, but you had to use a
different predicateType.

## Cosign predicateTypes changes
`cosign.sigstore.dev/attestation/vuln/v1` => `https://cosign.sigstore.dev/attestation/vuln/v1`
`cosign.sigstore.dev/attestation/v1` => `https://cosign.sigstore.dev/attestation/v1`

PredicateType was changed [here](https://github.com/sigstore/cosign/pull/2717).
This again means you have to know which predicateType was created before you
can use it.

## SBOM wrapper removal

[Here](https://github.com/sigstore/cosign/pull/2718) the attestation that was
created with the short vs. full predicateType was fixed. Before this, if you
created with the short type, the predicateType was correct, but the actual
predicate had an extra wrapper in it.

# Public images

ghcr.io/vaikas/attestation-samples/demo@sha256:cc0b27b2b24a0d5f9bc40ef0549155aec896940b85afe4af901e6be814fc0ca5

| Option      | Value |
| ----------- | ----------- |
| cosignVersion | v2.0.0 |
| sbomFormat | spdx-json |
| sbomPredicateType | https://spdx.dev/Document |
| vulnOutputFormat | json |
| vulnPredicateType | https://cosign.sigstore.dev/attestation/vuln/v1|
