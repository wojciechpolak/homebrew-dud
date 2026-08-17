# homebrew-dud

A [Homebrew](https://brew.sh) tap for [DUD](https://github.com/wojciechpolak/dud),
a discreet upload / download client for macOS and Linux.

## Install

```sh
brew install wojciechpolak/homebrew-dud/dud
```

That one command taps this repository and installs the client. The name has to
be spelled in full, and the next section is why.

## `brew install dud` installs something else

Homebrew's own package index already has a formula named `dud`: a
[data versioning tool](https://formulae.brew.sh/formula/dud) by a different
author, unrelated to this project in every way but the name. Homebrew resolves a
bare `dud` to that formula, so `brew install dud` gets you a program for
tracking large files in Git repositories rather than this one.

The qualified name `wojciechpolak/homebrew-dud/dud` names this tap's formula
unambiguously, and it keeps working after the tap is added — `brew install dud`
does not become this project once you have tapped. Use the full name every time,
including for `upgrade`, `reinstall`, and `uninstall`.

The two cannot be installed side by side. Both formulae are named `dud` and both
install a `dud` executable into the same Homebrew prefix, so Homebrew refuses to
link the second one. Pick one:

```sh
brew uninstall dud                                # removes whichever is installed
brew install wojciechpolak/homebrew-dud/dud       # then install this one
```

## Upgrade and uninstall

```sh
brew upgrade wojciechpolak/homebrew-dud/dud
brew uninstall wojciechpolak/homebrew-dud/dud
brew untap wojciechpolak/homebrew-dud
```

## What gets installed

The formula builds the client from the source archive of a published release
tag, with the same flags the project's own release binaries are built with: no
cgo, no build paths, no build ID, and the release version compiled in. So

```sh
dud --version
```

prints the version the formula installed.

DUD shells out to a few programs at runtime, and Homebrew installs them as
dependencies rather than leaving you to find them:

| Dependency | Used for                                                     |
| ---------- | ------------------------------------------------------------ |
| `age`      | encrypting and decrypting payloads, and `dud keygen`          |
| `git`      | `dud git *`                                                   |
| `qrencode` | the QR codes printed by `dud upload` and `dud peer invite`    |
| `go`       | building the client; not needed once it is installed          |

Homebrew builds this formula from source. The project also publishes signed,
reproducible binaries and a container image for people who would rather not
build anything; see
[the client documentation](https://github.com/wojciechpolak/dud/blob/master/docs/client.md)
for those, and for how to verify a release.

## Using the client

Start with [the DUD README](https://github.com/wojciechpolak/dud). In short,
there are two ways to move a file, and the client does both:

- a **dead drop** is addressed by an opaque ID you share out of band
- a **peer** transfer is addressed by the local alias of a device you paired
  with beforehand

Both need a backend to talk to — a Cloudflare Worker or a self-hosted server you
run. Installing the client does not give you one.

Issues with the client, the protocol, or the documentation belong on
[the DUD issue tracker](https://github.com/wojciechpolak/dud/issues). Only
problems with packaging belong here.

## About `Formula/dud.rb`

The formula is generated. Publishing a stable release of DUD renders it from
that release's tag and checksum and pushes the result here, so an edit made
directly to this repository is overwritten by the next release. The renderer is
[`scripts/render-homebrew-formula.mjs`](https://github.com/wojciechpolak/dud/blob/master/scripts/render-homebrew-formula.mjs)
in the main repository; change it there.

CI in this repository audits the formula and installs it from source on macOS
and Linux on every push, and checks that the client it produces reports the
version the formula claims.

## License

MIT, matching DUD itself. See [LICENSE](LICENSE).
