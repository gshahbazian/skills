# Gabe Style — React

Apply these when writing or editing React / JSX / TSX code.

## Use logical AND for conditional JSX without a fallback

When JSX should render only when a condition passes and render nothing otherwise, use `&&` instead of a
ternary whose second branch is `null`.

Prefer:

```tsx
{conversation && (
  <OnboardingProgress progress={conversation.plan.progress} />
)}
```

Avoid:

```tsx
{conversation ? (
  <OnboardingProgress progress={conversation.plan.progress} />
) : null}
```

Make the condition explicitly boolean when another falsy value, such as `0` or an empty string, could be
rendered accidentally.

## Prefer direct layout fixes over `min-w-0`

Do not add `min-w-0` to make flex or grid layouts behave. Treat it as a layout band-aid.

Fix the actual sizing or overflow constraint instead:

- Adjust `flex`, `basis`, `shrink`, `grow`, track sizing, or container width rules.
- Change the DOM structure when the layout responsibility is in the wrong element.
- Use explicit overflow handling when content should clip or scroll.

If a change seems to require `min-w-0`, stop and look for the structural cause first.
If min-w-0 is absolutely necessary ask me before using it.

## Inline React props at the component boundary

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

## Extract long conditional derived values out of React components

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
