# Conversion of SIMH Documentation to Markdown

This repository exists as a place to store
[OpenSIMH](https://opensimh.org)'s documentation as it is converted
from Microsoft Word to Markdown. The work is being done partially
using [Pandoc](https://pandoc.org), but in each case requires cleanup
by hand.

Pull requests against this repository are welcome. The hope is that
this work will eventually be merged into
https://github.com/open-simh/simh

## Why this is needed at all.

The SIMH documentation has traditionally been kept in Microsoft Word
format.

Unfortunately, this format makes it difficult to view the
documentation online. It also makes it hard to use modern code
development practices, like being able to view changes to
documentation under source control as diffs, or to easily accept pull
requests. Word also does not make it possible to automatically merges
of distinct development branches using source control.

Easier development of SIMH's documentation requires that it be moved
to a plain text-based format that is both viewable online and which
can be maintained using modern source control.

Markdown is the most commonly used format for such documentation these
days, and so this effort is using GitHub Flavored Markdown.

## Status of Files Being Converted

Files requiring fixes:


| filename                         | status      | comments              |
| -------------------------------- | ----------- | --------------------- |
| `b5500_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `gri_doc.md`                   | ❌ raw pandoc | Not yet hand repaired |
| `h316_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `h316_imp.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `h316_imp_IO_Device_Codes.md`  | ❌ raw pandoc | Not yet hand repaired |
| `hp2100_doc.md`                | ❌ raw pandoc | Not yet hand repaired |
| `hp3000_doc.md`                | ❌ raw pandoc | Not yet hand repaired |
| `i1401_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `i1620_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `i650_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `i7010_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `i701_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `i7070_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `i7080_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `i7090_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `i7094_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `ibm1130.md`                   | ❌ raw pandoc | Not yet hand repaired |
| `id_doc.md`                    | ❌ raw pandoc | Not yet hand repaired |
| `ka10_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `ki10_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `kl10_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `ks10_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `lgp_doc.md`                   | ❌ raw pandoc | Not yet hand repaired |
| `nova_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `pdp10_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `pdp11_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `pdp18b_doc.md`                | ❌ raw pandoc | Not yet hand repaired |
| `pdp1_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `pdp6_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `pdp8_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `sds_doc.md`                   | ❌ raw pandoc | Not yet hand repaired |
| `sel32_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `sigma_doc.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `simh.md`                      | ❌ raw pandoc | Not yet hand repaired |
| `simh_breakpoints.md`          | ❌ raw pandoc | Not yet hand repaired |
| `simh_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `simh_faq.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `simh_magtape.md`              | ❌ raw pandoc | Not yet hand repaired |
| `simh_swre.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `simh_vmio.md`                 | ❌ raw pandoc | Not yet hand repaired |
| `simulators_acm_queue_2004.md` | ❌ raw pandoc | Not yet hand repaired |
| `ssem_doc.md`                  | ❌ raw pandoc | Not yet hand repaired |
| `swtp6800_doc.md`              | ❌ raw pandoc | Not yet hand repaired |
| `tx0_doc.md`                   | ❌ raw pandoc | Not yet hand repaired |
| `vax780_doc.md`                | ❌ raw pandoc | Not yet hand repaired |
| `vax_doc.md`                   | ❌ raw pandoc | Not yet hand repaired |

