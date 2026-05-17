<p align="center">
  <img src="https://raw.githubusercontent.com/themattvision/squircle-ui-skill/main/banner.png" alt="squircle-ui banner: a river-polished stone shaped like a squircle next to a sharp CSS box" width="100%">
</p>

<p align="center">
  <a href="https://github.com/themattvision/squircle-ui-skill/releases"><img alt="version" src="https://img.shields.io/github/v/release/themattvision/squircle-ui-skill?style=flat-square&label=version&color=fffb00&labelColor=14141a"></a>
  <a href="LICENSE"><img alt="license" src="https://img.shields.io/badge/license-MIT-black?style=flat-square"></a>
  <a href="https://github.com/anthropics/claude-code"><img alt="claude code" src="https://img.shields.io/badge/Claude%20Code-skill-black?style=flat-square"></a>
  <a href="https://squircle.js.org"><img alt="squircle-js" src="https://img.shields.io/badge/built%20on-%40squircle--js%2Freact-fffb00?style=flat-square&labelColor=14141a"></a>
</p>

<h1 align="center">squircle-ui</h1>

<p align="center">
  A Claude Code skill that makes <a href="https://squircle.js.org"><code>@squircle-js/react</code></a> the default for rounded UI in React, replacing <code>border-radius</code> and Tailwind's <code>rounded-*</code>.
</p>

<p align="center">
  <a href="https://squircle-ui.pages.dev"><b>Live demo →</b></a>
</p>

## Install

One liner with the [`skills`](https://github.com/vercel-labs/skills) CLI by Vercel:

```bash
npx skills add themattvision/squircle-ui-skill -a claude-code -g
```

That drops the skill into `~/.claude/skills/squircle-ui`, available globally to Claude Code. Drop `-g` to install only inside the current project (`./.claude/skills/`).

Or clone manually:

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/themattvision/squircle-ui-skill.git squircle-ui
```

Claude Code auto discovers the skill from that point on.

## Why this exists

I believe squircles could harmonize digital life with the real world a little more.

Almost every corner in software is a circular arc, the CSS `border-radius`, which jumps abruptly from the straight edge into the curve (G1 continuity). In the physical world the objects we handle every day, a stone polished by a river, a worn bar of soap, an iPhone, have curves that blend continuously (G2). No corners, no abrupt seams, just smooth transitions.

Apple has known this since 2013, when the iOS app icon mask was replaced with a superellipse. Ever since, most "premium feeling" interfaces are quietly using squircles without you noticing.

This skill turns that into the default for anyone building UI with Claude Code.

## What it does

When active, Claude uses `<Squircle>` and `<StaticSquircle>` with `cornerSmoothing: 0.6` (the Apple/iOS preset) in place of `rounded-xl`, `rounded-2xl`, and so on, for buttons, cards, avatars, hero images, and app icons.

Inside:

- Real `@squircle-js/react` API (props, `asChild`).
- A recommended `cornerRadius` scale per element type.
- Seven copy paste patterns (button, card, avatar, hero, iOS app icon, Framer Motion, border).
- Anti blob rule for small elements.
- Common mistakes table.
- Exceptions that stay on plain CSS (`rounded-full` for true circles, small inputs).

## Quick taste

```jsx
import { Squircle } from "@squircle-js/react";

export function Button({ children, onClick }) {
  return (
    <Squircle
      cornerRadius={12}
      cornerSmoothing={0.6}
      className="bg-indigo-600 px-5 py-2.5 text-white font-semibold text-sm"
      asChild
    >
      <button type="button" onClick={onClick}>
        {children}
      </button>
    </Squircle>
  );
}
```

`cornerSmoothing: 0.6` is the Apple/iOS preset. `asChild` clips the `<button>` directly, no extra wrapper, no broken focus ring.

## Forcing it as your default

The skill activates when your prompt contains triggers like "squircle", "iOS style", "Apple feel". To make it the default even when you only say "build me a card", add a personal memory file under `~/.claude/projects/<your-project>/memory/` instructing Claude to invoke it instead of `rounded-*`.

## Built on

This skill is a wrapper of opinions on top of [`@squircle-js/react`](https://squircle.js.org), the library that does the actual math. All credit for the rendering goes to that project. squircle-ui exists to make Claude use it correctly and consistently in place of `rounded-*`.

## Author

Made by **Matteo Zampieri**, creator [@themattvision](https://instagram.com/themattvision) on Instagram.

If you ship something built with this skill, I would love to see it.

## License

MIT. See [LICENSE](LICENSE).
