# Markdownlint Shared Configurations

A collection of reusable [markdownlint](https://github.com/DavidAnson/markdownlint) base configurations for different use cases and project types.

## Available Configurations

### 🎯 strict.jsonc

**Recommended for:** New projects, libraries, technical documentation requiring high consistency.

Enables all rules with strict settings for maximum consistency and quality. Enforces:

- 80-character line length limit
- ATX-style headings (`#`)
- Dash-style unordered lists (`-`)
- 2-space list indentation
- Backtick code fences
- Asterisk emphasis/strong
- Consistent horizontal rule style (`---`)

**Usage:**

```json
{
  "extends": "@gfmio/config-markdownlint/strict.jsonc"
}
```

### 🌊 relaxed.jsonc

**Recommended for:** READMEs, personal projects, quick documentation.

Enables all rules but disables the most commonly problematic ones:

- No line length limit (MD013)
- Allows duplicate headings in different sections (MD024/siblings_only)
- Allows inline HTML (MD033)
- Allows bare URLs (MD034)
- Allows emphasis as heading replacement (MD036)
- Doesn't require first line to be heading (MD041)

**Usage:**

```json
{
  "extends": "@gfmio/config-markdownlint/relaxed.jsonc"
}
```

### 🎨 style-guide.jsonc

**Recommended for:** Teams establishing style guidelines, enforcing consistency.

Focuses on style consistency rather than strictness. Uses "consistent" settings where possible, allowing teams to choose their own style while enforcing consistency:

- Consistent heading, list, code fence, and emphasis styles
- Proper whitespace and indentation rules
- No line length limit
- Allows duplicate headings in siblings

**Usage:**

```json
{
  "extends": "@gfmio/config-markdownlint/style-guide.jsonc"
}
```

### 🐙 github-flavored.jsonc

**Recommended for:** GitHub repositories, projects using GitHub-flavored Markdown features.

Optimized for GitHub's Markdown rendering with:

- 120-character line length (comfortable for GitHub UI)
- Allows common GitHub HTML elements (details, summary, kbd, etc.)
- Fenced code blocks with backticks
- ATX-style headings
- Dash-style lists

**Usage:**

```json
{
  "extends": "@gfmio/config-markdownlint/github-flavored.jsonc"
}
```

### 📚 documentation.jsonc

**Recommended for:** Documentation sites, wikis, knowledge bases, technical writing.

Balanced configuration for documentation projects:

- No line length limit (docs often have long paragraphs)
- Allows inline HTML for rich formatting
- Supports front matter titles
- Doesn't enforce strict duplicate heading rules
- Allows documents without top-level heading
- Consistent code and emphasis styles

**Usage:**

```json
{
  "extends": "@gfmio/config-markdownlint/documentation.jsonc"
}
```

## Installation & Usage

### With markdownlint-cli2

1. **Install markdownlint-cli2:**

```bash
npm install --save-dev markdownlint-cli2
```

2. **Create your config file** (`.markdownlint.json`, `.markdownlint.jsonc`, or `.markdownlint-cli2.jsonc`):

```json
{
  "extends": "@gfmio/config-markdownlint/strict.jsonc"
}
```

3. **Run linting:**

```bash
npx markdownlint-cli2 "**/*.md"
```

### With VS Code Extension

1. **Install the extension:** [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)

2. **Configure in `.vscode/settings.json`:**

```json
{
  "markdownlint.config": {
    "extends": "@gfmio/config-markdownlint/strict.jsonc"
  }
}
```

Or reference a config file:

```json
{
  "markdownlint.customRules": [],
  "markdownlint.config": {}
}
```

And create `.markdownlint.jsonc` in your project root:

```json
{
  "extends": "@gfmio/config-markdownlint/github-flavored.jsonc"
}
```

### Direct File Copy

You can also copy any configuration file directly to your project:

```bash
cp /path/to/config-markdownlint/strict.jsonc /path/to/your/project/.markdownlint.jsonc
```

## Extending Configurations

All configurations can be extended and customized:

```json
{
  "extends": "@gfmio/config-markdownlint/strict.jsonc",
  "MD013": {
    "line_length": 100
  },
  "MD033": {
    "allowed_elements": ["br", "img"]
  }
}
```

## Multiple Configuration Inheritance

You can extend multiple configurations (later ones override earlier ones):

```json
{
  "extends": [
    "@gfmio/config-markdownlint/strict.jsonc",
    "./custom-overrides.jsonc"
  ]
}
```

## Choosing the Right Configuration

| Project Type | Recommended Config | Why |
|--------------|-------------------|-----|
| New library/package | `strict.jsonc` | Establishes high quality standards from the start |
| README only | `relaxed.jsonc` | Less friction for quick documentation |
| GitHub project | `github-flavored.jsonc` | Optimized for GitHub's Markdown rendering |
| Documentation site | `documentation.jsonc` | Balanced for long-form technical writing |
| Team style guide | `style-guide.jsonc` | Enforces consistency while allowing team preferences |

## CI/CD Integration

### GitHub Actions

```yaml
name: Lint Markdown

on: [push, pull_request]

jobs:
  markdown-lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npx markdownlint-cli2 "**/*.md"
```

### Pre-commit Hook (with husky + lint-staged)

**package.json:**

```json
{
  "lint-staged": {
    "*.md": "markdownlint-cli2"
  }
}
```

**.husky/pre-commit:**

```bash
#!/bin/sh
npx lint-staged
```

## Common Overrides

### Allow Longer Lines

```json
{
  "extends": "@gfmio/config-markdownlint/strict.jsonc",
  "MD013": {
    "line_length": 120
  }
}
```

### Allow Specific HTML Elements

```json
{
  "extends": "@gfmio/config-markdownlint/strict.jsonc",
  "MD033": {
    "allowed_elements": ["br", "details", "summary"]
  }
}
```

### Disable Specific Rules

```json
{
  "extends": "@gfmio/config-markdownlint/strict.jsonc",
  "MD041": false,
  "MD013": false
}
```

## Inline Rule Control

You can temporarily disable rules in your Markdown files:

```markdown
<!-- markdownlint-disable MD013 -->
This line can be as long as you want without triggering the line length rule.
<!-- markdownlint-enable MD013 -->

<!-- markdownlint-disable-next-line MD033 -->
<div>This HTML is allowed</div>
```

## Contributing

Suggestions for new configurations or improvements to existing ones are welcome!

## License

[MIT](LICENSE)

## Resources

- [markdownlint repository](https://github.com/DavidAnson/markdownlint)
- [Rules reference](https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md)
- [markdownlint-cli2](https://github.com/DavidAnson/markdownlint-cli2)
- [VS Code extension](https://github.com/DavidAnson/vscode-markdownlint)
