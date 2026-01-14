# Helse-Arbeidsgiver workflows

En samling av gjenbrukbare workflows for [GitHub Actions](https://github.com/features/actions).

## Komme i gang

[Dokumentasjonen til GitHub](https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows) forklarer hvordan reusable workflows fungerer.

Eksemplene under beskriver hvordan de ulike workflowene kan tas i bruk. Triggerne under `on` er veiledende.

### build-and-deploy

```yml
name: Build and deploy

on:
  # for prod, bruk denne
  release:
    types: [released]
  # for dev, bruk denne
  push:
    branches:
      - main
      - dev/**

jobs:
  build-and-deploy:
    uses: navikt/hag-workflows/.github/workflows/build-and-deploy.yml@main
    permissions:
      id-token: write
    with:
      environment: <dev|prod>
      nais-manifest: <path-to-nais-manifest.yml>
      nais-manifest-vars: <path-to-nais-manifest-vars.yml> # optional
      project-id: ${{ vars.NAIS_MANAGEMENT_PROJECT_ID }}
    secrets:
      identity-provider: ${{ secrets.NAIS_WORKLOAD_IDENTITY_PROVIDER }}
```

### build-and-publish

```yml
name: Build and publish

on:
  push:
    branches:
      - '**'

jobs:
  build-and-publish:
    uses: navikt/hag-workflows/.github/workflows/build-and-publish.yml@main
    permissions:
      packages: write
```

### dependency-submission

```yml
name: Dependency submission

on:
  schedule:
    # Hver onsdag kl. 04:00
    - cron: '0 4 * * 3'
  push:
    branches:
      - main
    paths:
      - gradle.properties
      - build.gradle.kts
      - settings.gradle.kts

jobs:
  dependency-submission:
    uses: navikt/hag-workflows/.github/workflows/dependency-submission.yml@main
    permissions:
      contents: write
```

### ktlint

```yml
name: Ktlint

on: [pull_request]

jobs:
  ktlint:
    uses: navikt/hag-workflows/.github/workflows/ktlint.yml@main
    with:
      ktlint-version: <ktlint-version>
```

---

## Henvendelser

Spørsmål knyttet til koden eller repositoryet kan stilles som [issues](https://github.com/navikt/hag-workflows/issues/new) her på GitHub.

### For Nav-ansatte

Interne henvendelser kan sendes via Slack i kanalen [#helse-arbeidsgiver](https://nav-it.slack.com/archives/CSMN6320N).
