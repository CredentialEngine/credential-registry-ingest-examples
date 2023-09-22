---
title: Credential Registry Publish Action
---

Credential Engine is now offering a GitHub Action that makes it easy to publish
data to the Credential Registry. The action is available in the
[GitHub Marketplace](#TBD) and may be incorporated in partner organizations'
CI/CD pipelines as new data is published. Publication may be triggered manually,
scheduled to run at regular intervals, or triggered as a step in a larger
workflow immediately following applicable data updates. Use the action as best
fits your organization's needs, and provide feedback to Credential Engine to
help us improve the action.

## Philosophy for self-publishing linked open data

When organizations publish data themselves, the overall pattern is to select
useful identifiers (URLs) for objects and make data available about the objects
at those URLs, and potentially at other URLs as well (such as when those objects
appear within a graph).

The [5-star Open Data](https://5stardata.info/en/) plan contains some useful
guidance for publishing linked open data. The Credential Registry Publish Action
tool is designed in line with these recommendations:

- `★` Make your stuff available on the Web (whatever format) under an open
  license
- `★★` Make it available as structured data (e.g., Excel instead of image scan
  of a table)
- `★★★` Make it available in a non-proprietary open format (e.g., CSV instead of
  Excel)
- `★★★★` Use URIs to denote things, so that people can point at your stuff
- `★★★★★` Link your data to other data to provide context

The recommended practices for using the Credential Registry Publish Action are
also in line with the [Open Skills Network](https://openskillsnetwork.org) makes
for publishing
[Rich Skills Descriptors (RSDs)](https://rsd.openskillsnetwork.org). This
includes that linked open data entities should:

- Identify objects with a canonical URL.
- Publish data in an open and standardized or standards-track format at the URL
  for the object.

## Recommended practices for publishing data to the Credential Registry

Here are some specific recommendations for publishing data on organizations' own
websites that will be used for ingestion into the registry using the Credential
Registry Publish Action.

### Publish data in CTDL JSON-LD similar to how it is presented in the Registry

This website and its examples, as well as the error messages you might encounter
when publishing data, will guide you in adopting the structure used by the
Registry to publish data in the open vocabulary
[CTDL](https://credreg.net/ctdl/terms).

- Use language maps for string values, identifying the language in which they
  appear. This is preferred over other valid JSON-LD methods for identifying
  language, such as `@language` properties at the root level of documents.
- Use only the CTDL Context and the CTDL Terms vocabulary. Do not use other
  vocabularies, such as schema.org, to describe your data embedded in the same
  files unless you know what you are doing. The Credential Registry will
  typically ignore data using other vocabularies, but it is possible to
  introduce conflicts that will
- For ease of publishing, sometimes it makes sense to offer both a `@graph`
  style presentation in addition to or instead of individual objects posted at
  their own URLs. The Credential Registry Publish Action will accept both
  styles, and the examples on this website, especially for
  [Learning Programs](LearningProgram/index.md), show how to publish data in
  both styles. However, the risk of accidentally introducing incompatibilities
  between multiple representations of data is higher when using both the
  `@graph` and individual object styles, so it is recommended to publish data at
  their own URLs whenever possible. If publishing your own `@graph` endpoints,
  ensure that object IDs are well-selected and do not introduce URLs for
  individual objects that do not resolve to an up-to-date representation of that
  object.

### Meet the minimum data policy for the objects you are publishing

Credential Engine publishes a
[Minimum Data Policy](https://credreg.net/registry/policy#mindata) outlining
required properties for various data classes as well as a "benchmark" level. It
is required that minimum data be populated and recommended that benchmark
properties also appear. The Credential Registry API will enforce the minimum
data policy, and if some required data is missing, error messages will help
operators identify the missing data. After correcting the data, the Credential
Registry Publish Action can be triggered again to publish the data.

## How to use the Credential Registry Publish Action

It is recommended that you start by testing on the sandbox manually, then move
to automated runs on production.

The Credential Registry Publish Action is designed to be used in GitHub Actions.
To invoke the Action, add a workflow file to a relevant GitHub repository, or
create a new repository in your organization's space for publishing data. The
workflow file should contain environment variables that specify the Registry
environment to publish to, the API key to use, and the CTID to use for the
organization publishing the data. The workflow file should also contain a step
that invokes the Credential Registry Publish Action. The example workflow files
in this repository shows how to do this.

**Example workflow file for publishing data to the sandbox**

For each set of URLs to publish to a particular Registry environment, create a
workflow YAML file in the `.github/workflows` directory of the repository. For
example, to publish the LearningProgram-1 example to the sandbox, create a file
named `publish-learningprogram-1.yml` with the following contents:

```yaml
name: Publish test LearningProgram 1 from examples to the Registry sandbox
on: workflow_dispatch
env:
  REGISTRY_ENV: 'sandbox'
  URLS: 'https://credentialengine.github.io/credential-registry-ingest-examples/LearningProgram/1/LearningProgram-1.json'
jobs:
  publish:
    runs-on: 'ubuntu-latest'
    steps:
      - name: Publish
        uses: credentialengine/credential-registry-publish-action@main
        with:
          registry-env: ${{ env.REGISTRY_ENV }}
          urls: ${{ env.URLS }}
          organization-ctid: 'ce-7f42a13b-2c7e-4cbb-bbae-337a5187cf61'
          registry-api-key: ${{ secrets.SANDBOX_API_KEY }}
```

- The manual trigger `on: workflow_dispatch` means that the workflow will not
  run automatically, but will be triggered manually by a user with access to the
  repository. This is useful for testing the workflow before it is scheduled to
  run automatically.
- The `REGISTRY_ENV` environment variable specifies the Registry environment to
  publish to. In this case, the value is `sandbox`, which means that the data
  will be published to the sandbox registry at
  `https://sandbox.credentialengineregistry.org`. For production data, this
  value would be `production`.
- The `URLS` environment variable specifies the URLs of the data to publish. In
  this case, the value is a single URL, but multiple URLs may be specified,
  separated by commas. The Credential Registry Publish Action will download the
  data from these URLs and publish it to the Registry.
- Within the Publish step, there are several "inputs" configured. The
  `organization-ctid` input specifies the CTID of the organization publishing
  the data. This is required because the organization must be published to the
  Registry before the data can be published. The organization CTID is used to
  identify the organization in the Registry. **Warning**: the identifiers of the
  target organization in the source data will be overwritten with the
  identifiers for this organization on the specified Registry environment. Make
  sure you have targeted the right files, because the Action has no ability to
  check whether you are publishing the data to the correct organizational
  account.
- The organization's API key is used to authenticate the publication request. It
  is not intended to be checked into code but rather defined using the GitHub
  Secrets feature. The example workflow file shows how to reference a secret
  named `SANDBOX_API_KEY` that contains the API key for the sandbox environment.
  The API key for the production environment could be stored in a secret named
  `PRODUCTION_API_KEY`. See information about
  [using secrets in GitHbActions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository).
  Creating a secret at the repository level is likely the best fit for most
  invocations of the Credential Registry Publish Action.

To invoke the workflow using `workflow_dispatch`, navigate to the repository in
GitHub and click the "Actions" tab. Click the "Publish test LearningProgram 1
from examples to the registry" workflow, then click the "Run workflow" button.
The workflow will run and publish the data to the sandbox registry. If there are
errors, they will be reported in the workflow run. If the workflow runs
successfully, the data will be published to the sandbox registry. For more
information, see
[Manually running a workflow](https://docs.github.com/en/actions/managing-workflow-runs/manually-running-a-workflow)

### Migrating to production

To migrate from publishing to the sandbox to publishing to production, change
the `REGISTRY_ENV` environment variable in the above file (the `registry-env`
input to the Publish step) from `sandbox` to `production`. The API key will need
to be updated to the production API key as well, and the organization CTID will
likely need to be updated to the production value. It is best to create a new
workflow file for production, rather than updating the sandbox file, so that the
sandbox file can be retained for testing.

Choose what event should trigger a run of the workflow. It is good to start with
`on: workflow_dispatch` for production, and after confirmation that configured
API key, organization CTID, and URLs are correct, change the trigger to an
appropriate value for your use case, such as `on: push` or `on: schedule`, to
run the workflow automatically.

**Example: Scheduling publication to occur regularly**

To run a workflow automatically on a schedule, you can use a
[cron syntax](https://www.netiq.com/documentation/cloud-manager-2-5/ncm-reference/data/bexyssf.html#beykoa4)
to define a time to run the workflow. For example the following cron syntax will
run the workflow at 8am server time on the first day of each month:
`'0 8 1 * *'`. Note that `*` is a special character in YAML so you have to quote
the cron invocation string.

````yaml

Create the workflow file `.github/workflows/publish-learningprogram-1.yml` with
the following contents:

```yaml
name: Publish test LearningProgram 1 from examples to the Registry production environment
on:
  schedule:
    - cron:  '0 8 1 * *'
env:
  REGISTRY_ENV: 'production'
  URLS: 'https://credentialengine.github.io/credential-registry-ingest-examples/LearningProgram/1/LearningProgram-1.json'
jobs:
  publish:
    runs-on: 'ubuntu-latest'
    steps:
      - name: Publish
        uses: credentialengine/credential-registry-publish-action@main
        with:
          registry-env: ${{ env.REGISTRY_ENV }}
          urls: ${{ env.URLS }}
          organization-ctid: 'ce-7f42a13b-2c7e-4cbb-bbae-337a5187cf61'
          registry-api-key: ${{ secrets.PRODUCTION_API_KEY }}
````
