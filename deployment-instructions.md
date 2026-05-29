# Good Game — Landing Page Deployment

Two builds of the same landing page, ready to host anywhere.

```
dist/
├─ good-game-landing.html            ← for ANY static host
└─ shopify/
   └─ templates/
      └─ page.good-game.liquid       ← for Shopify
```

Both files are fully self-contained — fonts, images, CSS, and JS are inlined. No CDN dependencies, no build step.

---

## A. Hosting on any static host (Netlify, Vercel, Cloudflare Pages, S3, GitHub Pages, your own server…)

Upload **one file**: `good-game-landing.html`.

### Quick options

- **Netlify drop:** drag `good-game-landing.html` onto https://app.netlify.com/drop. Done.
- **Vercel:** put it in a folder, run `vercel --prod`. Done.
- **Cloudflare Pages:** drag-and-drop the folder.
- **Custom server / S3 / nginx:** upload it as `index.html` at whatever path you want (`/`, `/good-game/`, `/landing/`).

Rename to `index.html` if you want it served as the default document at the root of a domain.

---

## B. Hosting on Shopify

Use the file in `dist/shopify/templates/page.good-game.liquid`. This is a Shopify **custom page template** that bypasses your theme's layout entirely — the page renders exactly as the standalone file does, with no theme header/footer wrapping it.

### Step-by-step

1. **Shopify admin → Online Store → Themes → ⋯ → Edit code** on your live (or a duplicate) theme.
2. Under **Templates**, click **Add a new template**.
   - Template for: **Page**
   - File type: **liquid**
   - Name: **good-game**
   - This creates `templates/page.good-game.liquid`.
3. **Delete everything** in the new file. Open `dist/shopify/templates/page.good-game.liquid` from this download, copy its entire contents, and paste it in. Save.
4. **Online Store → Pages → Add page.**
   - Title: e.g. *Good Game by T-Pain*
   - Content: leave blank (the template ignores it)
   - In the right sidebar under **Theme template**, choose **good-game**
   - Save
5. The page is now live at `https://yourstore.com/pages/<page-handle>` (handle is auto-generated from the title — you can edit it).

### Notes for Shopify

- The first line `{% layout none %}` tells Shopify NOT to wrap the page in `theme.liquid`. That's what lets the page render full-bleed without your theme's nav/footer fighting it.
- If you want the theme's header/footer **around** the landing page instead, delete the `{% layout none %}` line. The page will then render inside your normal theme chrome. (Recommended only if your theme is minimal — otherwise the landing's own design competes with theme styles.)
- Because everything is inlined, this template will be large (~2 MB). Shopify accepts it, but page-load is slightly slower than splitting assets out. For best performance long-term, consider uploading the images/fonts to Shopify's **Files** and rewriting the `data:` URLs to CDN URLs — not required to launch.
- Do **not** paste this HTML into the WYSIWYG **Page content** editor in Shopify admin. That editor strips `<script>` tags and will break the page. The template-file route above is the correct path.

---

## Updating the page

If you want to make changes, edit the source files in this project (`landing.jsx`, `components.jsx`, `styles.css`, etc.), rebuild the standalone, and replace both deployed files.
