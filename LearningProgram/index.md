---
title: Learning Programs
---

Here are some pointers and examples showing how to publish CTDL-formatted
JSON-LD metadata about a `LearningProgram` so that it can be easily ingested
into the Credential Registry with the Credential Registry Publish Action tool on
GitHub Actions.

## Learning Program Identifiers

The recommended practice for identifying a `LearningProgram` with an `@id` in
JSON is to use the URL of the JSON-LD file that contains the data for the
`LearningProgram`. This is the same as the recommended practice for identifying
a `Credential` or `Organization`. When the Learning Program references other
entities that have CTIDs, such as a `Course`, use URLs that resolve to the
latest version of the data for those entities.

### Option: Publish Graphs in CTDL format for LearningPrograms

The Credential Registry Publish Action tool supports the ingestion of a
`LearningProgram` (a subclass of `LearningOpportunity`) published as a JSON-LD
`@graph`. This enables the incorporation of a variety of entities into a single
file rather than requiring URI IDs for a number of dependent objects, each of
which must be fetched independently. With a graph format, the data can be
closest to the Credential Registry API input format, which expects string IDs
for certain relationship values, even if those are "blank node identifiers".

For example if an organization wanted to describe that their Courses as embedded
objects in a Learning program without a unique URL for each, make sure to still
assign a CTID (`ceterms:ctid`) for this and any other resource that needs one.

## Example Files

**Example Learning Program: Lifelong Learning Accelerator**

This learning program consists of three courses, published separately, and it
includes a significant amount of metadata, including estimated costs, required
prerequisites, and related occupations.

- [LearningProgram-1.json](1/LearningProgram-1.json)
  - hasPart: [Course-1a.json](1/Course-1a.json)
  - hasPart: [Course-1a.json](1/Course-1a.json)
  - hasPart: [Course-1a.json](1/Course-1a.json)
  - ownedBy: [Organization-1.json](../Organization/1/Organization-1.json)
- [LearningProgram-1-graph.json](1/LearningProgram-1-graph.json) - a JSON-LD
  graph variant of the above.

**Example Learning Program: Cybersecurity Program**

- [LearningProgram-2.json](2/LearningProgram-2.json) - Program that doesn't
  reference any parts.
