# dillonagency.co

Agency conversion site for Dillon Agency. Growth leadership for traditional industries.

## Files

```
index.html       Home — positioning, capabilities, pattern, CTA
approach.html    Approach — methodology, differentiators, audience
background.html  Background — career arc as proof of pattern (4 chapters)
results.html     Results — venture outcomes, advisory work, what's being built
contact.html     Contact — form + Calendly + social links
style.css        Shared styles (AdvisorOS / Dieter Rams design system)
main.js          Scroll animations + mobile nav
vercel.json      Clean URLs, security headers, caching
```

## Design System

- Palette: Ivory (#FAF8F0), Cream (#F0EBE0), Cognac (#8C6840), Deep (#1E1C18)
- Typography: Cormorant Garamond (display) + Inter (body)
- Components: Walnut top bar, panel-seam grids, leather stitch dividers,
  chronograph gauge strip, cognac accent reveals
- Fully responsive with mobile hamburger nav

## Deploy to Vercel

1. Create a new GitHub repo: `dillonagency-site`
2. Push all files to the `main` branch
3. Go to vercel.com, click "Add New Project"
4. Import your GitHub repo
5. Framework Preset: "Other"
6. Root Directory: leave as default
7. Click "Deploy"
8. In Project Settings > Domains > Add: dillonagency.co
9. Update your domain DNS:
   - Add a CNAME record: `@` -> `cname.vercel-dns.com`
   - Or use Vercel nameservers for full DNS management
10. SSL is automatic

## Cross-linking

- Footer links to keirdillon.com and LinkedIn
- Background page links to keirdillon.com/story for the personal narrative
- keirdillon.com should link back to dillonagency.co from the "What I Do Now" section

## To-do before launch

- [ ] Add Dillon Agency logo (SVG or transparent PNG from CDN)
- [ ] Replace favicon
- [ ] Add OG image for social sharing
- [ ] Connect contact form backend (Formspree or Vercel serverless function)
- [ ] Add Calendly URL to "Book a Call" button on contact page
- [ ] Connect analytics (Plausible or Google Analytics)
- [ ] Submit sitemap to Google Search Console
- [ ] Update email address if keir@dillonagency.co is not yet set up

## Local development

Just open any HTML file in a browser. No build tools needed.
For live reload during development: `npx serve .` (requires Node.js)
