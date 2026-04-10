# Contributing to Awesome Agent Rules

Thank you for contributing! Here's how to make your contribution count.

## What We're Looking For

- **Battle-tested rules** — Rules you've actually used in real projects, not theoretical ones
- **Problem → Solution format** — Every rule should explain what problem it solves
- **Tool-agnostic when possible** — Prefer AGENTS.md format (works across tools)

## How to Submit

1. **Fork** this repo
2. **Add** your rule in the appropriate section of README.md, or add a new template in `templates/`
3. **Sanitize** all personal data:
   - ❌ `/Users/yourname/projects/secret-project/`
   - ✅ `$PROJECT_ROOT/` or `~/projects/your-project/`
   - ❌ API keys, passwords, internal URLs
   - ✅ `YOUR_API_KEY`, `https://your-service.com`
4. **Test** — confirm the rule actually works with at least one AI coding tool
5. **Submit PR** with:
   - What tool(s) you tested with
   - What problem it solves
   - How long you've been using it (rough estimate)

## Format Guidelines

### For inline rules (in README.md)

```markdown
### 📌 Rule Category Name

**Problem:** [What goes wrong without this rule]

\`\`\`markdown
## Your Rule Title

[The actual rule content that goes in CLAUDE.md / AGENTS.md]
\`\`\`

**Why it works:** [Brief explanation]
```

### For templates (in templates/)

- Use `.md` extension
- Include a header comment explaining the template's purpose
- Use placeholder variables: `$PROJECT_NAME`, `$WORKSPACE`, etc.

## Quality Standards

- ✅ Tested with at least one AI coding tool
- ✅ Solves a real, common problem
- ✅ No personal data or secrets
- ✅ Concise — agents have limited context windows
- ❌ No promotional content
- ❌ No tool-bashing (compare objectively)

## Questions?

Open an issue! We're happy to help you shape your contribution.
