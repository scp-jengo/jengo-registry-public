# jengo-registry-public

Public API registry and capability discovery endpoints.

## What's Here

| Path | Purpose |
|------|---------|
| `apis/*.yaml` | Public API definitions and capability discovery specs |

## Architecture

This repo is part of the **Jengo 3-Tier Modular System**:
- **Layer:** registry (operational state — services, deployments, tracking)
- **Tier:** public (safe to share, publish, or include in open-source)

Public-facing API registry that allows external systems to discover Jengo's capabilities. Internal deployment details and infrastructure state remain in `jengo-registry-private`.
