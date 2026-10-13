# Softspoken releases

Installers and update manifests for [Softspoken](https://softspoken.io), a private meeting
notetaker and dictation app for macOS and Windows that transcribes on your own computer.

Softspoken is in closed beta. The waitlist is at [softspoken.io](https://softspoken.io).

The application source lives in a private repository. This one holds only what has to be
publicly reachable: the things you download.

## What is here

- **Releases** carry the installers. Each version's release holds the macOS DMG, the Windows
  installer, and the update payloads the in-app updater downloads.
- **`latest-darwin.json`** and **`latest-windows.json`** are the update manifests. The app
  fetches its own platform's file, compares versions, and does nothing if it is already
  current. They are served from this repository at `https://updates.softspoken.io/`.
- **`preview-darwin.json`** and **`preview-windows.json`** serve people who have switched on
  Early bird updates in the app. They carry builds ahead of a release, verified in the same way.

## Why the installers are public

Every desktop application's downloads are public, and a licence has never been about who can
download a file. It is about what you are entitled to run. Keeping the installers reachable
costs nothing and makes the alternative unnecessary: because there is nothing to gate, the
in-app updater needs no account, no token and no credentials. It fetches a small JSON file
and a public payload, and sends no identity of any kind.

## How an update is verified

Every payload is signed with [minisign](https://jedisct1.github.io/minisign/), and each copy
of Softspoken carries the matching public key compiled into its own binary. A payload whose
signature does not verify is refused before anything is installed.

TLS protects the transport. The signature protects the contents. That separation is the point:
on a corporate network that inspects HTTPS traffic through its own certificate authority, the
download still works and a substituted build is still refused.

## Who signed the installers

Windows installers, the app and every first-party executable inside it are Authenticode-signed
by River Edge Ventures Pty Ltd, the company behind Softspoken. macOS builds are signed with the
same company's Apple Developer ID and notarised by Apple.

## Publishing, for maintainers

Payloads are uploaded to a release **first**, and the manifest is committed **second**. In that
order a manifest can never advertise an asset that is not there yet, which makes the commit the
moment a version goes live, and reverting it the way a bad release is withdrawn.

macOS publishes from a maintainer's own machine, because the code-signing identity exists only
there. Windows publishes from CI. The two write different files, so neither waits on the other
and neither can overwrite the other's work.

Full instructions live in `packaging/README.md` in the application repository.

## Placeholder manifests

Until the first release lands, both manifests declare version `0.0.0`, which every installed
copy already exceeds. The result is a truthful "you are up to date" rather than a failed fetch.
