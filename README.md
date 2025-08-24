# Conversion of SIMH Documentation to Markdown

This repository exists as a place to store
[OpenSIMH](https://opensimh.org)'s documentation as it is converted
from Microsoft Word to Markdown. The conversion is being done
partially using [Pandoc](https://pandoc.org), but generally requires
extensive hand cleanup of the Pandoc output to get a usable result.

**Pull requests against this repository are welcome.**

The hope is that these files will eventually be merged into
<https://github.com/open-simh/simh>, but this repository will be
maintained even if it is not merged into OpenSIMH.

## Status of Files Being Converted

These documentation files are considered to be in good shape. They
have been hand fixed following the Pandoc conversion but might still
contain proofreading errors:

| filename                      | documentation contents          |
|-------------------------------|---------------------------------|
| [gri_doc.md](docs/gri_doc.md) | GRI-909 simulator |
| [pdp8_doc.md](docs/pdp8_doc.md) | DEC PDP-8 simulator |
| [pdp10_doc.md](docs/pdp10_doc.md) | DEC PDP-10 (KS10) simulator |
| [pdp11_doc.md](docs/pdp11_doc.md) | DEC PDP-11 simulator |
| [simh.md](docs/simh.md)         | Writing a SIMH simulator |
| [simh_breakpoints.md](docs/simh_breakpoints.md) | SIMH Breakpoints |
| [simh_doc.md](docs/simh_doc.md) | SIMH User’s Guide |
| [simh_faq.md](docs/simh_faq.md) | SIMH FAQ |
| [simh_magtape.md](docs/simh_magtape.md) | SIMH Magtape docs |
| [simh_swre.md](docs/simh_swre.md) | SIMH Sample Software |
| [simh_vmio.md](docs/simh_vmio.md) | Adding an I/O device to SIMH |
| [simulators_acm_queue_2004.md](docs/simulators_acm_queue_2004.md) | ACM Queue article on SIMH |
| [ssem_doc.md](docs/ssem_doc.md) | SSEM simulator |
| [vax_doc.md](docs/vax_doc.md) | DEC VAX simulator |
| [vax780_doc.md](docs/vax780_doc.md) | DEC VAX 11/780 simulator |

Files that are nearly untouched. Generally, the raw output of Pandoc
is unlikely to be readable:

| filename                          | status        |
|-----------------------------------|---------------|
| [b5500_doc.md](docs/b5500_doc.md) | ❌ raw pandoc |
| [h316_doc.md](docs/h316_doc.md)   | ❌ raw pandoc |
| [h316_imp.md](docs/h316_imp.md)   | ❌ raw pandoc |
| [h316_imp_IO_Device_Codes.md](docs/h316_imp_IO_Device_Codes.md) | ❌ raw pandoc |
| [hp2100_doc.md](docs/hp2100_doc.md)     | ❌ raw pandoc |
| [hp3000_doc.md](docs/hp3000_doc.md)     | ❌ raw pandoc |
| [i1401_doc.md](docs/i1401_doc.md)       | ❌ raw pandoc |
| [i1620_doc.md](docs/i1620_doc.md)       | ❌ raw pandoc |
| [i650_doc.md](docs/i650_doc.md)         | ❌ raw pandoc |
| [i7010_doc.md](docs/i7010_doc.md)       | ❌ raw pandoc |
| [i701_doc.md](docs/i701_doc.md)         | ❌ raw pandoc |
| [i7070_doc.md](docs/i7070_doc.md)       | ❌ raw pandoc |
| [i7080_doc.md](docs/i7080_doc.md)       | ❌ raw pandoc |
| [i7090_doc.md](docs/i7090_doc.md)       | ❌ raw pandoc |
| [i7094_doc.md](docs/i7094_doc.md)       | ❌ raw pandoc |
| [ibm1130.md](docs/ibm1130.md)           | ❌ raw pandoc |
| [id_doc.md](docs/id_doc.md)             | ❌ raw pandoc |
| [ka10_doc.md](docs/ka10_doc.md)         | ❌ raw pandoc |
| [ki10_doc.md](docs/ki10_doc.md)         | ❌ raw pandoc |
| [kl10_doc.md](docs/kl10_doc.md)         | ❌ raw pandoc |
| [ks10_doc.md](docs/ks10_doc.md)         | ❌ raw pandoc |
| [lgp_doc.md](docs/lgp_doc.md)           | ❌ raw pandoc |
| [nova_doc.md](docs/nova_doc.md)         | ❌ raw pandoc |
| [pdp18b_doc.md](docs/pdp18b_doc.md)     | ❌ raw pandoc |
| [pdp1_doc.md](docs/pdp1_doc.md)         | ❌ raw pandoc |
| [pdp6_doc.md](docs/pdp6_doc.md)         | ❌ raw pandoc |
| [sds_doc.md](docs/sds_doc.md)           | ❌ raw pandoc |
| [sel32_doc.md](docs/sel32_doc.md)       | ❌ raw pandoc |
| [sigma_doc.md](docs/sigma_doc.md)       | ❌ raw pandoc |
| [swtp6800_doc.md](docs/swtp6800_doc.md) | ❌ raw pandoc |
| [tx0_doc.md](docs/tx0_doc.md)           | ❌ raw pandoc |


## Why this is needed at all.

The SIMH documentation has previously been kept in Microsoft Word
format.

Unfortunately, this format makes it difficult to view the
documentation online except as pre-formatted PDFs.

Word it also makes it hard to use modern code development practices;
it is not a "git friendly" format because:

- You cannot view changes to documentation under source control as
  diffs.
- You cannot automatically merge distinct development branches using
  source control.
- You cannot easily accept pull requests.

Easier development of SIMH's documentation requires that it be moved
to a plain text-based format that is both viewable online and which
can be maintained using modern source control.

Markdown is the most commonly used format for such documentation these
days, and so this effort is using GitHub Flavored Markdown.

### Why not use another format such as...

Markdown is imperfect, and there are certainly better formats that
allow better semantic markup and better control over the appearance of
the output.

However, Markdown works well enough, is very readable even without
formatting, can be viewed in its formatted form directly on GitHub
without requiring any tools at all, requires almost no effort for
newcomers to learn, and is in widespread use.
