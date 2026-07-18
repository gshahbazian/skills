---
name: gabe-style
description: Apply Gabe's code style and readability preferences when writing or editing code, or when asked to follow his style or clean up code structure.
---

# Gabe Style

## Overview

Apply these defaults when editing code unless the user or repository provides a stronger instruction. Optimize for readability and low visual noise rather than compact syntax.

## General rules

These are language-agnostic and always apply.

### Always use straight quotes, never curly quotes

Always use straight quotes (`'` and `"`). Never use curly/smart quotes (`‘` `’` `“` `”`).

### Return early instead of chaining `if` / `else if` / `else`

Prefer guard clauses and fast exits. Reduce indentation and keep the main path visually straight.

Prefer:

```ts
if (!user) {
  return null;
}

if (!user.isActive) {
  return "inactive";
}

return renderDashboard(user);
```

Avoid:

```ts
if (!user) {
  return null;
} else if (!user.isActive) {
  return "inactive";
} else {
  return renderDashboard(user);
}
```

## Language-specific rules

Read the relevant reference file before editing that kind of code:

- **TypeScript / JavaScript** — see [references/typescript.md](references/typescript.md): avoiding ternaries, matching braces to statement length. The general rules above also apply.
- **React (JSX / TSX)** — see [references/react.md](references/react.md): avoiding `min-w-0`, inlining props at the component boundary, extracting long derived values out of components. React work is also TypeScript work, so apply the TypeScript and general rules too.

## Editing Heuristics

When applying this skill:

- Limit style-only edits to files already modified in the current agent session.
- Do not expand into untouched files just to enforce consistency or clean up nearby code.
- Only apply this style to additional files when the user explicitly asks for a broader sweep.
- Rewrite only as much surrounding code as needed to make the result coherent.
- Preserve behavior unless the user asked for a functional change.
- If repo-local lint rules or established conventions conflict with this skill, follow the repo and keep the result as close to these preferences as possible.
- Mention style-driven refactors in summaries so the user can distinguish them from behavior changes.
