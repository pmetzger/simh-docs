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

Files requiring fixes but considered in good shape:

| filename                        | status     | comments                     |
|---------------------------------|------------|------------------------------|
| [gri_doc.md](docs/gri_doc.md)   | unverified | Hand repaired, not proofread |
| [pdp10_doc.md](docs/pdp10_doc.md) | unverified | Hand repaired, not proofread |
| [pdp11_doc.md](docs/pdp11_doc.md) | unverified | Hand repaired, not proofread |
| [simh.md](docs/simh.md)         | unverified | Hand repaired, not proofread |
| [simh_breakpoints.md](docs/simh_breakpoints.md) | unverified | Hand repaired, not proofread |
| [simh_doc.md](docs/simh_doc.md) | unverified | Hand repaired, not proofread |
| [simh_faq.md](docs/simh_faq.md) | unverified | Hand repaired, not proofread |
| [simh_magtape.md](docs/simh_magtape.md) | unverified | Hand repaired, not proofread |
| [simh_swre.md](docs/simh_swre.md) | unverified | Hand repaired, not proofread |
| [simh_vmio.md](docs/simh_vmio.md) | unverified | Hand repaired, not proofread |
| [simulators_acm_queue_2004.md](docs/simulators_acm_queue_2004.md) | unverified | Hand repaired, not proofread |
| [ssem_doc.md](docs/ssem_doc.md) | unverified | Hand repaired, not proofread |
| [vax_doc.md](docs/vax_doc.md) | unverified | Hand repaired, not proofread |
| [vax780_doc.md](docs/vax780_doc.md) | unverified | Hand repaired, not proofread |


Files that are nearly untouched:


| filename                         | status      | comments              |
| -------------------------------- | ----------- | --------------------- |
| [b5500_doc.md](docs/b5500_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [h316_doc.md](docs/h316_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [h316_imp.md](docs/h316_imp.md) | ❌ raw pandoc | Not yet hand repaired |
| [h316_imp_IO_Device_Codes.md](docs/h316_imp_IO_Device_Codes.md) | ❌ raw pandoc | Not yet hand repaired |
| [hp2100_doc.md](docs/hp2100_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [hp3000_doc.md](docs/hp3000_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [i1401_doc.md](docs/i1401_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [i1620_doc.md](docs/i1620_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [i650_doc.md](docs/i650_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [i7010_doc.md](docs/i7010_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [i701_doc.md](docs/i701_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [i7070_doc.md](docs/i7070_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [i7080_doc.md](docs/i7080_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [i7090_doc.md](docs/i7090_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [i7094_doc.md](docs/i7094_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [ibm1130.md](docs/ibm1130.md) | ❌ raw pandoc | Not yet hand repaired |
| [id_doc.md](docs/id_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [ka10_doc.md](docs/ka10_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [ki10_doc.md](docs/ki10_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [kl10_doc.md](docs/kl10_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [ks10_doc.md](docs/ks10_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [lgp_doc.md](docs/lgp_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [nova_doc.md](docs/nova_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [pdp18b_doc.md](docs/pdp18b_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [pdp1_doc.md](docs/pdp1_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [pdp6_doc.md](docs/pdp6_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [pdp8_doc.md](docs/pdp8_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [sds_doc.md](docs/sds_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [sel32_doc.md](docs/sel32_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [sigma_doc.md](docs/sigma_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [swtp6800_doc.md](docs/swtp6800_doc.md) | ❌ raw pandoc | Not yet hand repaired |
| [tx0_doc.md](docs/tx0_doc.md) | ❌ raw pandoc | Not yet hand repaired |
