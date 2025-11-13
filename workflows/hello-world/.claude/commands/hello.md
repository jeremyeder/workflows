---
description: Say hello and create a greeting file
---

## Say Hello

### User Input

```text
$ARGUMENTS
```

---

## Steps

1. Get the user's name (from $ARGUMENTS or ask)
2. Create a greeting
3. Save it to a file

---

## Implementation

If $ARGUMENTS is empty, use "World" as the name.

Create a greeting file:

```bash
mkdir -p artifacts/greetings
```

Create the greeting:

```markdown
# Hello, [NAME]!

Welcome to ACP workflows! 🎉

This is your first workflow output.

Generated at: [TIMESTAMP]
```

Save to `artifacts/greetings/hello-[name].md`

Tell the user:
```
✅ Hello, [NAME]!

I created a greeting for you at:
  artifacts/greetings/hello-[name].md

That's it! You just ran your first workflow. 🎉
```
