---
name: skill-polisher
description: >
  Polishes standalone skills and multi-skill bundles for ClawHub readability without sacrificing LLM effectiveness. Use when improving a skill's listing, making a skill look better on ClawHub, or preparing a skill for publish. Rewrites SKILL.md with better formatting, then audits changes to ensure nothing the LLM needs was lost. Supports cross-skill dependency mapping and regression checking for bundles. Moved content goes to references/ — never deleted.
---

# Skill Polisher 🪚

Improve a skill's SKILL.md for ClawHub readability. Run after the skill is built and tested — never before.

Works in two modes: **standalone** (single skill) and **bundle** (multi-skill bundle with shared infrastructure and cross-skill data contracts). Mode is auto-detected from the directory structure.

## Priority Order

1. **LLM execution** — the agent must still be able to do the task correctly
2. **Triggering** — the skill must still activate on the right queries
3. **ClawHub display** — browsers should understand the skill at a glance

Readability supports these goals. Never sacrifice clarity for aesthetics.

## How It Works

**Standalone:**
```
1. Read the existing SKILL.md
2. Rewrite for readability (apply rules from references/)
3. Audit the rewrite against the original
4. Output: polished SKILL.md + audit report + any new reference files
```

**Bundle:**
```
1. Detect bundle structure
2. Build dependency map (cross-skill field names, paths, references)
3. Clean stale §X.Y architecture references across all files
4. For each sub-skill:
   a. Rewrite for readability
   b. Update cross-skill references (already-polished skills)
   c. Queue fixes for not-yet-polished skills
   d. Run regression check
   e. Audit the rewrite
5. Cross-validation pass (final contract verification)
6. Output: polished SKILL.md files + audit reports + reference files
```

See [references/bundle-rules.md](references/bundle-rules.md) for the full bundle workflow.

## Before Polishing

Ensure the skill is:
- ✅ Functionally working (scripts tested)
- ✅ Description is correct (triggers on the right queries)

If either fails, fix first. Don't polish a broken skill.

## Polishing Rules

Read [references/rules.md](references/rules.md) for the full set of formatting rules.

Key principles:
- Short paragraphs (1-2 lines) — dense blocks kill readability
- Code blocks for lists — renders as visual boxes on ClawHub
- Emoji as section anchors — 🔒 📊 ⚡ give instant visual context
- One code block per concept — not three variations for three platforms
- **Protected content stays inline** — behavioral contracts, safety rules, data integrity constraints never get moved to references/

## What Gets Moved to references/

Content that's valuable but doesn't belong in the storefront:

- Platform-specific formatting examples (Slack, WhatsApp, Discord)
- Detailed credential setup tutorials
- Extended configuration reference
- Historical changelogs

**Rule: Content is moved, never deleted. Every reference file must be valid and useful.**

**Exceptions — never move these:**
- Anti-hallucination / data integrity rules
- Safety constraints ("does NOT do", "what NOT to do")
- Confirmation requirements (when to ask the user)
- Interactive behavior rules (auto-proceed vs ask)
- Display format contracts (pipeline views, output templates)
- Cross-skill data contracts (field names, report structures, shared paths)
- Content referenced by other skills in the bundle

## The Audit

After rewriting, compare the original against the polished version. Flag:
```
⚠ Potentially risky changes:
• Protected content moved to references/
• Cross-skill data contract broken
• Security notes removed
• Credential instructions lost
• Trigger phrases removed from description
• Reference files created but empty or incomplete
```

See [references/audit-guide.md](references/audit-guide.md) for the full audit checklist.

## Output

Present to the user:

**Standalone:** polished SKILL.md + any new reference files + audit report.

**Bundle:** polished SKILL.md per sub-skill + any new reference files + audit report per sub-skill + final cross-validation report.

Wait for user approval before overwriting originals.

## When to Use

- "Make this skill look better on ClawHub"
- "Polish my SKILL.md"
- "Improve this skill's listing"
- "Polish my bundle"
- "Clean up these skills"

## When NOT to Use

- On extracted/inline sub-skills from a bundle install. These are flat copies that have lost their bundle context. The polisher needs the original nested bundle structure to preserve cross-skill contracts. If you spot signs of an extracted sub-skill (references to shared directories like `$WEBCLIENT_STUDIO_CONFIG_DIR/shared/`, cross-skill field names, or bundle-specific env vars), advise the user to point the polisher at the full bundle source instead.
- On skills that aren't functionally working yet.
- As a replacement for the skill-creator — this polishes existing skills, it doesn't build them.

## Not a Replacement for skill-creator

This skill polishes existing skills. Use the built-in `skill-creator` to build skills from scratch. Polish after building, not during.
