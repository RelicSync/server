# Moved: Relic is now fully open source

This repository held Relic's sync server while the rest of the product was
closed. That split is over. **The entire Relic monorepo is now public**, and it
is the one place where development happens:

## → [github.com/RelicSync/relic](https://github.com/RelicSync/relic)

Everything that lived here (the worker, the self-host build, the Docker image
workflow) lives there now, alongside the desktop and mobile app, the on-device
AI, the verifiable client crypto, and the agent-facing CLI.

- **Self-hosting:** see [`selfhost/`](https://github.com/RelicSync/relic/tree/main/selfhost)
  in the monorepo. The Docker image remains `ghcr.io/relicsync/relic-selfhost`.
- **Issues and PRs:** open them on the monorepo.

This repository is archived and will not receive further updates.
