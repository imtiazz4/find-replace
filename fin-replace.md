# Quickly Prefix CSS Selectors in VS Code Using Regex and Multi-Cursor Editing

When working with large CSS files, there are times when you need to scope existing selectors under a parent class.

For example, you may have:

```css
.header {}
.nav {}
.nav-item {}
.footer {}
```

And you want to transform them into:

```css
.parent .header {}
.parent .nav {}
.parent .nav-item {}
.parent .footer {}
```

Instead of manually editing every selector, VS Code's regex search and multi-cursor editing can do the job in seconds.

## Step 1: Open Find

Press:

```text
Ctrl + F
```

Enable Regular Expression mode by clicking the `.*` icon or pressing:

```text
Alt + R
```

## Step 2: Use This Regex

```regex
(?<!\()\.[a-zA-Z_][a-zA-Z0-9_-]*(?:\.[a-zA-Z_][a-zA-Z0-9_-]*)*
```

### What It Matches

The regex finds CSS class selectors such as:

```css
.header
.nav-item
.card.large
.menu.dropdown.active
```

### Why the Negative Lookbehind?

```regex
(?<!\()
```

This prevents matching class-like patterns that appear immediately after an opening parenthesis.

For example:

```css
:not(.active)
```

The `.active` won't be selected because it is preceded by `(`.

This helps avoid accidental modifications in pseudo-class expressions and other CSS functions.

## Step 3: Select All Matches

Press:

```text
Ctrl + Shift + P
```

Run:

```text
Select All Occurrences of Find Match
```

Or simply use:

```text
Alt + Enter
```

All matching class selectors will now have individual cursors.

## Step 4: Insert the Parent Selector

Type:

```text
.parent 
```

before the selected text.

Since every occurrence has its own cursor, VS Code updates all matches simultaneously.

### Before

```css
.header {}
.nav {}
.footer {}
```

### After

```css
.parent .header {}
.parent .nav {}
.parent .footer {}
```

## Alternative: Search and Replace

If you prefer a single replace operation, use:

### Find

```regex
(?<!\()\.[a-zA-Z_][a-zA-Z0-9_-]*(?:\.[a-zA-Z_][a-zA-Z0-9_-]*)*
```

### Replace

```text
.parent $&
```

Where:

* `$&` = the entire matched text
* `.parent ` = the prefix being added

Result:

```css
.header
```

becomes:

```css
.parent .header
```

## Common Problem: Selectors Containing Spaces

This technique works best for direct class selectors.

Examples:

```css
.header
.card.large
.nav-item
```

But complex selectors such as:

```css
.header .logo
.card > .title
.menu - item
```

may require additional review depending on the desired output.

Always preview replacements before applying them to an entire stylesheet.

## Why This Method Is Useful

This approach is especially valuable when:

* Scoping third-party CSS
* Converting global styles into component styles
* Working with CSS modules
* Migrating legacy stylesheets
* Creating parent-specific theme overrides
* Refactoring large codebases

Instead of spending minutes or hours manually editing selectors, VS Code's regex search combined with multi-cursor editing can perform the transformation almost instantly.

Once you become comfortable with regex and multi-cursor workflows, you'll find many repetitive CSS refactoring tasks can be automated with just a few keystrokes.
