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
(`specs/resources/<name>.verify.yaml`) holds a verdict per fact. Full definitions: driver-lab's
[glossary](https://github.com/curtisgalloway/driver-lab/blob/main/GLOSSARY.md).

## The specs

None yet.

| Spec | Revision | Access class | Composed by |
|---|---|---|---|

## What a spec here may cite

Documents only (`accepts: []`), listed under `resources.documents` and cited by name with a
page, section, table or clause. By the document's `access` class: `public` and `registration`
documents are cited as usual (for `registration`, the URL is recorded, never a copy);
`members-only` documents get a map only (`cite: false`); `private` documents never.

## What is here

- `specs/`: the spec root. `board-specs.yaml` is the root marker; each spec will be
  `<name>.spec.yaml`, and its verification record `resources/<name>.verify.yaml`.
- `LICENSE`: the license.

CI (`checks.yml`, `publish.yml`, `scripts/checks.sh`, as in
[hardware-specs-docs](https://github.com/curtisgalloway/hardware-specs-docs)) arrives with the
first spec.

## License

Everything here is licensed CC-BY-4.0 ([LICENSE](LICENSE)); each file states it in an
`SPDX-License-Identifier` line.
