---
title: Learning Programs
---

Here are some pointers and examples showing how to publish CTDL-formatted
JSON-LD metadata about a `LearningProgram` so that it can be easily ingested
into the Credential Registry with the Credential Registry Publish Action tool on
GitHub Actions.

### Publish Graphs in CTDL format for LearningPrograms

The Credential Registry Publish Action tool supports the ingestion of a
`LearningProgram` (a subclass of `LearningOpportunity`) published as a JSON-LD
`@graph`. This enables the incorporation of a variety of entities into a single
file rather than requiring URI IDs for a number of dependent objects, each of
which must be fetched independently. With a graph format, the data can be
closest to the Credential Registry API input format, which expects string IDs
for certain relationship values, even if those are "blank node identifiers". For
example if an organization wanted to describe that their Credential

## Learning Program Identifiers

The recommended practice for identifying a `LearningProgram` is to use the URL
of the JSON-LD file that contains the data for the `LearningProgram`. This is
the same as the recommended practice for identifying a `Credential` or
`Organization`. When the Learning Program references other entities, such as
Courses it contains, use URLs that resolve to the latest version of the data for
those entities.
