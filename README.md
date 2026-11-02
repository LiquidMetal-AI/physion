# Physion

Physion is a programming language and a column database in one binary.
The same process stores tables, runs queries, and serves other processes over the network.
An operation applies to a whole vector at once, so a program works on columns of data, not on one item at a time.
Physion also ships GPU Monitor, which collects and stores the telemetry of GPUs and other accelerators.

This repository holds the Physion releases.
The documentation is at <https://docs.liquidmetal.ai/physion/>.

## Download

Get the files from the [releases page](https://github.com/LiquidMetal-AI/physion/releases).
Each release has these files:

| File | Contents |
|---|---|
| `physion-VERSION-linux-x86_64.tar.gz` | Linux on x86_64, glibc 2.28 or later |
| `physion-VERSION-linux-aarch64.tar.gz` | Linux on aarch64 (arm64), glibc 2.28 or later |
| `physion-VERSION-darwin-arm64.tar.gz` | macOS: Apple Silicon (arm64), macOS 15 or later |
| `physion-min-VERSION-PLATFORM.tar.gz` | The small binary and the standard library only |
| `physion-VERSION.spdx.json` | The SBOM (SPDX 2.3) of every tarball |
| `SHA256SUMS` | The SHA-256 of every file |
| `physion-VERSION.provenance.sigstore.jsonl` | The build provenance attestations |
| `physion-VERSION.sbom.sigstore.jsonl` | The SBOM attestations |

Unpack a tarball in any directory and put its `bin/` on your `PATH`:

```sh
V=1.2.3
tar -xzf physion-$V-linux-x86_64.tar.gz
export PATH="$PWD/physion-$V-linux-x86_64/bin:$PATH"
physion --version
```

## Container images and the Helm chart

```sh
docker pull ghcr.io/liquidmetal-ai/physion:1.2.3
docker pull ghcr.io/liquidmetal-ai/gpumon:1.2.3
helm install gpumon oci://ghcr.io/liquidmetal-ai/charts/gpumon --version 1.2.3
```

Each image holds the unpacked Linux tarball for `amd64` and `arm64`.
The chart runs GPU Monitor from the `gpumon` image.

## Verify a download

Check each file against `SHA256SUMS`:

```sh
sha256sum --check --ignore-missing SHA256SUMS          # Linux
shasum -a 256 --check --ignore-missing SHA256SUMS      # macOS
```

Each file, image and chart also has a signed attestation.
The attestation records that the file was published by the workflow `.github/workflows/publish.yml` of this repository.
Check it with the GitHub CLI (`gh`, version 2.49 or later):

```sh
gh attestation verify physion-1.2.3-linux-x86_64.tar.gz --repo LiquidMetal-AI/physion
gh attestation verify oci://ghcr.io/liquidmetal-ai/physion:1.2.3 --repo LiquidMetal-AI/physion
gh attestation verify oci://ghcr.io/liquidmetal-ai/gpumon:1.2.3 --repo LiquidMetal-AI/physion
gh attestation verify oci://ghcr.io/liquidmetal-ai/charts/gpumon:1.2.3 --repo LiquidMetal-AI/physion
```

Without a network connection, use the bundle from the release:

```sh
gh attestation verify physion-1.2.3-linux-x86_64.tar.gz --repo LiquidMetal-AI/physion \
  --bundle physion-1.2.3.provenance.sigstore.jsonl
```

To check the SBOM attestation, add `--predicate-type https://spdx.dev/Document/v2.3`.

The command prints `Verification succeeded!` for a good file.
For any other result, do not use the file, and tell us at support@liquidmetal.ai.

The macOS binaries also carry a Developer ID signature of LiquidMetal AI, Inc., and Apple notarizes them.
`codesign --verify --strict --verbose=2 bin/physion` checks the signature.

## Free tier

Physion is free to use on up to 72 accelerators (GPUs or other accelerators), for example one NVL72 rack, with no licence key.
For more accelerators, ask for a licence key at support@liquidmetal.ai.
The licence check is advisory: it never stops physion, disables a feature or slows anything down.
`physion licence status` shows the status.

The [LICENSE](LICENSE) file gives the terms. [NOTICE](NOTICE) lists the third-party materials.

## Support

Write to support@liquidmetal.ai.
