---
title: Organization
---

All primary classes in CTDL reference their author organization in some way. The
Credential Registry Publish Action assumes that your Organization record is
already published to the Credential Registry, because it needs to be prior to
obtaining an API key, and the Action does not feature the ability to update the
Organization record directly. But in the course of publishing the other objects
in your graph, there will likely be some reference to your organization. This
document describes recommended practices for publishing this Organization data
so that the subsidiary objects can be easily ingested into the Credential
Registry.

One option is to simply reference the production registry's Organization record
in your objects. This means that a URL representing the Organization would
appear in your Credentials or LearningPrograms like:

```json
    "ownedBy": ["https://credentialengineregistry.org/resources/ce-2b042ffc-fe9e-4408-b2ae-2bd6c58a6c8d"]
```

This enables viewers to navigate to a useful piece of linked open data that
describes the organization and does not pose any problems for ingestion.

Another option is to publish your Organization record to a URL on your own
domain. In that case, you would reference that URL in your Credentials or
LearningPrograms like:

```json
    "ownedBy": ["https://example.com/organization/data.json"]
```

When the Credential Registry Publish Action tool is triggered to publish or
update data on a particular Registry environment (such as `sandbox` or
`production`), such references will be replaced by the configured CTID in the
workflow Environment Variables. This leaves your source data in a coherent state
with all internal references intact, while also enabling this data to be
published to the Registry, where the identifier of all records will be a
Registry URL that includes the CTID for the organization on that Registry
environment.
