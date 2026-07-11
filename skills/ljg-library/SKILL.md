---
name: ljg-library
description: "One book → one clear viewfinder (取景框) mental image → one 2050 library card (PNG). Viewfinder = the angle from which the author looked at a question, and the picture they saw; the card carries the real cover, author portrait, and bibliographic info. The viewfinder block uses a Feynman-style explanation (费曼式讲解) to make this mental image both accessible and accurate; the illustration block uses AI image generation to paint that same picture, with Jigang (继刚) as the fixed protagonist (generated from his ink-portrait reference, recognizable as him). Illustration style: Seiji Yoshida (吉田诚治)-style picture-book feel (isekai everyday spaces, warm slanting light, healing yet refined). Light optical-glass card body, accent color dynamically extracted from the cover, adaptive width/height. Close the book remembering this picture, and the reading wasn't wasted. Use when user says '取景框卡' (viewfinder card), '图书馆卡' (library card), 'library card', '书卡' (book card), '铸书卡' (forge a book card), '一本书一句话一张卡' (one book, one sentence, one card), '/ljg-library', or provides a book name and wants it distilled into one collectible card. NOT FOR structural book-breakdown analysis (use ljg-book), pure text quote cards (use ljg-card -b), infographics (use ljg-card -i), or visual notes (use ljg-card -v)."
user_invocable: true
version: "3.3.0"
---

# ljg-library: Viewfinder Library Card

One book forged into one 2050 library card. The cover, author, and bibliographic info are this card's identity; the real job is to distill the book's original viewfinder (取景框) into a mental image — the author looking at some question from a certain angle, seeing a picture that others hadn't seen. The text block explains this picture thoroughly; the illustration block paints it. Glance at the card six months later, the picture comes back, and the book wasn't read for nothing.

One card, two jobs:

- **Viewfinder block (text)**: the main sentence `{{FRAME}}` names this picture in one line, and the Feynman explanation `{{EXP}}` explains it to a smart layperson. `{{EXP}}` must be generated via the `feynman-eli5` skill, not hand-written.
- **Illustration block (image generation)**: hand the same picture to `assets/gen_illustration.py` to paint. Jigang (继刚) is the protagonist of every image — the script feeds his ink portrait (`assets/ljg-portrait.png`) as a character reference to the model, painting a recognizable him, letting him personally experience the picture's core action — whether acting or being acted upon. The style is Seiji Yoshida (吉田诚治)-style picture-book feel: isekai everyday spaces, warm slanting light, honey-amber-wood warm tones, solid architectural perspective, layered detail of plants/books/objects, healing settings like magic workshops/old bookstores/libraries, natural handwritten Chinese text within the scene, with Jigang as the small figure dwelling inside it. The style is hardcoded into the script — the frame only needs to describe what to paint.

The text and the image must depict the same picture. If the distillation misses, the card is just a Douban book-review card.

Output is PNG only, no text notes produced.

## Read two references before starting

- `references/extraction.md` — how to distill the mental image from the book, how to write the image-generation frame. Distillation is six steps: subject, angle, old picture, new picture, Feynman explanation, verification. If Jigang has already thought the idea through (forging a card right after finishing a book is the norm), use his directly: just verify and generate the image, don't re-distill.
- `references/visual.md` — card body spec, colors, fonts, factory self-check.

## Workflow

```
Input: book title (or book title + an already-thought-through viewfinder idea)
  ↓
1. Get the real cover + bibliographic info from WeRead (微信读书) (see "Sourcing materials")
2. Fetch the author's portrait from the web
3. Extract the cover's dominant color → card accent color (python3 assets/extract_color.py <cover>)
4. Distill the mental image (given by user → verify; not given → extraction.md six steps)
5. {{FRAME}} main sentence + {{EXP}} Feynman explanation (via feynman-eli5)
6. Write the frame (English composition: what Jigang is doing, metaphorical objects, information flow, 3-5 Chinese labels each ≤5 characters), generate the image:
   python3 assets/gen_illustration.py --frame "<...>" --out /tmp/ljg_lib_{slug}_sketch.png
7. Fill placeholder variables in assets/library_template.html ({{SKETCH_IMG}} = file:// of the generated image)
8. Render (capture.js, fullpage)
9. Read the finished PNG to verify with your own eyes; if unsatisfied, adjust the frame and regenerate, then deliver
```

## Sourcing materials

### Cover + bibliographic info

Jigang uses WeRead (微信读书); go through the weread skill's `/store/search` first (Read `~/.claude/skills/weread/search.md` before use):

```bash
curl -s -X POST "https://i.weread.qq.com/api/agent/gateway" \
  -H "Authorization: Bearer $WEREAD_API_KEY" -H "Content-Type: application/json" \
  -d '{"api_name":"/store/search","keyword":"<book title>","scope":10,"skill_version":"1.0.3"}'
```

The response's `results[].books[].bookInfo` (one book per result) contains `title / author / translator / cover / publisher`. The cover URL defaults to the `s_` thumbnail (70×100, which will be blurry once placed in the card); swap `s_` for `t7_` to get the 285×411 HD version: `.../cover/942/635942/t7_635942.jpg`. Download with `-H "Referer: https://weread.qq.com/"`.

When WeRead has no result, fall back by book type:

- Chinese books / Dedao-course-type books → Douban cover, use the `l/public` large-image path (`s/public` small images will be blurry)
- Foreign academic books → OpenLibrary: `https://openlibrary.org/search.json?q=<book title>` to get `cover_i`, then use the covers CDN `https://covers.openlibrary.org/b/id/<cover_i>-L.jpg`
- Neither works → CSS placeholder cover, don't block

### Author portrait

Wikipedia's original image is the most reliable; use the Wikipedia API to get the original image URL directly:

```bash
curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)" \
  "https://en.wikipedia.org/w/api.php?action=query&titles=<English name>&prop=pageimages&piprop=original&format=json"
# After getting .original.source, download it (also with UA):
curl -sL -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)" -o /tmp/ljg_lib_{slug}_avatar.jpg "<original-url>"
```

Note: the thumb path (`/thumb/.../480px-xxx.jpg`) returns an HTML error page when that size isn't cached — use the original image path (drop `/thumb/` and the size segment), and a User-Agent is required, or Wikimedia will block the request.

Fallback: no English Wikipedia entry → Wikipedia REST summary (`/api/rest_v1/page/summary/<name>`) → Chinese figures fall back to Baidu Baike → if none work, omit the portrait (the template's author line automatically hides the avatar), don't block.

### Protagonist reference image

`assets/ljg-portrait.png` is Jigang's real portrait, background-removed into an ink-style image; `gen_illustration.py` automatically feeds it as a character reference to the model. The generated result is him as painted by the model in Seiji Yoshida's style — recognizable is enough, pixel-perfect likeness isn't the goal. To update the portrait, just swap this png.

### Card accent color

```bash
python3 assets/extract_color.py /tmp/ljg_lib_{slug}_cover.jpg
# Output looks like: #c43d30
```

Extract the most prominent color from the cover as the card's accent color — change the book, the color changes automatically: red cover, red card; blue cover, blue card. Two cases need manual adjustment: when the cover is mostly a large beige/gray background, the script may pick a dull background color — reorder by "vividness × frequency" instead, and pick a color from the real pixels that can carry the card, don't hardcode one out of thin air; when the cover color is very bright, darken it half a notch so it still reads clearly on the light card body. The accent color only governs the card body (tags, English line, keyword highlights, signature stamp); the illustration panel's palette comes with the generated image — the two don't interfere with each other. Pick warm colors (brown, orange, green) to better match that warm-toned illustration.

## Template variables (library_template.html)

| Variable | Content |
|------|------|
| `{{ACCENT}}` | Card accent color hex (extracted from cover) |
| `{{COVER}}` | Absolute `file://` path of the cover |
| `{{AVATAR_IMG}}` | Full avatar `<img class="avatar" src="file://…">` (empty string if no avatar) |
| `{{TITLE}}` `{{EN}}` `{{SUBTITLE}}` | Chinese / English / subtitle of the book title |
| `{{TAGS}}` | 3-4 topic tags, each `<span class="tag">…</span>` |
| `{{AUTHOR_CN}}` `{{AUTHOR_META}}` | Author's Chinese name / "English name · publisher · year" |
| `{{FRAME}}` | Main sentence of the mental image, keywords colored with `<span class="hl">…</span>` |
| `{{EXP}}` | Feynman explanation (feynman-eli5 output), keywords colored the same way |
| `{{SKETCH_TITLE}}` | Illustration panel's name (English + Chinese, e.g. `Ergodicity 遍历性`) |
| `{{SKETCH_IMG}}` | Absolute `file://` path of the generated illustration |

## Rendering

```bash
node ~/.claude/skills/ljg-card/assets/capture.js \
  /tmp/ljg_library_{name}.html ~/Downloads/{name}.png 1080 1440 fullpage
```

Reuse ljg-card's capture.js (playwright is installed in ljg-card/node_modules). `fullpage` cannot be omitted — the card height follows the content, and omitting it leaves blank space at the bottom. Locally referenced covers/avatars via `file://` render directly.

## Delivery

1. Read the finished PNG to verify with your own eyes, zoom in on the illustration panel. Check against visual.md's factory self-check: cover loads, the explanation thoroughly conveys the picture, card color is harmonious, Jigang is recognizable, the picture reads at a glance, Chinese labels aren't blurry, text and image depict the same picture, no gap on the right side, no blank space at the bottom. If the generated image is unsatisfactory, adjust the frame and regenerate.
2. Report the file path, plus a sentence explaining how the mental image was distilled.

## Gotchas

- **Cover size**: WeRead's `s_` is a 70×100 thumbnail, guaranteed to be blurry. Swap for `t7_` (285×411), download with `Referer`.
- **Avatar thumb trap**: Wikimedia `/thumb/.../NNNpx-` returns an HTML error page when uncached. Use the original image path, with a User-Agent.
- **Always check generated images**: the same frame produces a different image every time — gemini can blur the Chinese labels, paint Jigang unrecognizably, or get the action wrong. Always Read and verify after each generation; if it's not right, adjust the frame and regenerate. Before regenerating, save the current image to a different path first — the new one isn't necessarily better than the old one, don't overwrite a usable one.
- **Chinese labels ≤5 characters**: long labels and rare characters tend to blur. Keep each label to 5 characters or fewer; let the body text carry mechanistic detail, don't expect the image to spell it out clearly.
- **Separate labels in the frame with semicolons or Chinese enumeration commas**: separating Chinese labels with ` / ` gets misjudged by the safety hook as a dangerous command and BLOCKed — don't use slashes.
- **The frame must be concrete and paintable**: write clearly the action Jigang is personally experiencing, the real metaphorical object, how information flows. Abstract propositions can't be painted — go back to the mental image and turn it into visible objects and actions (see extraction.md).
- **The protagonist is stylized, not chasing the exact same face**: generated from the ink-portrait reference, recognizable is enough. This is not a literal face-swap composite from the real portrait.
- **The two color systems don't cross**: the card-body accent color is extracted from the cover; the illustration panel's background comes with the generated image.
- **/tmp filenames carry a slug**: when forging cards in parallel, generated images, covers, and avatars should all use unique paths (carrying the book's slug) — sharing a fixed name will mix up images.
- **Batch pipeline**: `gen_illustration.py` calls marswave gemini-3-pro directly, bypassing listenhub's interactive gating, so ljg-paper-flow's batch card-forging can use it. Each image requires network access, spends API quota, and needs a manual visual check — slower than pure templates, leave slack when scheduling batch jobs.
- **{{FRAME}} describes how the view was changed, not a content summary**: the sentence pattern is "it's not X, it's actually Y," not "what this book is about." Test: remove the book title and author — if the sentence still stands on its own, the distillation succeeded.
