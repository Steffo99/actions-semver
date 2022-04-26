# `Steffo99/actions-semver`

Github Action to parse strings into semantic version numbers

## Inputs

This action has a single named input:

- `string`: the string to split into semantic version numbers.

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
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.full }},
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.patch }},
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.minor }},
        ghcr.io/ryghub/impressive-strawberry:${{ steps.semver.outputs.major }},
        ghcr.io/ryghub/impressive-strawberry:latest
      push: true
    
# ...
```
