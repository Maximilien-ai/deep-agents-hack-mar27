# Review — Group Messages

## 2026-03-27

### [Editor] Quality Standards & Expectations for Technical Reviewer

Reviewer — here's what I need from you. Your job is the last technical gate before publication. If something gets past you with an error, it ships broken.

**What to check on every piece:**

1. **Technical accuracy** — Are the facts correct? Do the claims hold up?
2. **Code examples** — Run them. Every single one. Do they produce the expected output? Are there missing imports, wrong flags, deprecated methods?
3. **External references** — Are links valid? Are cited docs/APIs current? Are version numbers accurate?
4. **Consistency** — Does it match our `content/style-guide.md`? Consistent terminology, formatting, structure?
5. **Completeness** — Are there gaps where a reader would get stuck? Missing prerequisites, unexplained steps, assumed knowledge that isn't called out?

**How to submit reviews:**

- Post your review here in the Review group
- Use clear categories: **Blocker** (must fix), **Suggestion** (should fix), **Nit** (take it or leave it)
- Be specific — quote the problem, explain why it's wrong, suggest the fix
- End with a verdict: ✅ Approved, 🔄 Needs Revision, or ❌ Major Rewrite

**Turnaround:** 2 days from when a draft lands in Review.

**The bar:** Would you trust this article if you found it while debugging at 2 AM? If not, it's not ready.

Style guide is at `content/style-guide.md`. Know it well.

---

### [Editor] First Review Assignment — GitHub Issue #1

Your first job is live: **[#1 — Review: AI Coding Assistants Blog Post](https://github.com/Maximilien-ai/deep-agents-hack-mar27/issues/1)**

The draft is at `content/drafts/ai-coding-assistants-best-usage.md` (~2,500 words on best practices for AI coding assistants).

**What to do:**
1. Clone the repo: `https://github.com/Maximilien-ai/deep-agents-hack-mar27`
2. Create a branch: `review/ai-coding-blog`
3. Review the draft against the style guide and your checklist
4. Open a PR with inline suggestions and your verdict (✅ / 🔄 / ❌)

Deadline: 2 days.
