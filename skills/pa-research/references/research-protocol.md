# Deep Technical Research Protocol & Source Verification Guide

This guide establishes the protocols for conducting rigorous, evidence-backed technical research using `pa-research`. Every investigation must balance fast factual querying with live browser inspection, transparently handling access boundaries and outputting production-grade, opinionated conclusions.

---

## 1. Source Credibility Hierarchy

When gathering information, prioritize sources according to the following tiering:

| Tier | Category | Examples | Usage Guidance |
|---|---|---|---|
| **Tier 1** | **Primary / Authoritative** | Official documentation, official GitHub repositories, release notes, RFCs, W3C standards, maintainer blogs. | **Mandatory** for API signatures, architectural invariants, breaking changes, and configuration syntax. |
| **Tier 2** | **Secondary / Empirical** | Rigorous engineering blogs (e.g., Cloudflare, Netflix TechBlog), vetted benchmark suites, migration case studies. | Used for real-world performance numbers, scaling trade-offs, and production caveats. |
| **Tier 3** | **Tertiary / Community** | Stack Overflow, Reddit (`r/programming`), GitHub issues, community discussions. | Used **strictly** to discover common developer friction, obscure edge-case bugs, or undocumented workarounds. Never rely solely on Tier 3 for architectural decisions. |

---

## 2. Tool Routing: Static Search vs. Live Browser Subagent

Choose the appropriate tool based on the nature of the research query:

```text
Query Received
     │
     ├─► Static API, factual syntax, package versions, RFC docs?
     │     └──► Execute fast search (`search_web`)
     │
     └─► Interactive UI library, visual layout, client-rendered SPA,
         live dashboard, complex seat map, or interactive demo?
           └──► Launch Chrome DevTools browser subagent (`invoke_subagent` with `TypeName: "browser"`)
```

### When to Use Live Browser (`browser` Subagent):
- Navigating client-rendered single-page applications (React/Next.js/Vue docs with client-side search).
- Inspecting live interactive visual UI components (e.g. comparing Shadcn UI vs. Mantine components in the browser).
- Evaluating live network calls, console errors, or DOM layout behaviors.
- Reviewing package bundle sizes on interactive visualization tools (e.g. Bundlephobia).

### When to Use Static Web Search (`search_web`):
- Looking up specific method signatures or TypeScript declarations.
- Reading standard GitHub README files or release tags.
- Verifying deprecation notices and changelogs.

---

## 3. Transparent Access & Block Reporting Protocol

Antigravity operates with strict boundaries regarding authenticated portals, paywalls, and anti-bot verification challenges.

> [!CAUTION]
> **Zero Guessing on Blocked Origins**
> If a target site requires a login, displays a CAPTCHA or Cloudflare Turnstile challenge, returns a 403 Forbidden, or fails to connect:
> 1. You must **NEVER** attempt to bypass, spoof, or hack around the restriction.
> 2. You must **NEVER** guess or invent the contents of the unvisited page.
> 3. You must **EXPLICITLY** report the block to the user immediately.

### Standard Block Disclosure Format
When a source cannot be accessed, format the notification as:

```markdown
> [!WARNING]
> **Source Access Inaccessible / Gated**
> - **Target URL**: `https://example.com/restricted-docs`
> - **Reason**: [403 Forbidden / Cloudflare Turnstile Verification / Authentication Gate / Network Timeout]
> - **Action Taken**: Halting attempt on this origin without guessing contents. Proceeding with alternative public documentation and mirrors.
```

---

## 4. Synthesis & Decision Output Format

Every completed research investigation should deliver findings in the following structure:

````markdown
# Research Report: [Topic / Unknown]

## 1. Executive Summary
- **Direct Verdict**: [1-2 sentences delivering the concrete conclusion or recommended choice]
- **Confidence Level**: HIGH | MEDIUM | LOW
- **Primary Sources**:
  - [Title of Official Source](https://actual-url.com) (Accessed: YYYY-MM-DD)

## 2. Options & Trade-Off Comparison

| Option | Primary Strengths | Critical Caveats / Limitations | Performance & Footprint | Maintenance / Maturity |
|---|---|---|---|---|
| **Option A** (Recommended) | ... | ... | ... | ... |
| **Option B** | ... | ... | ... | ... |

## 3. Concrete Code Demonstration
```typescript
// Complete, idiomatic, typed implementation demonstrating the recommendation
// Free of placeholder stubs and narrative comments
```

## 4. Real-World Gotchas & Edge Cases
- **Failure Mode 1**: [Description and prevention]
- **Compatibility Note**: [Browser, Node.js, or OS requirements]

## 5. Proposed Project Decision (For Memory Sync)
```markdown
### TD-XXX: [Adopt X for Y]
- Status: PROPOSED
- Date: YYYY-MM-DD
- Source: pa-research investigation
- Decision: [Adopt Option A]
- Rationale: [Core technical justification based on trade-offs]
- Revisit when: [Condition that could invalidate this choice]
```
````
