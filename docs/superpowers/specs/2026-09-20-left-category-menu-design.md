# Left category menu — design

## Goal
Home and list pages show a left-hand menu — All, Game Dev, General Dev, Books — that
groups posts by a new single-valued `category:` front matter field.

## Constraints
- `blog/themes/hugo-theme-dream` is a nested git repo (gitlink, no `.gitmodules`); edits
  inside it are not tracked here and never reach the server (`git pull && hugo`). All
  changes live in `blog/layouts/`, `blog/static/`, `blog/content/`, `blog/hugo.toml`.
- The theme's Tailwind CSS is precompiled. Only classes already in
  `assets/css/output.css` work; anything else goes in `static/css/custom.css`, loaded
  through the theme's `params.advanced.customCSS` hook.

## Design
- **Taxonomy** — `hugo.toml` gets `[taxonomies]` with the defaults restated plus
  `group = "category"`. The front matter key is the plural, so posts use `category:`.
  Term pages: `/category/game-dev/`, `/category/general-dev/`, `/category/books/`.
  The existing fine-grained `categories:` lists and their pages are untouched.
- **Posts** — every post gets exactly one `category:` line, inserted above `categories:`.
  Game Dev: the 14 `unreal_*` posts, `cpp_rag_npc`, `cpp_whisper`, `cpp_networking`,
  `cpp_troubleshooting1`, `llm_npc_tuning`, `ffxiv_analyzer`. Everything else: General Dev.
- **Books** — no posts yet; `content/category/books/_index.md` makes the term page exist
  so the menu link is not a 404.
- **Menu** — `layouts/partials/groupMenu.html`: daisyUI `menu`, vertical on `md+`,
  horizontal on mobile, `menu-active` on the current item, post count per item. Item
  order is fixed in `params.groupMenu`.
- **Pages** — `index.html`, `section/posts.html`, `_default/term.html` are copied from the
  theme into `blog/layouts/` and wrapped in the two-column flex layout the theme's
  `term.html` already uses. The home grid caps at 3 columns. On term pages the menu sits
  above the existing badge cloud. Single post pages are unchanged.

## Verification
Build to a scratch directory (`hugo -D -d <scratch>`) so tracked `blog/public/` is not
touched. Check: three term pages exist; Game Dev + General Dev counts equal the post
count; menu present on home, `/posts/`, and term pages; absent on single posts.
