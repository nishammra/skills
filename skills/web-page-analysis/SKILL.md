---
name: web-page-analysis
description: Analyze web pages and produce structured content plans for video creators. Use this skill when the user provides a URL or pasted web page content (HTML or cleaned text) and wants any combination of: (1) a content summary with key points, topic, audience, and author goal extraction, (2) short-form video hook ideas for TikTok/Reels/YouTube Shorts, (3) a long-form YouTube video outline, or (4) call-to-action ideas. Especially relevant for restaurant, AI, and MENA-focused content creators.
---

# Web Page Analysis & Video Content Planning

Analyze web page content and produce structured, actionable content plans for video creators.

## Input Handling

Accept input in any of these forms:

- **URL**: Fetch the page using the WebFetch tool, then analyze the returned content.
- **Raw HTML**: Strip tags mentally, focus on visible text, headings, and metadata.
- **Cleaned text**: Analyze directly.

If the user provides a URL, fetch it first. If the content is behind a paywall or fails to load, inform the user and ask them to paste the text instead.

## Analysis Steps

### 1. Extract Page Metadata

Identify and state clearly:

- **Main topic**: One sentence describing what the page is about.
- **Target audience**: Who the author is writing for (demographics, expertise level, interests).
- **Author's goal**: What the author wants the reader to do or feel (inform, persuade, sell, entertain, educate).

### 2. Summarize Key Points

Produce 5–10 bullet points capturing the most important information. Each bullet should be a complete thought, not a sentence fragment. Prioritize:

- Claims backed by data or evidence
- Unique insights not widely known
- Actionable takeaways
- Controversial or attention-grabbing statements

### 3. Short-Form Video Hooks

Generate exactly 5 hook ideas optimized for TikTok, Instagram Reels, or YouTube Shorts. Each hook should:

- Be under 15 words
- Create curiosity or urgency in the first 2 seconds
- Be speakable (written for voice, not text)
- Use one of these proven formats: question, bold claim, "Did you know…", contrarian take, or story opener

### 4. Long-Form YouTube Outline

Create 1 outline with 8–12 sections. Each section includes:

- A clear section title
- 2–3 bullet points describing what to cover
- Keep the structure suitable for a 10–20 minute video

Follow this narrative arc: hook → context/problem → deep dive → examples → actionable takeaways → conclusion with CTA.

### 5. Call-to-Action Ideas

Produce 3 CTA ideas. Tailor them to the content's domain:

- **Restaurant content**: Reservation links, menu highlights, location visits, food delivery apps.
- **AI content**: Tool signups, newsletter subscriptions, tutorial series, community joins.
- **MENA content**: Regional platform follows, cultural event promotions, local business spotlights.
- **General content**: Subscribe, comment, share, or visit a linked resource.

If the content doesn't clearly fit restaurant/AI/MENA domains, produce general CTAs that match the actual topic.

## Output Format

Always use this exact structure:

```
SUMMARY:
- [bullet 1]
- [bullet 2]
- ...

VIDEO_PLAN:

Hooks:
1. [hook]
2. [hook]
3. [hook]
4. [hook]
5. [hook]

YouTube Outline:
1. [Section Title]
   - [point]
   - [point]
2. [Section Title]
   - [point]
   - [point]
   - [point]
...

CTAs:
1. [CTA idea]
2. [CTA idea]
3. [CTA idea]
```

## Quality Guidelines

- Write hooks that a real person would say on camera, not marketing copy.
- YouTube outlines should flow naturally as a spoken narrative, not read like a blog post.
- Summaries must be accurate to the source material — do not fabricate claims or statistics.
- When the source page is thin on content, note this and adjust output length accordingly rather than padding with filler.
- If the page covers multiple distinct topics, focus the video plan on the strongest single angle rather than trying to cover everything.
