# Publishing Checklist — Patek Philippe Collector Series

**Owner:** Publisher | **Version:** 1.0 | **Created:** 2026-04-06

Every article must clear this checklist before publication. No exceptions.

## Pre-Publish Requirements

### Editorial
- [ ] Review verdict: ✅ Approved by Reviewer
- [ ] Final editorial sign-off from Editor
- [ ] All fact-check issues resolved or acknowledged

### HTML Quality
- [ ] Valid semantic HTML (`<article>`, `<section>`, proper heading hierarchy)
- [ ] No inline styles
- [ ] No markdown artifacts or escaped characters
- [ ] No broken or placeholder links
- [ ] Tables used only when comparison adds genuine value
- [ ] Clean, readable source code

### SEO Metadata
- [ ] `<title>` — clear, search-friendly, under 60 characters
- [ ] `<meta name="description">` — compelling, under 160 characters
- [ ] Open Graph tags: `og:title`, `og:description`, `og:type`
- [ ] Target keyword/theme documented
- [ ] URL slug is clean and descriptive

### Content Structure
- [ ] H1 matches the article title exactly
- [ ] Introduction frames the collector problem in 2-3 sentences
- [ ] Conclusion ends with a practical takeaway
- [ ] Source list included (inline or appendix)
- [ ] No unsourced market or authenticity claims

### Final Formatting
- [ ] Filename follows convention: `patek-[slug].html`
- [ ] File placed in `content/published/`
- [ ] Draft file in `content/drafts/` retained for reference

### Deployment
- [ ] Published to target platform (TBD — see #56)
- [ ] Live URL verified
- [ ] Social/distribution metadata renders correctly

## SEO Metadata Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>[Article Title — Patek Philippe Collector Guide]</title>
  <meta name="description" content="[1-2 sentence summary for search results, under 160 chars]">
  <meta property="og:title" content="[Article Title]">
  <meta property="og:description" content="[Same as meta description or slightly varied]">
  <meta property="og:type" content="article">
</head>
<body>
  <article>
    <!-- Article content here -->
  </article>
</body>
</html>
```
