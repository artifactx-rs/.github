<div align="center">

<img src="https://raw.githubusercontent.com/artifactx-rs/.github/main/profile/logo.png" alt="ArtifactX" width="300">

# Build Once · Package Once · Publish Everywhere

**Your own signed apt + yum repository in under five minutes — from one static binary.**

No database. No JVM. No web console. No Ruby. No `dpkg-deb` or `rpmbuild`.
Just a single pure-Rust binary that **packages** your software and **publishes** it
to a repository `apt-get` and `dnf` install from directly.

</div>

---

## The 5-minute story

```bash
arx init ./repo                     # scaffold + generate a signing key
arx pack ./myapp.toml               # one manifest → .deb + .rpm (no toolchain)
arx add ./dist/*.deb ./dist/*.rpm   # drop into the pool
arx publish                         # generate + PGP-sign apt & yum metadata (atomic)
arx serve                           # ready for apt-get / dnf
```

Need to pull a bad release? `arx rm myapp --version 1.2.3`. Reclaim disk?
`arx gc --keep 5`. That's the whole product.

## Why ArtifactX?

The package-repo world splits into two camps: heavyweight **platforms**
(Nexus, JFrog, Pulp — databases, JVMs, RBAC, web UIs) and someone-else's-**SaaS**
(Cloudsmith, Gemfury, packagecloud — their servers, their bill). ArtifactX takes the
unoccupied middle:

> **aptly's safety** (every version kept, atomic publish, one-command rollback) ·
> **nfpm's manifest** (one file → many native formats) ·
> **Cloudsmith's push DX** (one-line CI, keyless OIDC auth) —
> in **one self-hosted static binary**, with none of the platform.

| Instead of… | …you get |
| --- | --- |
| **Nexus / Artifactory** (JVM + DB + license) | one static binary, running in 5 minutes |
| **Pulp** (Postgres + Django + workers) | immutable, atomically-published repo states — minus the cluster |
| **Cloudsmith / Gemfury** (their servers, their bill) | the same one-line push + OIDC, on infra **you** own |
| **aptly** (5-step toolkit, DB to clean, slow republish) | `push` · `rm` · `rollback` — same safety, none of the bookkeeping |
| **reprepro** (forgets your old versions) | every version kept; roll back in one command |
| **nfpm / FPM** (package, then stop at a file) | package **and publish**, end-to-end, deterministic, zero host tools |

## What it does

- 📦 **apt + yum from one tool** — Debian `Packages`/`Release` + RPM `repodata`, PGP-signed, verified against real `apt-get` and `dnf`.
- 🏗️ **Package without a toolchain** — `pack`: one manifest → native `.deb`/`.rpm` in pure Rust (no `dpkg-deb`, no `rpmbuild`).
- 🔏 **Signed & atomic** — `InRelease`/`Release.gpg`/`repomd.xml.asc`; `by-hash` + atomic swap so clients never see a torn publish.
- 🌐 **Serve built-in** — one HTTP command; drop it behind nginx/Caddy for production.
- 🦀 **One static binary** — drops into a `scratch` container; nothing to operate.

## Heritage

We didn't invent the [pool layout, suite-as-index] (Debian's **dak**), the
[one-manifest-many-formats] idea (**FPM**, **EPM**, **nfpm**), or the
[no-database 5-minute repo] (**mini-dinstall**, **freight**). We put all three in a
single Rust binary, deleted the database and the Ruby, and added the **publish** step
none of them had.

---

<div align="center">
<sub><b>ArtifactX</b> — remove the friction from software distribution.</sub><br>
<sub>Open-source · pure-Rust · self-hosted · alpha</sub>
</div>
