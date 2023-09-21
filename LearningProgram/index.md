---
title: Learning Programs
---

Here are some pointers and examples showing how to publish CTDL-formatted
JSON-LD metadata about a `LearningProgram` so that it can be easily ingested
into the Credential Registry with the Credential Registry Publish Action tool on
GitHub Actions.

## Learning Program Identifiers

The recommended practice for identifying a `LearningProgram` is to use the URL
of the JSON-LD file that contains the data for the `LearningProgram`. This is
the same as the recommended practice for identifying a `Credential` or
`Organization`. When the Learning Program references other entities, such as
Courses it contains, use URLs that resolve to the latest version of the data for
those entities.
