# content-compose-letter-example

A worked example of a **composed content project** built with
[throughline-compose](https://github.com/timebacksolutions/throughline-compose), and the
**sibling** of
[content-compose-example](https://github.com/timebacksolutions/content-compose-example).

That project is a council **rent-statement help web page**. This one carries the same
council message to the same lay tenant, but as a **posted rent-arrears letter**. It is
the *sibling-swap* demo: five of the six composed axes are identical, and only the
**medium** changes.

| Namespace | Source | Axis | Same as the web page? |
|---|---|---|---|
| `plain` | [throughline-plain-language](https://github.com/timebacksolutions/throughline-plain-language) `@v2026-07` | readability | ✅ identical |
| `conventions` | [throughline-conventions-uk](https://github.com/timebacksolutions/throughline-conventions-uk) `@v2026-07` | British-English conventions | ✅ identical |
| `tone` | [throughline-tone-formal](https://github.com/timebacksolutions/throughline-tone-formal) `@v2026-07` | register (formal) | ✅ identical |
| `purpose` | [throughline-purpose-instruct](https://github.com/timebacksolutions/throughline-purpose-instruct) `@v2026-07` | purpose (instruct) | ✅ identical |
| `audience` | [throughline-audience-general](https://github.com/timebacksolutions/throughline-audience-general) `@v2026-07` | audience (general reader) | ✅ identical |
| `medium` | [throughline-medium-letter](https://github.com/timebacksolutions/throughline-medium-letter) `@v2026-07` | medium (**letter**) | ❌ **web → letter** |

## The one axis that changed

The council still wants the message readable, correctly styled, in the formal register,
doing the instruct job and pitched at a lay tenant — so readability, conventions, tone,
purpose and audience are composed **unchanged** from the web page. The channel is what
differs: a web page is scanned on screen with links to click; a letter is read straight
through on paper with nothing to click. So everything the channel imposes changes, and
only that:

- it opens with an **address block, date, rent-account reference and salutation**, and
  closes with a **sign-off** (`medium:SR-0007`, `medium:SR-0008`) — a web page has none
  of this;
- it writes the **telephone number, office address and reference out in full** because
  there is nothing to click (`medium:SR-0005`), where the page would link and offer a
  pay button;
- it is a single **linear, self-contained read** (`medium:SR-0001` to `medium:SR-0004`),
  where the page is broken into scannable sections;
- it is presented for a **plain printed page** (`medium:SR-0009`, `medium:SR-0010`).

Swapping the channel was a **one-line change** in `throughline.toml`: the `medium`
source's `url` points at `throughline-medium-letter` instead of `throughline-medium-web`.

## The seam between medium and tone: the sign-off

The sign-off is where two axes meet, and the graph keeps them separate. That the letter
**has** a "Yours sincerely" close at all is a **medium** decision (`medium:SR-0008`); that
it reads *"Yours sincerely"* rather than *"Cheers"* is a **tone** decision
(`tone:SR-0011`). The letter's `SR-0003` cites **both**, side by side — the channel owns
the slot, the register fills it. A web page composes neither, because it has no sign-off.

## The authored letter

The requirements graph is the *spec*; the letter it governs is
[`content/rent-arrears-letter.md`](content/rent-arrears-letter.md). Read it against the
graph and see each composed axis bite: the address-block framing and sign-off of the
letter channel, the formal salutation and valediction, the general-reader gloss of
*arrears*, the GOV.UK amount and date style (£318.60, 1 August 2026), and the
uncontracted formal register throughout.

## How it's wired

- The project's own graph lives under `intents/`, `user-requirements/` and
  `system-requirements/`. Each letter requirement **grounds** through `implements` →
  `UR-0001` → `derives_from` → `INT-0001` (its own throughline), and **separately**
  `satisfies` the borrowed
  `plain:`/`conventions:`/`tone:`/`purpose:`/`audience:`/`medium:` clause it honours.
- `satisfies` is a traceability link, not a grounding link — so a letter requirement
  still justifies itself through its own intent, not through a borrowed standard.

## Running it

```sh
tl-compose check --strict     # fetches all six pinned sources, merges, validates
tl-compose trace SR-0003      # show the sign-off requirement across the medium and tone axes
```

Drive this project with `tl-compose`, never bare `tl`: bare `tl` fails fast the moment
it meets a namespace-qualified reference (`medium:SR-0008`) it cannot resolve, because
only the composition-aware tool fetches and merges the sources.
