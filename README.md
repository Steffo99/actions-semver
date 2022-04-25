# `steffo99/actions-semver`

Github Action to parse GitHub Releases tag names into semantic versions

## Captures

This action, when triggered on a `release` event, will capture the name of the release tag (for example, `v1.2.3-final`, located at `github.event.release.tag_name`).

## Inputs

This action has no named inputs.

## Outputs

This action has five named outputs:

- `tag`: the complete tag of the release, including the v, such as `v1.2.3-final`.
- `full`: the full tag of the release, excluding the v, such as `1.2.3-final`.
- `patch`: the patch version of the release, excluding the extra metadata, such as `1.2.3`.
- `minor`: the minor version of the release, excluding the patch number, such as `1.2`.
- `major`: the major version of the release, excluding the minor number, such as `1`.

## Example usage

In a task building and publishing a Docker image on every published release:

```yaml
# ...

steps:
  - name: "❓ Find the release semantic version"
    id: semver
    uses: Steffo99/actions-semver@v0.1.1

  - name: "🔨 Setup Buildx"
    uses: docker/setup-buildx-action@v1

  - name: "🔑 Login to GitHub Containers"
    uses: docker/login-action@v1
    with:
      registry: ghcr.io
      username: RYGhub
      password: ${{ secrets.GITHUB_TOKEN }}

  - name: "🏗 Build and push the Docker image"
    uses: docker/build-push-action@v2
    with:
      tags: >-
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.full }},
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.patch }},
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.minor }},
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.major }},
        ghcr.io/ryghub/impressive-strawberry:latest
      push: true
    
# ...
```
