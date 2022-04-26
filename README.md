# `Steffo99/actions-semver`

Github Action to parse strings into semantic version numbers

## Inputs

This action has a single named input:

- `string`: the string to split into semantic version numbers.

## Outputs

This action has nine named outputs:

- `full`: The full semantic version (`1.2.3-beta+ABCDEF`).
- `precedence`: The semantic version with metadata excluded (`1.2.3-beta`).
- `core`: The semantic version core (`1.2.3`).
- `pair`: The semantic version core, excluding the patch version (`1.2`).
- `major`: The major version (`1`).
- `minor`: The minor version (`2`).
- `patch`: The patch version (`3`).
- `prerelease`: The pre-release field contents (`beta`).
- `metadata`: The metadata field contents (`ABCDEF`).

## Example usage

In a task building and publishing a Docker image on every published release:

```yaml
# ...

steps:
  - name: "❓ Find the release semantic version"
    id: semver
    uses: Steffo99/actions-semver@v0.2.0
    with:
      string: ${{ github.event.release.tag_name }}

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
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.core }},
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.pair }},
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.major }},
        ghcr.io/ryghub/impressive-strawberry:latest
      push: true

# ...
```
