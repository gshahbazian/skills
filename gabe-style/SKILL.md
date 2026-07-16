---
name: gabe-style
description: Apply Gabe's code style and readability preferences when writing or editing code, or when asked to follow his style or clean up code structure.
---

# Gabe Style

## Overview

Apply these defaults when editing code unless the user or repository provides a stronger instruction. Optimize for readability and low visual noise rather than compact syntax.

## React rules

### Prefer direct layout fixes over `min-w-0`

Do not add `min-w-0` to make flex or grid layouts behave. Treat it as a layout band-aid.

Fix the actual sizing or overflow constraint instead:

- Adjust `flex`, `basis`, `shrink`, `grow`, track sizing, or container width rules.
- Change the DOM structure when the layout responsibility is in the wrong element.
- Use explicit overflow handling when content should clip or scroll.

If a change seems to require `min-w-0`, stop and look for the structural cause first.
If min-w-0 is absolutely necessary ask me before using it.

### Inline React props at the component boundary

Prefer inline prop typing in the component function arguments instead of separate `type` aliases that exist only for one component.

Prefer:

```tsx
export function Button({
  kind,
  disabled = false,
}: {
  kind: "primary" | "secondary";
  disabled?: boolean;
}) {
  // ...
}
```

Avoid:

```tsx
type ButtonProps = {
  kind: "primary" | "secondary";
  disabled?: boolean;
};

export function Button({ kind, disabled = false }: ButtonProps) {
  // ...
}
```

Keep a named prop type only when it is reused, exported as part of a public API, or materially improves comprehension.

### Extract long conditional derived values out of React components

When a React component needs a derived value and the conditional logic is more than a short obvious expression, prefer a helper function defined outside the component.

Inside the component, prefer:

```tsx
const summary = getSummary(item, state)
```

with the helper handling the branching through early returns:

```tsx
function getSummary(item: Item | null, state: State): string | undefined {
  if (!item) {
    return undefined
  }

  if (state === "idle") {
    return undefined
  }

  if (state === "branch") {
    return `Branch • ${item.name}`
  }

  return `Commit • ${item.sha.slice(0, 12)}`
}
```

Avoid introducing a mutable local in component render scope and assigning to it later:

```tsx
let summary: string | undefined
if (item && state !== "idle") {
  summary = item.name
}
```

Use a small inline `const` expression only when it stays genuinely short and obvious. Once the logic starts needing multiple conditions, branches, or formatting steps, extract it.

## General Typescript rules

### Avoid ternaries when a clearer structure exists

Prefer statements over nested or dense expressions.

- Use early returns for rendering branches.
- Split complex value selection into small variables with descriptive names.
- Use plain `if` blocks when the expression would otherwise become hard to scan.

Accept a simple ternary only when both branches are very short and the result is obviously easier to read than the equivalent statement form.

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

### Match braces to statement length

Use braces for any `if` block that spans multiple lines.

Prefer:

```ts
if (!user) {
  return null;
}
```

Omit braces when the full statement fits cleanly on one line.

Prefer:

```ts
if (!user) return null;
```

Avoid:

```ts
if (!user) { return null; }
```

```ts
if (!user)
  return null;
```

## Editing Heuristics

When applying this skill:

- Limit style-only edits to files already modified in the current agent session.
- Do not expand into untouched files just to enforce consistency or clean up nearby code.
- Only apply this style to additional files when the user explicitly asks for a broader sweep.
- Rewrite only as much surrounding code as needed to make the result coherent.
- Preserve behavior unless the user asked for a functional change.
- If repo-local lint rules or established conventions conflict with this skill, follow the repo and keep the result as close to these preferences as possible.
- Mention style-driven refactors in summaries so the user can distinguish them from behavior changes.
