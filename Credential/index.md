---
title: Credentials
---

Here are some pointers and examples showing how to publish CTDL-formatted
JSON-LD metadata about a `Credential` so that it can be easily ingested into the
Credential Registry with the Credential Registry Publish Action tool on GitHub
Actions.

## Example Credential graphs

Included in this webpage is the example of a CTDL representation of a
[BachelorDegree credential in JSON-LD graph format](1/Credential-1-graph.json).

## Recommended Practices for Credential publishing

### Publish Graphs in CTDL, not individual entities for Credentials

The Credential Registry Publish Action tool supports the ingestion of
credentials published as a JSON-LD `@graph`. This enables the incorporation of a
variety of entities into a single file rather than requiring URI IDs for a
number of dependent objects, each of which must be fetched independently. With a
graph format, the data can be closest to the Credential Registry API input
format, which expects string IDs for certain relationship values, even if those
are "blank node identifiers". For example if an organization wanted to describe
that their Credential

### Select an appropriate Credential sub-type

The Credential Registry does not permit ingesting credentials with a generic
type; instead, implementers must be more specific and choose a relevant sub-type
like `ceterms:Certificate`. See more about the list of relevant Credential Types
in the
[Credential Minimum Data Policy](https://credreg.net/registry/policy#credential_required).

### SubjectWebpage must resolve to a web page successfully

The `ceterms:SubjectWebpage` property is required for all credentials. This
property must be a valid URL that resolves to a web page. The Credential
Registry will attempt to fetch the page and will reject the credential if the
page does not resolve successfully.

### Reference an organization ID in the Registry or on your site

References to `ceterms:ownedBy` and `ceterms:offeredBy` organization id will be
stripped out and replaced with the configured CTID input to the Credential
Registry Publish Action tool, but the values should be coherent within the data
published on your site.

Use a URL for these terms of an Organization record like this
[example](../Organization/1/Organization-1.json) or use the URL of the
production Credential Registry record for your Organization, in the form
`https://credentialengineregistry.org/resources/{ctid}` like
`https://credentialengineregistry.org/resources/ce-e3eed882-5c2c-4b48-baf9-ba1b47179d02`.
This ensures viewers of the data can navigate to an appropriate representation
of the organization.

Any other terms referencing an organization such as `ceterms:recognizedBy` may
be embedded in the Credential data (see [example](1/Credential-1.json)). A blank
node identifier for a node will be assigned automatically (or you could include
one manually as is done in the [example credential](1/Credential-1-graph.json)
in the alternate graph format). Using registry URLs will complicate
cross-registry environment testing, as requests may be rejected if the Registry
environment does not have a record of the referenced organization under the CTID
used for production.

### Example Data

- [Example Credential](1/Credential-1.json)
- [Example Credential (graph variant)](1/Credential-1-graph.json)
