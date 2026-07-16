# Open Item 1 — Sauvage EDP lineKey verification (2026-07-16)

Status: **DIAGNOSED from production data — final byte-level confirmation still requires the live
`lineKey()` source from the n8n workflow (not reachable from this environment).**
Recommendation: **keep the batch HELD.** Evidence points to outcome (a) — normalization gap.

## Environment caveat (read first)

This session ran in a remote container scoped to `faraazshaikh/coursera-test`, which is an
**empty repository** — no HANDOVER.md, no workflow backups, and no n8n API key are present here.
An attempt to attach the likely working repo (`intelisenselabs`) was blocked by the platform
permission layer. Everything below is derived from the production Google Sheet
(`RLO_Used_Products`, ID `1V1TFN7BKebN74FLd5O9u4XXoZJDlQb8tlOMy_PSvaAQ`), which was fully
readable: all 316 Sheet1 rows, the `state` tab, and all `log` rows including `action=rejected`.

## 1. The exact item (task step 1 — complete)

Sheet1 row 268, batch **2026-07-11**, status still `pending`:

- Product: `Dior Sauvage Eau de Parfum 100ml Spray`
- URL: `https://redlabeloutlet.co.uk/collections/fragrance-for-men/products/dior-sauvage-eau-de-parfum-100ml-men-spray`
- Shopify handle: **`dior-sauvage-eau-de-parfum-100ml-men-spray`**

## 2. The rejected-lines log (task step 2 — complete)

All `action=rejected` rows in the `log` tab (Sauvage-relevant):

| Date | Handle |
|---|---|
| 07-02, 07-04 ×2 | `dior-sauvage-eau-de-toilette-100ml-men-spray` |
| 07-02, 07-04 | `dior-sauvage-eau-de-toilette-60ml-men-spray` |
| 07-04 | `dior-sauvage-parfum-100ml-men-spray` |
| **07-11** | **`dior-sauvage-eau-de-parfum-100ml-men-spray`** ← the EDP itself was rejected after it surfaced |

Non-Sauvage rejections (07-04): Designer French Collection Victory M, Nikos Sculpture EDP,
CR7 3-piece gift set.

## 3. Behavioral evidence from the batches

- Batches 06-01 → 07-04 repeatedly served Sauvage EDT 100ml (7 appearances) → rejections on
  07-02/07-04 → lineKey exclusion presumably went live after 07-04.
- **Batch 07-11**: contains the EDP, but NOT EDT 100ml, NOT EDT 60ml, NOT plain Parfum — the
  previously always-recurring EDT variants are gone. The exclusion was therefore active and
  filtering the `dior-sauvage` key; only the EDP slipped through. This is the signature of a
  **key mismatch for the EDP handle specifically**, not a dead exclusion.
- **Batch 07-13 anomaly (new finding)**: row 281 is `dior-sauvage-eau-de-toilette-60ml-men-spray`
  — the *exact handle* rejected on 07-02 and 07-04. Same-handle recurrence cannot be a regex gap.
  This batch also has intra-batch duplicates (Creed Aventus 50ml twice, Chanel No.5 twice under
  two different handles) and mixed URL shapes (`/products/...` without collection prefix on rows
  298–303). This is consistent with the pre-fix Google Sheets **429 read failures** returning an
  empty/partial exclusion list during that START — the same failure class the batchGet
  consolidation fix addressed. The first post-fix START (07-14, exactly 12 products, rows
  305–316) contains **zero** rejected lines and zero duplicates.
- Batch 07-03 row 195 (`dior-sauvage-eau-de-toilette`, size-less handle) recurring after the
  07-02 rejection dates from before the exclusion existed.

## 4. lineKey() analysis (task step 3 — done against reconstructed logic; live source still needed)

Per the documented spec (strip concentration/size/spray suffixes; keep formulations like Elixir
separate), the intended key for **all four** Sauvage variants is `dior-sauvage` → the EDP
**should** have been excluded → outcome (a).

The observed pass-through has a classic, exactly-matching cause: a strip list that contains
`parfum` (needed for the plain-Parfum handle) but does not consume `eau-de-parfum` as a whole
token (missing from the list, or ordered after `parfum` in the alternation / sequential
`.replace()` chain). Result, verified by simulation on the real handles:

| Handle | Correct (longest-first) | Buggy (`parfum` before `eau-de-parfum`) |
|---|---|---|
| `...eau-de-parfum-100ml-men-spray` | `dior-sauvage` | **`dior-sauvage-eau-de`** ← distinct key, not excluded |
| `...eau-de-toilette-100ml-men-spray` | `dior-sauvage` | `dior-sauvage` (excluded ✓) |
| `...eau-de-toilette-60ml-men-spray` | `dior-sauvage` | `dior-sauvage` (excluded ✓) |
| `...parfum-100ml-men-spray` | `dior-sauvage` | `dior-sauvage` (excluded ✓) |
| `...elixir-60ml...` (control) | `dior-sauvage-elixir` | `dior-sauvage-elixir` (stays distinct ✓) |

The `dior-sauvage-eau-de` residue exactly reproduces production: EDT + Parfum filtered from the
07-11 batch, EDP not.

## 5. Proposed regex fix (pending source confirmation)

Single alternation, **longest-first**, whole-token anchored — replaces any sequential
`.replace()` chain or shorter-first ordering:

```js
h = h
  .replace(/-(extrait-de-parfum|eau-de-parfum|eau-de-toilette|eau-de-cologne|eau-fraiche|parfum|edp|edt|edc|cologne)(?=-|$)/g, '')
  .replace(/-\d+ml(?=-|$)/g, '')
  .replace(/-(men|women|unisex|for-men|for-women|for-unisex|spray|perfume)(?=-|$)/g, '')
  .replace(/-\d+$/, '');   // Shopify duplicate-handle suffix (-1)
```

Deliberately NOT stripped: `elixir`, `intense`, `noir`, `absolu` etc. — distinct formulations
stay distinct per the architecture invariant. Harness fixtures must include all six handles
above plus an Elixir control asserting key inequality.

## 6. Verdict and required next steps

1. **Outcome (a) — normalization gap** is the strongly supported conclusion. The batch stays
   HELD. Independently of the regex question, the 07-11 batch cannot be approved as-is: its
   Sauvage EDP row (268) was rejected by the user on 07-11 yet is still `pending` in Sheet1,
   and 07-13 row 281 is a literal rejected handle, also still `pending`.
2. **Silver lining**: the EDP's own handle is now in the rejected log (07-11), so even the
   buggy key (`dior-sauvage-eau-de`) will self-exclude going forward. The remaining exposure is
   any *not-yet-rejected* EDP-suffixed variant of a rejected line (e.g. a future
   `dior-sauvage-eau-de-parfum-60ml-...` → key `dior-sauvage-eau-de`... which ironically matches
   the rejected EDP's buggy key — but any other line rejected only as EDT would not cover its
   EDP sibling). The gap is real and should be fixed.
3. **To close to deploy standard** (must run from an environment with the n8n API key, e.g. the
   Windows client): GET workflow `QxrhUedwDboEXkEW`, locate the node containing `lineKey` by
   name, extract the function, run the four handles through it verbatim, confirm the EDP key
   diverges, then patch per the standard deploy pattern (backup → patch jsCode only → deactivate
   → PUT with `{executionOrder:"v1"}` → activate → fetch-back byte-compare) with QC regression
   fixtures from §5.
4. Cleanup alongside the fix: mark rows 268 (Sauvage EDP) and 281 (Sauvage EDT 60ml) so they
   cannot be served (or rely on the exclusion once verified); investigate why the 07-13 START
   produced 24 rows with duplicates — expected to be pre-fix 429 fallout, worth confirming in
   n8n execution logs for that date.
