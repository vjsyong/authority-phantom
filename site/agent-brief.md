# Agent brief: build under the Jennu authority

Authority repo: `authority-phantom` (v0.2.0, format 0.1). The Jennu design authority — the interface system of zhenyoyo.github.io as deployed: Source Sans Pro 300/700/900, pink reading ink #ff6bbc with neon-green links #6bff2c on a dotted blue underline, blue-ground code, a lime slide-in menu, red checked marks with purple labels, radius flattened to 0 on marks/boxes/images/chips, ink-ring buttons, underline fields, a tiles-only load-in and one interaction pink #f2849e. Contrast failures are recorded as measured observations (pass 2 extracted what the site IS; pass 1's grey-ink canon was reversed). Per-page black-body overrides are part of the system.

Reference build (what "look like this" means for this authority):
  https://designauthority.seanyong.xyz/authorities/phantom/site/
Every recorded selector, state and note, in one directory:
  https://designauthority.seanyong.xyz/authorities/phantom/site/#artefacts

Build an interface that conforms to THIS authority alone.

## Setup (public repos, MIT; stdlib-only CLI)

Get the pack - either route works; the zip also carries this authority's
reference build, fonts and manifests:

Route A - bundle zip (one download):

    curl -fsSL -o phantom-site.zip https://designauthority.seanyong.xyz/authorities/phantom/site/download/phantom-site.zip
    python3 -c "import zipfile; zipfile.ZipFile('phantom-site.zip').extractall('phantom-authority')"

Route B - git (the pack is its own repository):

    git clone --depth 1 https://github.com/vjsyong/authority-phantom.git /tmp/authority-phantom

Tooling (same for both routes):

    git clone --depth 1 https://github.com/vjsyong/design-authority.git /tmp/design-authority
    cd /tmp/design-authority
    export PACK=/tmp/authority-phantom      # Route B
    # or, from where you unzipped:  export PACK="$PWD/phantom-authority/pack"   # Route A

Sanity check (prints the authority overview):

    python3 tools/da.py --pack "$PACK" overview

## The loop, for every design decision

1. Resolve each need in natural language:
   python3 tools/da.py --pack "$PACK" resolve "primary button" --json
2. Inspect every record before adopting it:
   python3 tools/da.py --pack "$PACK" inspect <id>
3. ADOPT THE RECORDED SELECTOR along with the recorded values: the
   verification contract below addresses elements by their recorded class
   names (.cta, .card, .dlg, .ledger, .badge, ...). Keep those names on the
   elements you build; if you must deviate, declare it in verify.map.json
   (see Verify below).
4. Adopt only records shipped by this authority. Never borrow another's
   components, values or classes.
5. When the authority is silent: build from the nearest recorded pieces, keep
   the improvisation visible (an HTML comment plus data-improv="<reason>"),
   and file it:
   python3 tools/da.py --pack "$PACK" gap-add --need "<need>" \
     --context '{"source":"<your app>"}' --workspace .design-authority

## Verify before you claim done

The pack ships a verification contract (`verification.json`): mechanically
checkable assertions for this authority's records (computed-style, DOM,
static, interaction). It is a separate layer from `validators` (the lint
path, which may be empty). Run it over your build:

    python3 tools/da_verify.py --pack "$PACK" --target <your-app-dir> --out .verify

Reading the result: PASS / VIOLATION (fix it) / UNVERIFIABLE (could not run,
usually a missing selector or file) / N/A (declared ignore) / REVIEW_REQUIRED
(human item). Browser-backed checks need Playwright:

    python3 -m venv .venv && .venv/bin/pip install playwright && .venv/bin/playwright install chromium
    .venv/bin/python3 tools/da_verify.py --pack "$PACK" --target <your-app-dir> --out .verify

If your build cannot keep a recorded selector (or uses different file names),
declare it in `verify.map.json` in your app root:

    {"files": {"css": ["styles.css"], "html": ["index.html"]},
      "selectors": {".dlg": ".dialog"},
      "ignore": {"authority/check-id": "why this build is exempt"}}

## House rules

- Quote recorded values (colours, sizes, radii, type) from the records; never
  invent values that a record can give you.
- Copy selectors and states from the artefacts directory, not from memory.
- Plain HTML/CSS is enough; the authority requires no framework.
- An agent's own report is evidence, not proof. Re-read the artifacts, then
  run the verification contract.

## Take it away

- Download the reference build in one file (page, styles, fonts, the pack, the
  full audit trail):
  https://designauthority.seanyong.xyz/authorities/phantom/site/download/phantom-site.zip
- Inside the zip, `site/` is the shipped build and `pack/` is the same authority
  data this CLI reads; `site/MANIFEST.md` lists sha256 hashes for everything.
- `quickstart.sh` wires the pack and prints the first commands.
