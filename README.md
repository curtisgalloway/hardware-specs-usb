<!--
SPDX-FileCopyrightText: 2026 contributors
SPDX-License-Identifier: CC-BY-4.0
-->

# hardware-specs-usb

Specs of the USB standard family, built only from public specifications and licensed
CC-BY-4.0: USB itself, the eXtensible Host Controller Interface (xHCI), USB Type-C, USB Power
Delivery (PD) and the Type-C Port Controller Interface (TCPCI). No spec here cites source code,
so you may use them whatever your own project's license is, with attribution.

A spec here records the generic facts of a standard (registers, capability structures,
protocol behavior) once. Board and SoC specs in the other spec repositories compose them
through `describes` and `instances[].ip` and record only how a chip integrates the standard.
The layout is the license-split design's
([LICENSE-SPLIT.md](https://github.com/curtisgalloway/driver-lab/blob/main/docs/LICENSE-SPLIT.md#standards-and-architecture-specs-154)).

**Terms.** A *spec* is a hardware description that driver authors, people or AI agents, read
instead of the original sources. A *fact* is one record in it: a claim with a stable id and its
*support*, the citations it rests on. `specs/board-specs.yaml` is this repository's *root
marker*: it states the repository's license and its *accepts list*, the licenses a cited source
may carry (none here: documents only). A *verification record*
(`specs/resources/<name>.verify.yaml`) holds a verdict per fact. The *corpus ledger*
(`specs/corpus.yaml`) lists every document and block of the family found so far, each with a
disposition: specified, planned or excluded. Full definitions: driver-lab's
[glossary](https://github.com/curtisgalloway/driver-lab/blob/main/GLOSSARY.md).

## The specs

| Spec | Revision | Access class | Composed by |
|---|---|---|---|
| [`xhci`](specs/xhci.spec.yaml): eXtensible Host Controller Interface | xHCI 1.2c (October 2025) | `public` | none yet |
| [`ehci`](specs/ehci.spec.yaml): Enhanced Host Controller Interface (USB 2.0) | EHCI 1.0 (March 2002) | `public` | none yet |
| [`usb-type-c`](specs/usb-type-c.spec.yaml): Type-C connection detection, accessories, VPDs and cable eMarkers | Type-C Cable and Connector Release 2.5 (March 2026) | `public` | none yet |
| [`usb2`](specs/usb2.spec.yaml): USB 2.0 protocol and device framework | USB 2.0 (April 2000, base specification) | `public` | none yet |
| [`usb-bc`](specs/usb-bc.spec.yaml): USB Battery Charging (BC 1.2) port detection | BC 1.2 (March 2012, with errata) | `public` | none yet |
| [`usb2-ecns`](specs/usb2-ecns.spec.yaml): USB 2.0 ECNs (Connect Timing, LPM, IAD) | USB 2.0 ECNs (Connect Timing, Link Power Management, Interface Association Descriptor) | `public` | none yet |
| [`eusb2`](specs/eusb2.spec.yaml): Embedded USB2 (eUSB2V2) physical and link layer supplement | eUSB2 Version 2.0 Supplement, Version 1.0 (August 2024) | `public` | none yet |
| [`ucsi`](specs/ucsi.spec.yaml): USB Type-C Connector System Software Interface | UCSI Revision 3.1 (June 2026) | `public` | none yet |
| [`tcpci`](specs/tcpci.spec.yaml): USB Type-C Port Controller Interface (TCPCI) | TCPCI Revision 2.0, Version 1.3 (January 2022) | `public` | none yet |

The views (a Markdown copy and an HTML viewer of each spec) are built by CI and published to
https://curtisgalloway.github.io/hardware-specs-usb/ from `main`; nothing generated is
committed here. A pull request's views are in its `spec-views` artifact.

`specs/corpus.yaml` lists what is planned next: the USB PD specification, and the
USB 3.2 and USB4 protocol specifications.
Protocol data structures (descriptor layouts, request tables) are written as prose claims for
now; driver-lab#164 tracks a spec kind that would carry them as structured layouts.

## How other spec repositories use these specs

A SoC or board spec names a spec here from an instance row (`instances[].ip: xhci`), and its
repository reads this one as a context root. `board-expert` finds this repository the same way
it finds any spec root: list this checkout's `specs/` directory under `roots:` in your
board-specs configuration marker (`~/.config/board-specs/board-specs.yaml`) or in a project's
marker. A protocol question about an xHCI controller is then answered from these facts, not
from memory.

## What a spec here may cite

Documents only (`accepts: []`), listed under `resources.documents` and cited by name with a
page, section, table or clause. By the document's `access` class: `public` and `registration`
documents are cited as usual (for `registration`, the URL is recorded, never a copy);
`members-only` documents get a map only (`cite: false`); `private` documents never. Document
bytes are never committed here: each entry records the URL and SHA-256 a reader fetches and
checks.

## What is here

- `specs/`: the spec root. `board-specs.yaml` is the root marker, `corpus.yaml` the corpus
  ledger; each spec is `<name>.spec.yaml`, its verification record
  `resources/<name>.verify.yaml`, and `index/<document>.index.yaml` is a generated section
  index of a cited document (headings and page ranges, no document text).
- `LICENSE`: the license.
- `LICENSES/Apache-2.0.txt`: the license of the CI files.
- `.github/workflows/checks.yml` and `scripts/checks.sh`: the checks below.
- `.github/workflows/publish.yml`: builds the published views.

## How a spec is written and checked

The method lives in [driver-lab](https://github.com/curtisgalloway/driver-lab). The contract
for specs, the root marker and verification records is the
[`spec-format`](https://github.com/curtisgalloway/driver-lab/blob/main/skills/spec-format/SKILL.md)
skill; [`spec-verifier`](https://github.com/curtisgalloway/driver-lab/tree/main/skills/spec-verifier)
writes the verification records. CI checks out driver-lab at one pinned commit, installs
`spec.py`'s hash-pinned dependencies, and runs, through `scripts/checks.sh`:

1. `check`: `spec.py check specs --require-license --require-verified <mode>`, then
   `spec.py corpus specs` (the step sets `REQUIRE_CORPUS=1`). Pull requests use mode `pr`: a
   fact with no verdict or a stale one fails, so nothing merges unverified. `main` uses mode
   `main`.
2. `resolve`: `spec.py resolve` on every spec.
3. `render`: the Markdown view and the viewer of every spec.
4. `self-test`: proves the license gate with this repository's own root marker on driver-lab's
   fixture specs; specs citing source must fail, documents-only specs pass.

To run the same checks locally:

```bash
git clone https://github.com/curtisgalloway/driver-lab ../driver-lab
python3 -m venv .venv-spec
.venv-spec/bin/pip install --require-hashes -r ../driver-lab/skills/spec-format/requirements.txt
CHECKS_PYTHON=.venv-spec/bin/python RESOLVE_SRC=1 REQUIRE_CORPUS=1 scripts/checks.sh all ../driver-lab
```

Check out the driver-lab commit that `.github/workflows/checks.yml` pins for an identical run.
`resolve` checks document citations against the document bytes only when they are supplied:
run `spec.py resolve specs/<name>.spec.yaml --root specs --docs-dir <dir>` with each document
saved as `<dir>/<name>` (its `resources.documents` name) after checking its SHA-256.

Other spec repositories read this one as a context root, so this repository's driver-lab pin
(`TOOL_COMMIT` in `checks.yml` and `publish.yml`) must never be newer than theirs, or their
checks may reject it: bump the repositories that read this one first, or together.

## License

The specs, their verification records, the corpus ledger and documentation are licensed
CC-BY-4.0 ([LICENSE](LICENSE)). The CI files (`.github/workflows/checks.yml`,
`.github/workflows/publish.yml`, `scripts/checks.sh`) are licensed Apache-2.0
([LICENSES/Apache-2.0.txt](LICENSES/Apache-2.0.txt)). Each file states its license in an
`SPDX-License-Identifier` line.
