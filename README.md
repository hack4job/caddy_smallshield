# caddy_smallshield (fork)

A drop-in replacement for the original
[`proofrock/caddy_smallshield`](https://github.com/proofrock/caddy_smallshield)
Caddy HTTP handler that adds **live blacklist reloading** while remaining 100 % backward-compatible with
the upstream module.

---

## ✨ What’s new in this fork

| Feature | Upstream | **This fork** |
|---------|----------|---------------|
| `refresh` directive – re-pull the blacklist on a configurable interval | ✗ | ✓ |
| Conditional GET (ETag / Last-Modified) – zero bandwidth & CPU if the file is unchanged | ✗ | ✓ |
| Hash-safe bulk swap of the CIDR tree (no per-request lock contention) | ✗ | ✓ |
| Generic constructor `iptree.New(io.Reader, threadSafe)` added to the internal IPTree package | ✗ | ✓ |

---

## Building

### 1. With **xcaddy**

```bash
xcaddy build \
  --with github.com/proofrock/caddy_smallshield=github.com/hack4job/caddy_smallshield@refresh-loop
```

## Usage

```json
{
    order caddy_smallshield first
}

:443 {
    caddy_smallshield {
        blacklist_url "https://raw.githubusercontent.com/ktsaou/blocklist-ipsets/master/firehol_level1.netset"
        refresh       "30s"                    
        log_blockings "1"
    }

    reverse_proxy http://app:8080
}
```