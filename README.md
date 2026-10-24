<div align="center">

<img src="https://raw.githubusercontent.com/NetrunnerGames/.github/main/profile/logo.png" width="130" height="130" alt="NetrunnerGames Logo" />

# NetrunnerGames OTA

Centralized Over-The-Air pattern feeds and dynamic runtime configuration for NetrunnerGames components.

<a href="https://github.com/NetrunnerGames/OTA"><img src="https://img.shields.io/badge/OTA_Feed-Live-090a0f?style=for-the-badge&labelColor=090a0f&logo=github&logoColor=00ffff" height="42" alt="OTA Feed Status" /></a>
<a href="https://github.com/NetrunnerGames/IceBreaker"><img src="https://img.shields.io/badge/IceBreaker-Integrated-090a0f?style=for-the-badge&labelColor=090a0f&logo=c%2B%2B&logoColor=00f5ff" height="42" alt="IceBreaker Integration" /></a>
<a href="https://discord.gg/SadzcAwdjW"><img src="https://img.shields.io/badge/Discord-Join_Server-090a0f?style=for-the-badge&labelColor=090a0f&logo=discord&logoColor=5865F2" height="42" alt="Discord" /></a>
<a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-090a0f?style=for-the-badge&labelColor=090a0f&logo=open-source-initiative&logoColor=3da639" height="42" alt="License" /></a>

</div>

> [!IMPORTANT]
> **DISCLAIMER & NOTICE**
> NetrunnerGames OTA is an independent open-source configuration feed. It is not affiliated with, endorsed by, or associated with Valve Corporation or Steam. All trademarks belong to their respective owners.

---

## Overview

The **OTA (Over-The-Air)** repository serves as the official live upstream feed for binary pattern signatures, interface offsets, and dynamic configuration used across the NetrunnerGames ecosystem (including **IceBreaker**, **DataJackUI**, and **Jack-in**).

When client updates modify module signatures or rearrange compiler registers, updated AOB patterns are published directly to this repository. Connected clients automatically fetch the latest definitions on demand—bypassing the need for manual binary re-releases.

---

## Available Feeds

| Feed File | Target Component | Description | Raw Endpoint |
|---|---|---|---|
| [`icebreaker_patterns.json`](icebreaker_patterns.json) | **IceBreaker** (`version.dll`) | AOB byte signatures and wildcarded fallbacks for `steamclient64.dll` and `steamui.dll`. | [View Raw](https://raw.githubusercontent.com/NetrunnerGames/OTA/main/icebreaker_patterns.json) |

---

## Feed Schema & Architecture

Feeds use versioned, lightweight JSON schemas optimized for direct in-memory deserialization by native modules:

```json
{
  "version": 1,
  "updated_at": "2026-10-24T06:50:00Z",
  "patterns": {
    "steamclient64": {
      "CheckAppOwnership": [
        "48 8B C4 89 50 10 48 89 48 08 55 53",
        "48 8B C4 89 50 ?? 48 89 48 ?? 55 53"
      ],
      "ConfigStoreGetBinary": [
        "40 53 55 56 57 48 83 EC 38 48 63 FA 49 8B E9",
        "40 53 55 56 57 48 83 EC ?? 48 63 FA 49 8B E9"
      ]
    }
  }
}
```

### Key Highlights
* **Multi-Pattern Fallbacks**: Each target function provides candidate arrays sorted in order of preference.
* **Wildcarding (`??`)**: Softens compiler-dependent stack offsets and register choices without compromising match specificity.
* **Offline Caching**: Clients mirror successful downloads locally (e.g. `config/icebreaker_patterns_cache.json`) to guarantee offline functionality.

---

## Contributing & Updates

To submit pattern updates following a Steam client update:
1. Verify the signature against the latest target module using x64dbg, IDA, or Ghidra.
2. Ensure compiler-volatile offsets are appropriately masked with `??`.
3. Open a Pull Request against `main`. Changes become active immediately upon merge.

---

## License

This repository is licensed under the [MIT License](LICENSE).
