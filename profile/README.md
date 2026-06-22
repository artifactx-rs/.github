<div align="center">

<img src="https://raw.githubusercontent.com/artifactx-rs/artifactx/main/res/org-logo.png" alt="ArtifactX" width="180">

# ArtifactX — import existing apt/yum repos into a signed static repo

**Import first. Cut over when ready.**

Pull packages from the repos you already have, regenerate apt/yum metadata under
your key, and serve the result from one static Rust binary.

[![CI](https://github.com/artifactx-rs/artifactx/actions/workflows/ci.yml/badge.svg)](https://github.com/artifactx-rs/artifactx/actions/workflows/ci.yml)
[![Release](https://github.com/artifactx-rs/artifactx/actions/workflows/release.yml/badge.svg)](https://github.com/artifactx-rs/artifactx/actions/workflows/release.yml)
[![Latest release](https://img.shields.io/github/v/release/artifactx-rs/artifactx)](https://github.com/artifactx-rs/artifactx/releases/latest)
[![crates.io](https://img.shields.io/crates/v/artifactx.svg)](https://crates.io/crates/artifactx)

</div>

---

## The short version

ArtifactX (`arx`) is for teams that ship Linux packages but do not want to run
Nexus, aptly, Pulp, S3 glue scripts, custom signing jobs, and a web server just
so users can run `apt install` or `dnf install`.

```bash
# Path 1: migrate a slice of an existing repo, then serve it
arx init ./repo
arx import https://packages.example.com --apt --dist stable --component main --match-name myapp
arx publish --root ./repo
arx serve --root ./repo
```

```bash
# Path 2: start a new repo from packages you already built
arx init ./repo
arx add dist/*.deb dist/*.rpm --root ./repo
arx publish --root ./repo
arx serve --root ./repo
```

Users get the boring install path they already know:

```bash
sudo apt-get update && sudo apt-get install myapp
# or
sudo dnf install myapp
```

## How it works

<p align="center">
  <img src="https://raw.githubusercontent.com/artifactx-rs/artifactx/main/res/readme-architecture.svg" alt="ArtifactX import, sign, publish, and static hosting architecture">
</p>

ArtifactX keeps the repository as inspectable static files: import or add
packages, sign regenerated metadata, publish atomically, and serve from `arx
serve`, GitHub Pages, nginx, or object storage.

## Why ArtifactX

- **Import first** — pull packages from existing apt or yum/dnf repositories into
  your own signed repo.
- **One binary** — pack, add, import, publish, serve, push, promote, GC, rollback.
- **Signed repository metadata** — apt `InRelease` / `Release.gpg`, yum
  `repomd.xml.asc`. Package signing stays in your build pipeline.
- **Atomic publish + rollback** — build metadata in staging, flip the live state,
  and roll back when a bad release escapes.
- **CI-friendly push** — upload to `arx serve` with a token or GitHub OIDC.
- **No daemon required** — static binary, Docker image, or GitHub Pages-hosted repo.

## What is shipped

| Pillar | Status | Highlights |
| --- | --- | --- |
| Repository | ✅ Shipped | apt + yum/dnf metadata, signing, import, publish, rollback, GC, promote, watch, HTTP API. |
| Package | ✅ Shipped | Pure-Rust `.deb`, `.rpm`, `.apk`; Cargo.toml-driven pack; Docker backend. |
| Operations | 🟢 Polishing | Import-first docs, trust path, Pages dogfood, systemd/Docker guidance. |

## Current roadmap

- 🟢 **Now:** [`v0.1.x — Import-first polish`](https://github.com/artifactx-rs/artifactx/milestone/1)
- 🔵 **Next:** [`v0.2.0 — Packaging ergonomics`](https://github.com/artifactx-rs/artifactx/milestone/2)
- 📋 **Project board:** <https://github.com/orgs/artifactx-rs/projects/1>
- 🧭 **Roadmap:** <https://github.com/artifactx-rs/artifactx/blob/main/ROADMAP.md>

## Repositories

- [`artifactx`](https://github.com/artifactx-rs/artifactx) — the `arx` CLI,
  packager, repository generator, server, and documentation.

---

<div align="center">
<sub><b>ArtifactX</b> — import first, publish safely, cut over when ready.</sub><br>
<sub>Open-source · Rust · self-hosted · alpha</sub>
</div>
