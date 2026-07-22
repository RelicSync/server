<!-- This file becomes the root README.md of the public repo (e.g. RelicSync/server).
     It is staged here in the private monorepo; publish/export.sh assembles the
     public tree. Keep it presenting as "Relic". -->

# Relic

**Copy it once. Keep it forever.** 

https://github.com/user-attachments/assets/651992b9-85b5-4618-a84b-252bd7492ce6

Relic is an end-to-end-encrypted clipboard
manager that keeps a permanent, searchable history of everything you copy and
syncs it across your devices. Only you can read it: everything is encrypted on
your device with XChaCha20-Poly1305 and a key derived from your passphrase with
Argon2id, and the server only ever stores ciphertext.

This repository is Relic's **sync server**. It powers both Relic Cloud (the
managed, hosted option) and the self-host build you can run yourself. The two
share the exact same code, so a self-hosted vault behaves identically to a hosted
one. The Relic desktop and mobile apps are distributed as signed binaries from
[relic.space](https://relic.space).

> Platform support today: **Windows and Android are live.** Mac, iPhone, and
> Linux are coming soon.

## Run your own server

You do not need us. Relic is local-first, so every device already holds your full
vault; a sync server is just an encrypted pipe between your devices. Host that
pipe yourself in one command (prebuilt multi-arch image, x86 and ARM):

```sh
docker run -d -p 8787:8787 -v relic-data:/data ghcr.io/relicsync/relic-selfhost
```

Then in the app, point it at your server and enter a passphrase. There are no
accounts and no configuration. Full guide: [`selfhost/README.md`](./selfhost/README.md).

Because it is local-first and end-to-end encrypted, you are never locked in. You
can move between your own server and Relic Cloud whenever you like, and your data
is never held hostage by either.

## Or let us host it

If you would rather not run a server, [Relic Cloud](https://relic.space) hosts it
for you with the same zero-knowledge encryption. Free for unlimited text; paid
tiers lift the storage and attachment limits. Self-hosting stays free forever.

## Why zero-knowledge matters here

Your clipboard quietly accumulates the most sensitive things you handle:
passwords, 2FA codes, API keys, private messages. Most clipboard tools store that
in plain text. Relic encrypts every item on your device before it ever leaves,
and the server cannot decrypt any of it. This repository lets you verify the
server half of that claim: it stores only ciphertext plus a content-free metadata
index. The encryption itself happens in the client, and that code is published
separately at [RelicSync/relic-crypto](https://github.com/RelicSync/relic-crypto),
where a `dart test` runs it against pinned vectors.

## What's in this repo

```
worker/      The server. A small, dependency-light request handler (the sync data
             plane, device registry, sharing, and billing for the hosted option).
             Deploys to Cloudflare Workers for Relic Cloud.
selfhost/    The self-host build: the same worker run on plain Node, backed by
             local SQLite and local-disk storage instead of Cloudflare, plus a
             one-command Docker image.
```

The server is deliberately thin and stores only ciphertext plus a plaintext-free
metadata index (ids and timestamps, never content).

## License

[GNU AGPL-3.0](./LICENSE). You are free to run, study, modify, and self-host
Relic. The AGPL's network-copyleft means anyone who offers a modified version of
this server as a service must share their changes under the same license. This is
the same license Bitwarden and Standard Notes use, for the same reason: it keeps
the project genuinely open while keeping the hosted business sustainable.
