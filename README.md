English | [한국어](README.ko.md) | [中文](README.zh.md) | [日本語](README.ja.md) | [Español](README.es.md)

# godmode

<div align="center">
  <img src="assets/godmode-hero-01.png" width="860" alt="Mythic key-art: a lightning bolt strikes a Greek marble temple while a plasma loop orbits its columns; a lone developer with a laptop watches from the stairs below the carved GODMODE wordmark and a phosphor-green '> iddqd' cheat code">
</div>

```
> iddqd

GOD MODE ON.
Degreelessness activated.
Your idea can no longer die before it ships.
```

> **The cheat code for finishing things.** Turn it on, drop an idea, and the loop refuses to stop until the documents themselves say it's done.

Every project dies the same way: 80% finished, 0% shipped. godmode removes the option of dying. It chains interview → PRD → goal setup → `/goal` execution, then keeps hammering **verify → review → improve** cycles until the Definition of Done is met — **every VALIDATION item passes AND zero blocking assumptions remain**. Stalls trigger a recovery ladder that changes the approach instead of giving up. Early exits get caught by the driven guard and thrown back into the loop.

You are not asked whether to continue. That's the whole point.

## Quick Start

### 1. Add the marketplace

```
/plugin marketplace add fivetaku/godmode
```

### 2. Install

```
/plugin install godmode@godmode
```

### 3. Enter the cheat code

```
/godmode build me a pomodoro timer that shames me when I quit early
```

That's it. Come back when it's done. (It will tell you when it's done. The documents decide, not vibes.)

## The rules of god mode

1. **Done is defined, not felt.** DoD = all VALIDATION items pass + 0 blocking assumptions. Written in documents, checked by the loop.
2. **It never stops early.** Stall → retry → change approach with a deeper reviewer → re-plan gate. Giving up is not on the ladder.
3. **Micro-loops beat one-shot perfection.** Each stage passes at "good enough to start," then the loop tightens quality relentlessly.
4. **Reviewers are pluggable.** self-review (default) / insane-review / codex / gjc — pick your judge.

## Requirements

- [Claude Code](https://claude.com/claude-code)
- Best with the [gptaku-plugins](https://github.com/fivetaku/gptaku_plugins) pipeline installed (`show-me-the-prd`, `goaljaby`) — godmode orchestrates them when present.

## Lineage

godmode is the meme edition of [insane-loop](https://github.com/fivetaku/insane-loop) — same engine, more attitude.

## License

MIT — see [LICENSE](LICENSE) and [DISCLAIMER](DISCLAIMER.md). God mode is a metaphor. You remain mortal and responsible for what you ship.
