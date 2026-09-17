# markdown-renderer

Renders markdown from a Sigma workbook into formatted, styled HTML.

**Input precedence**
1. `Markdown Text Control` — a workbook text control (type markdown into it, it renders live).
2. `Fallback Source Element` + `Markdown Column` — renders the first row's value, or every
   row joined with a blank line if `Concatenate all rows` is checked.
3. Neither bound → renders a sample document so the element never looks broken.

**Supports** GFM: headings, bold/italic/strikethrough, inline + fenced code, links, images,
nested lists, task lists, tables (including `| ---: |` column alignment), block quotes, rules.

**Styling** Theme (Light / Dark / Transparent), accent colour, base font size, content width,
padding, and left/centre alignment — all from the editor panel.

Raw HTML in the markdown is sanitized with DOMPurify before injection, so a control someone
can type into cannot execute script in the workbook.

**Editor-panel gotchas (from the SDK's own `index.d.ts`, which the skill docs contradict)**
`values` on `dropdown`/`radio` is `string[]` — a numeric value crashes Sigma's panel renderer
and surfaces as a Sentry error toast with a blank panel. `defaultValue` is a string. There is no
`description` property on any entry type. `getVariable` / `subscribeToWorkbookVariable` live on
`client.config`, not on `client`.

**Dependencies** `marked` 15.0.7 and `dompurify` 3.2.4 are vendored next to `index.html`
(no CDN dependency at runtime). The Sigma SDK's UMD build requires React on the global,
so `react` / `react-dom` 18.3.1 load before it — without them `window.SigmaPlugin` stays empty.
