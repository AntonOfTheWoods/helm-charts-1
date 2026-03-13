# Releasing

This document is intended for project maintainers.

Charts are packaged and released with [`cr`](https://github.com/helm/chart-releaser) when the `develop` branch is merged into `master`.

## Prerequisites

A `RELEASE_TOKEN` repository secret must be set — a GitHub PAT with `repo` scope. This is required so that the workflow-triggered push to
`master` can fire the [release workflow](https://github.com/vectordotdev/helm-charts/actions/workflows/release.yaml). The default
`GITHUB_TOKEN` cannot trigger other workflows.

## Automated release

1. Go to **Actions → Prepare Release** and click **Run workflow**, or run:
   ```shell
   gh workflow run release-prepare.yml
   ```
2. Review and merge the opened PR into `develop`.
3. The **Post Release** workflow fires automatically on merge and:
  - Merges `develop` into `master` (triggering the chart release CI)
  - Opens a version bump PR for the next development cycle
4. Review and merge the version bump PR.

Once the [release workflow](https://github.com/vectordotdev/helm-charts/actions/workflows/release.yaml) completes, the chart is published.

## Releasing manually

<details>
<summary>Use this only if the automated workflows fail.</summary>

1. Run `.github/release-vector-version.sh` to update the Vector image version, then run `helm-docs`.
  - Commit: `feat(vector): Bump Vector to <version> and update Helm docs`

2. Run `.github/release-changelog.sh` to regenerate the CHANGELOG.
  - Commit: `feat(vector): Regenerate CHANGELOG for <version>`

3. Submit both commits as a single PR to `develop` and merge it.

4. Merge `develop` into `master` to trigger the release workflow:
   ```shell
   git switch master && git pull
   git merge develop
   git push
   ```

5. Bump the chart minor version in `charts/vector/Chart.yaml`, run `helm-docs`, and open a PR to `develop`.

</details>
