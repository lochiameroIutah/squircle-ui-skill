# squircle-ui

A Claude Code skill that makes [`@squircle-js/react`](https://squircle.js.org) the default for rounded UI in React, replacing `border-radius` and Tailwind's `rounded-*`.

## Install

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/lochiameroIutah/squircle-ui-skill.git squircle-ui
```

Claude Code will auto-discover the skill from that point on.

You can also try the community `skills` CLI (not officially tested with this repo):

```bash
npx skills add lochiameroIutah/squircle-ui-skill
```

## Why this exists

I believe squircles could harmonize digital life with the real world a little more.

Almost every corner in software is a circular arc, the CSS `border-radius`, which jumps abruptly from the straight edge into the curve (G1 continuity). In the physical world, the objects we handle every day, a stone polished by a river, a worn bar of soap, an iPhone, have curves that blend continuously (G2). No corners, no abrupt seams, just smooth transitions.

Apple has known this since 2013, when the iOS app icon mask was replaced with a superellipse. Ever since, most "premium feeling" interfaces are quietly using squircles without you noticing.

This skill turns that into the default for anyone building UI with Claude Code.

## What it does

When active, Claude uses `<Squircle>` and `<StaticSquircle>` with `cornerSmoothing: 0.6` (the Apple/iOS preset) in place of `rounded-xl`, `rounded-2xl`, and so on, for buttons, cards, avatars, hero images, and app icons.

Inside:

- Real `@squircle-js/react` API (props, `asChild`).
- A recommended `cornerRadius` scale per element type.
- 7 copy paste patterns (button, card, avatar, hero, iOS app icon, Framer Motion, border).
- Anti blob rule for small elements.
- Common mistakes table.
- Exceptions that stay on plain CSS (`rounded-full` for true circles, small inputs).

## Forcing it as your default

The skill activates when your prompt contains triggers like "squircle", "iOS style", "Apple feel". To make it the default even when you only say "build me a card", add a personal memory file under `~/.claude/projects/<your-project>/memory/` instructing Claude to invoke it instead of `rounded-*`.

## License

MIT.
