# korri-os/nix-cache

A signed Nix binary cache for Korri's own build outputs, served from GitHub
release assets. It is a substituter, not an archive: Nix reads
`nix-cache-info` and `<hash>.narinfo` from the metadata release and fetches
NAR payloads from batch releases.

Nothing that resolves from `cache.nixos.org` is stored here. The publisher
omits any path a configured upstream cache already serves, so this repository
carries Korri binaries and nothing else.

## Consuming

```
substituters        = https://cache.nixos.org/ https://github.com/korri-os/nix-cache/releases/download/cache/
trusted-public-keys = cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY= korri-cache-fuji-1:... korri-cache-zao-1:...
```

Keep `require-sigs = true`. Each builder signs with its own key, so one
machine can be revoked without reissuing the others.

## Publishing

Producers run the publisher in korri-os/korri. Builders never push by hand;
the Nix `post-build-hook` uploads what a machine builds.
