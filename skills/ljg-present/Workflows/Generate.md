# Generate Workflow

Forge Orgmode, Markdown, or plain text into an outline-faithful single-file HTML presentation.

## 1. Announce

```bash
curl -s -X POST http://localhost:31337/notify \
  -H "Content-Type: application/json" \
  -d '{"message":"Running the Generate workflow in the ljg-present skill"}' \
  >/dev/null 2>&1 &
```

Output: `Running the **Generate** workflow in the **ljg-present** skill to build an outline-faithful HTML presentation...`

## 2. Load the Contract

Read in full:

1. `RenderingSpec.md`
2. `SloganTemplate.html`

Don't rewrite the template from memory, and don't copy the last-generated HTML as a new template.

## 3. Read and Inventory the Source

- Read the entire input, not just the first few hundred lines.
- Extract title, subtitle/meta, and filetags.
- Build a stable `SRC-NNN` source manifest.
- Count headings, paragraphs, list items, quotes, tables, and examples.
- Record each source element's original text and order — this is the fidelity oracle used after generation.

For URL input, fetch the body text first; for local files, prefer reading them directly. If the complete content can't be fetched, stop and say so — never fill in gaps from a summary.

## 4. Resolve Theme

Priority: explicit argument > filetags > black.

| User intent | Theme |
|---|---|
| `-r`, `--theme=red` | red |
| `-b`, `--theme=black` | black |
| `-y`, `--theme=yellow` | yellow |
| `--hacker`, Hacker style | hacker |
| `--cyber` | hacker (compatibility alias) |
| dark Hacker, Hacker-dark | hacker-dark |
| `:share:`, `:talk:`, `:manifesto:`, `:keynote:` | red |
| `:critique:`, `:warn:`, `:rant:` | yellow |
| anything else | black |

If the user gives a custom visual direction, first translate it into a reading strategy (regular/cover/hl/structural lines), then change the theme variables — don't just pile on effect keywords.

## 5. Parse into Slides

Follow the mapping in `RenderingSpec.md`:

- The title cover is independent of the outline.
- A level-1 heading → emphasis.
- A level-2-or-deeper heading → a title page.
- Paragraphs, lists, quotes → lines.
- A table → `table`, with `header: true|false` written explicitly.
- example/fenced code → `pre`.
- Every slide gets `sourceIds`.

When paginating, only split at physical boundaries — never change the text. A consecutive, same-indent list is first treated as a single semantic run: 3–4 items stay together on one page; beyond 4 items, split by 3–4 per page, avoiding a lone trailing item. Quotes keep at most 2 original non-empty lines per page. Long paragraphs are split at existing sentence boundaries first. When one source is split across pages, record `sourceParts: [{id,index,total,joinBefore}]`, keeping the same font size, theme, and center axis.

When generating a single-line text page, identify "semantic atoms" (语义原子) before applying CJK-weighted length tiering: a complete short sentence that isn't a list item, isn't a whole-line formula, and has no more than 16 grapheme clusters after whitespace is stripped is marked `data-semantic-atom=true`. It must stay on one line, enter the Takahashi flow, and be scaled as a whole by measured fit — it must not be overridden by the generic line-wrap rules for quote/title/long. For long Chinese sentences that are allowed to wrap, protect the last three CJK characters plus punctuation with a tail span at the rendering layer that doesn't change the visible text, to avoid stranding a single character on its own line.

## 6. Build the HTML

Substitute the four placeholders from `SloganTemplate.html`. You must use a functional replacer (e.g. `.replace("{{SLIDES_JSON}}", () => safeJson)`) — never pass content directly as the replacement string, since the latter would interpret `$$` inside a formula as `$`, and would also interpret `$&`, ``$` ``, and `$'`:

| Placeholder | Value |
|---|---|
| `{{TITLE}}` | HTML-escaped document title |
| `{{SUBTITLE}}` | HTML-escaped meta, empty if absent |
| `{{THEME}}` | black/red/yellow/hacker/hacker-dark |
| `{{SLIDES_JSON}}` | safe JSON serialization |

Write to `~/Downloads/{title}.html`. Strip path characters from the filename, keep readable Chinese characters, and cap it at 40 characters.

## 7. Fidelity Audit

After building the complete HTML but before writing it out, first re-parse the final `RAW_SLIDES` from it, then immediately check:

1. The dedupe order of slides' first `sourceIds` matches the source manifest exactly.
2. Every source ID is referenced at least once.
3. Continuation pages of the same source are contiguous, `sourceParts` is strictly `1..total`, and reconstructing via `joinBefore` matches the source text character-for-character.
4. Table cell contents, example whitespace, and formula source text remain unchanged.
5. The cover title is correct, and the original first node still exists.
6. Every consecutive same-indent list run is paginated at a preferred size of `3–4`, without creating an artificial lone-trailing-item page; generated pages retain an auditable `semanticGroup`.

The audit target must be "the slides re-parsed from the final HTML," never just the in-memory object before template injection — otherwise drift caused by the replacement string, HTML escaping, or JSON safe-escaping would be falsely reported as faithful.

Any omission, duplicate consumption, or ordering drift must be fixed in the parser — never patched by appending pages at the end of the output.

## 8. Static Validation

```bash
bun Tools/ValidateDeck.ts ~/Downloads/{title}.html --theme <theme>
```

A failure stops delivery until it's fixed. Don't delete a rule the validator dislikes just to get a PASS — check whether the contract it's flagging has genuinely been broken.

## 9. Visual Verification

Open the local HTML with Interceptor's isolated test profile, run the four probes (DOM, console, network, screenshot), and check the representative pages listed in `RenderingSpec.md`.

- If the Interceptor gate fails: stop the browser workflow, keep the static validation results, and report "not yet browser-verified."
- Don't touch the Default profile.
- Don't substitute a headless browser, system screenshot tool, or the main browser.

## 10. Report

Return:

- The HTML's absolute path.
- Theme and page count.
- Source fidelity result.
- Validator result.
- Browser visual verification result, or the reason it was deferred.
- Page-turn keys: `→ ← ↑ ↓ Space PageUp PageDown F Home End`.
