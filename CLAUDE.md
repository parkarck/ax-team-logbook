# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**AX팀 운영일지 (AX Team Operations Logbook)** is a static website documenting the AI Experience Team's 32-week program (2026-04-01 ~ 2026-12-09). The team meets every Wednesday to explore AI technologies (AI Agent, MCP, RAG, LLM, etc.) and maintains educational content in both blog-style HTML pages and markdown notes.

**Deployment**: Vercel static hosting at https://ax-team-logbook.vercel.app/

## Architecture

### Static Site Structure
- **Pure HTML/CSS/JS**: No build tools or frameworks
- **Dynamic rendering**: `data/weeks.js` provides 32-week data, rendered by JS in index.html and study-notes.html
- **Vercel Deployment**: Uses `@vercel/static` builder
- **Content Organization**: Weekly content in `week-{n}/` directories

### File Organization
```
/                           # Root - homepage (index.html)
├── styles.css             # Shared stylesheet
├── data/weeks.js          # 32-week program data (source of truth)
├── index.html             # Homepage with timeline + progress bar
├── study-notes.html       # Hub page for all learning notes
├── week-1/                # Week 1 content
│   ├── ai-agent-guide.html
│   ├── AI_Agent_교육_자료.html
│   └── AI_Agent_교육_자료.md
├── week-2/                # Week 2 content
│   ├── rag-guide.html
│   ├── RAG_Presentation.html
│   └── RAG_Educational_Content.md
├── week-{n}/              # Future week directories
├── vercel.json            # Deployment config
├── sitemap.xml            # SEO sitemap
├── robots.txt             # Search engine guidelines
├── site.webmanifest       # PWA manifest
├── .env                   # API keys (NEVER commit - in .gitignore)
└── .gitignore             # Git ignore rules
```

## Weekly Content Workflow

### Adding New Weekly Content (Standard Process)

1. **Place raw materials** in `week-{n}/` directory (md files, notes, etc.)

2. **Create dedicated HTML page** (`week-{n}/{topic}.html`):
   - Copy structure from existing pages (e.g., `week-1/ai-agent-guide.html`)
   - Include inline `<style>` block (self-contained page)
   - Korean language with proper HTML structure
   - Include header with back navigation to homepage

3. **Update `data/weeks.js`**:
   - Set `title`, `description`, `tags` for the week
   - Add content entries with `title` and `htmlPath`
   - Homepage and study-notes page will auto-update via JS rendering

4. **Update `sitemap.xml`**: Add `<url>` entry for new HTML page

5. **Commit and push**: Vercel auto-deploys on push to main

### Example weeks.js Entry
```js
{
  week: 3,
  date: "2026-04-15",
  title: "MCP 프로토콜 이해",
  description: "Model Context Protocol의 구조와 활용 방법",
  tags: ["MCP", "AI Agent"],
  contents: [
    { title: "MCP 교육 자료", htmlPath: "week-3/mcp-guide.html" }
  ]
}
```

## Design System

### Color Scheme (Dark Theme)
- Background: `#0f0f11`
- Card background: `#13131a`
- Border: `#1e1e2e`
- Primary text: `#e2e8f0`
- Secondary text: `#64748b`
- Accent purple: `#7c3aed` / `#a78bfa`
- Accent blue: `#2563eb` / `#60a5fa`

### Tag Colors
- AI Agent: purple (`tag-ai`)
- RAG: blue (`tag-rag`)
- MCP: green (`tag-mcp`)
- LLM: yellow (`tag-llm`)
- Default: gray (`tag-default`)

### Typography
- System fonts with Noto Sans KR for Korean
- Responsive: Mobile-first design

## Content Conventions

### Language & Style
- **Primary Language**: Korean (ko-KR)
- **Educational Tone**: Clear explanations with analogies
- **Session Tags**: AI Agent, RAG, MCP, LLM, etc.

### File Naming
- HTML guides: `week-{n}/{topic}-guide.html` (English, lowercase, hyphens)
- Korean markdown: `week-{n}/{Korean_Name}.md` accepted

## Environment Variables (.env)

The `.env` file contains sensitive API keys and is protected by `.gitignore`:
- `GEMINI_API_KEY`: For image generation when creating weekly content
- `GITHUB_TOKEN`: For GitHub API access
- `GITHUB_URL`: Repository URL

**NEVER commit .env to git.**

## Testing & Deployment

### Local Development
```bash
npx serve
# or
python -m http.server 8000
```

### Vercel Deployment
Automatic deployment on push to main via Vercel GitHub integration.

## Git Workflow

- **Branch**: `main` (production)
- **Commits**: Conventional commit messages in English
- **Protected files**: `.env` (in .gitignore)
