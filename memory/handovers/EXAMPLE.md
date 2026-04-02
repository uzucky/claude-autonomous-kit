# Handover: Blog migration — pages 1-3 complete

**Date**: 2026-04-01
**Session goal**: Migrate first 3 pages from WordPress to Next.js

## Completed
- Migrated home page, about page, and contact page
- Set up TailwindCSS with the existing color scheme
- Fixed responsive layout issue on mobile (nav was overlapping)

## Incomplete
- Blog post pages (5 remaining) — need to handle MDX parsing
- Image optimization — currently using unoptimized <img> tags
- SEO meta tags — not yet migrated from WordPress

## Decisions
- Chose MDX over plain Markdown — need code blocks with syntax highlighting
- Kept the original URL structure (/blog/slug) for SEO continuity

## Next session: start here
- [ ] Set up MDX processing with next-mdx-remote
- [ ] Migrate the 2 most-read blog posts first (test the pipeline)

## Notes
This is an example file. Delete it and let /handover create real ones.
