# iPXE Templates

Collection of simple, single-purpose iPXE scripts that boot popular distributions directly from upstream mirrors. Drop the desired file on your iPXE server (or reference it via `chain`) to present an installer for the matching OS release.

All templates assume DHCP on the primary NIC and plain-HTTP access (since some firmware builds lack HTTPS). Swap the mirror URLs for your local cache or HTTPS endpoints if your environment supports them.

## Available profiles

| File | What it boots | Notes |
| --- | --- | --- |
| `profiles/netboot.ipxe` | `netboot.xyz` | Chains to the community menu. |
| `profiles/ubuntu-24.04.ipxe` | Ubuntu Server 24.04 LTS | Subiquity live-server installer. |
| `profiles/almalinux-10.ipxe` | AlmaLinux 10 | BaseOS tree installer. |
| `profiles/debian-13.ipxe` | Debian 13 (trixie/testing) | Netboot installer. |
| `profiles/alpine-3.20.ipxe` | Alpine Linux 3.20 | LTS netboot image + repo hint. |

> Tip: When automating installs (Kickstart, Preseed, Autoinstall, etc.) append the relevant kernel parameters directly within each `kernel` line so they are version-controlled alongside the template.

See [GitHub Releases](https://github.com/cherryservers/ipxe-templates/releases) for signed, versioned, and immutable artifacts.

## Release verification
Use [Cosign](https://github.com/sigstore/cosign) to verify release artifacts,

```bash
OWNER="cherryservers"
REPO="ipxe-templates"
TAG="<tag_to_verify>"

cosign verify-blob "${REPO}-${TAG}.sha256sums" \
  --bundle "${REPO}-${TAG}.sha256sums.sigstore.json" \
  --certificate-identity "https://github.com/${OWNER}/${REPO}/.github/workflows/release.yaml@refs/tags/${TAG}" \
  --certificate-oidc-issuer "https://token.actions.githubusercontent.com"
```

Or alternatively with [GitHub CLI](https://cli.github.com/),

```bash
OWNER="cherryservers"
REPO="ipxe-templates"

gh attestation verify /path/to/release/artifact --repo "${OWNER}/${REPO}"
```
