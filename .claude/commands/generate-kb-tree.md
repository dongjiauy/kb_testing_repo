# Generate KB Tree

Generate a knowledge tree HTML visualization for a user from their conversate_samples conversation files.

## Usage

```
/generate-kb-tree <user_id>
```

Example: `/generate-kb-tree 1338403`

## What this does

1. Reads all `.txt` files under `conversate_samples/<user_id>/`
2. Applies the KB extraction prompt at `KB_generation_prompt_native_20260522_1746.md` to build a structured knowledge tree
3. Generates an interactive HTML mind map at `conversate_samples/<user_id>/knowledge_tree.html`

## Instructions

The argument `$ARGUMENTS` is the user ID.

Follow these steps exactly:

### Step 1 — Read the prompt
Read the full prompt from:
`/Users/even/Desktop/Even Realities/Knowledge Base/KB_generation_prompt_native_20260522_1746.md`

### Step 2 — Read all source files
Read every `.txt` file in:
`/Users/even/Desktop/Even Realities/conversate_samples/$ARGUMENTS/`

Skip files with fewer than 5 messages (check the `messages:` header). For files with 100+ messages, read them in full.

### Step 3 — Build the knowledge tree
Apply all 6 steps from the prompt to produce a valid JSON tree:
- 2–5 depth-1 branches with short stable identifiers (e.g., "work", "personal", "ai")
- All nodes have non-empty `summary` and non-empty `sources` array
- No parent has only one child
- Summary format by depth:
  - Depth 0 (root): plain prose only, ≤60 words
  - Depth 1: 1–2 prose sentences + 3–5 bullets encoded as `\n• item`
  - Depth 2: 1 prose sentence + 2–4 bullets
  - Depth 3: bullets only, 2–4 bullets
  - Depth 4 / leaf: 1–2 bullets maximum

### Step 4 — Generate the HTML
Write the file to:
`/Users/even/Desktop/Even Realities/conversate_samples/$ARGUMENTS/knowledge_tree.html`

Use this exact HTML template structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Subject $ARGUMENTS — Knowledge Tree</title>
  <style>
    /* [same styles as existing knowledge_tree.html files] */
  </style>
</head>
<body>
  <!-- tree-wrap, panel, D3 script -->
  <!-- const DATA = { ...your JSON tree... }; -->
  <!-- BRANCH_COLOR must include all branch IDs used -->
</body>
</html>
```

Copy the full HTML template from an existing file:
`/Users/even/Desktop/Even Realities/conversate_samples/1338403/knowledge_tree.html`

Replace only the `const DATA = {...}` block and the `<title>` tag. Keep everything else identical.

**Branch colors to use:**
- work / professional: `#2563eb` (blue)
- ai / tech: `#7c3aed` (purple)  
- personal / life: `#059669` (green)
- health / medical: `#059669` (green)
- media / external: `#d97706` (amber)
- null (root): `#6b7280` (gray)

Add any new branch IDs and colors as needed for this user's content.

### Step 5 — Validate
Before finishing, confirm:
- The HTML file was written successfully
- The JS `const DATA = {...}` block is valid (no unescaped inner double quotes in strings)
- All nodes have non-empty summary and sources
