# Gabe Style — TypeScript

Apply these when writing or editing TypeScript / JavaScript code.

## Avoid ternaries when a clearer structure exists

Prefer statements over nested or dense expressions.

- Use early returns for rendering branches.
- Split complex value selection into small variables with descriptive names.
- Use plain `if` blocks when the expression would otherwise become hard to scan.

Accept a simple ternary only when both branches are very short and the result is obviously easier to read than the equivalent statement form.

## Match braces to statement length

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
