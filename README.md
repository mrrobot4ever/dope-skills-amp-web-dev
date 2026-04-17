# Dope Skills - AMP Web Dev

An OpenClaw agent skill for building valid Google AMP pages with modern, premium design aesthetics.

## What It Does

Guides an AI agent through building production-ready AMP pages with:

- Valid AMP boilerplate (exact official spec)
- Frosted glass cards, neon glow effects, gradient text
- Shimmer animations on buttons (pure CSS)
- Responsive two-column/one-column layouts
- FAQ accordions with animated carets
- GitHub Pages deployment with custom domains

## Why AMP

AMP pages load instantly, are eligible for Google's AMP cache, and rank well in mobile search. The constraints (no JS, 75KB CSS limit) force clean, performant code.

## Usage with OpenClaw

```bash
cd ~/.openclaw/workspace/skills
git clone https://github.com/mrrobot4ever/dope-skills-amp-web-dev.git
```

Then tell your agent: *"Build me an AMP landing page"* and describe what you want.

## Documentation

| File | Purpose |
|------|---------|
| [SKILL.md](SKILL.md) | Complete AMP development guide -- boilerplate, CSS rules, components, design patterns, deployment |
| [references/lessons-learned.md](references/lessons-learned.md) | Validation pitfalls and fixes from production |

## License

MIT
