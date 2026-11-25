# symflip CLI

The command line client for [symflip](https://symflip.com): zero-downtime
deploys to your own Linux servers over SSH, with nothing installed on them.

Use it to deploy from CI or let a coding agent deploy through an API token,
with a human approval in front of production if you want one.

This repository only holds release binaries. There is no source code here.

## Install

Pick a version from [Releases](https://github.com/SarmaleMedia/symflip-cli/releases)
and pin it. Linux amd64:

```sh
VERSION=v0.70.1
bin="symflip-${VERSION}-linux-amd64"
curl -fsSLO "https://symflip.com/download/${VERSION}/${bin}"
curl -fsSLO "https://symflip.com/download/${VERSION}/checksums.txt"
grep " ${bin}\$" checksums.txt | sha256sum -c -
sudo install -m 0755 "${bin}" /usr/local/bin/symflip
symflip version
```

Builds exist for `linux` and `darwin` on `amd64` and `arm64`. On macOS use
`symflip-${VERSION}-darwin-arm64` and `shasum -a 256 -c -`.

`checksums.txt` is signed keylessly with Sigstore. To verify the signature:

```sh
curl -fsSLO "https://symflip.com/download/${VERSION}/checksums.txt.sigstore.json"
cosign verify-blob --bundle checksums.txt.sigstore.json \
  --certificate-identity "https://github.com/teodorsavin/symflip/.github/workflows/release.yml@refs/heads/master" \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  checksums.txt
```

## Use

Create an API token in symflip under Settings, API tokens, and pass it
through the environment:

```sh
export SYMFLIP_TOKEN=sfp_xxxxxxxx
symflip whoami
symflip projects
symflip deploy --project my-app --ref "$(git rev-parse HEAD)" --wait
```

Add `--json` for machine-readable output.

## More

- [How symflip works](https://symflip.com/how-it-works)
- [Security](https://symflip.com/security)
- `symflip --help` lists every command and flag.

## Support

Email [hello@symflip.com](mailto:hello@symflip.com). Report security issues to
[security@symflip.com](mailto:security@symflip.com).

## License

Proprietary. Copyright (c) 2026 Teodor Savin. All rights reserved. The
binaries may be used with the symflip service under its
[Terms of Service](https://symflip.com/terms).
