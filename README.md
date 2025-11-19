# DSF - Det Sentrale Folketrygdsystemet

Dette systemet var et av de eldste IT-systemene i NAV, og kjørte
i produksjon i 51 år, helt fra folketrygden ble innført i 1967.
I januar 2018 ble [Presys](https://github.com/navikt/presys) satt i produksjon,
som sammen med Pesys ble arvtager til DSF.
Da ble også [DSF slått av](https://memu.no/kort-fortalt/pensjonert-it-dinosaur/).

Systemet er skrevet i [PL/I](https://en.wikipedia.org/wiki/PL/I).

---
## Comment 
I initiated this via a github Coding Agent task to document this extraordinary achievement in software engineering which;
- Sustained over 5 decades through technological change
- Served an entire nation reliably and accurately
- Adapted continuously to legislative and policy changes
- Was successfully replaced by modern systems without disruption

This documentation preserves the knowledge and lessons from this remarkable system for future generations. 
The documentation transforms this historic system from an intimidating black box into an understandable, "maintainable" codebase. Future developers can now:

- Understand the system architecture quickly
- Navigate 717 programs with confidence
- Learn from a successfully maintained 51-year system

This repo honors the developers who built DSF and supports those who will learn from it. The system served Norway's entire population reliably for over half a century, it deserves comprehensive documentation!

The maintainer of this repo take no personal responsibility on the use of, or any errors herein! 

---

## 📚 Documentation / Dokumentasjon

This repository now includes comprehensive documentation for understanding, maintaining, and (as educational excercise) modernizing the DSF system:

### Getting Started
- **[Architecture Overview](docs/ARCHITECTURE.md)** - High-level system design and technology stack
- **[Developer Guide](docs/DEVELOPER_GUIDE.md)** - Onboarding guide for new developers
- **[Glossary](docs/GLOSSARY.md)** - Norwegian terms and abbreviations explained

### System Structure
- **[Module Structure](docs/MODULE_STRUCTURE.md)** - Detailed breakdown of all 700+ programs organized by function
- **[Visual Diagrams](docs/DIAGRAMS.md)** - Architecture diagrams, data flows, and module relationships

## 🏗️ System Overview

**DSF (Det Sentrale Folketrygdsystemet)** - The Central National Insurance System:

- **717 PL/I source files** organized into functional modules
- **51 years of operational history** (1967-2018)
- Processed **all Norwegian pensions**, disability benefits, and survivor benefits
- Built on **IBM Mainframe** with CICS transaction processing
- A remarkable achievement in sustained software engineering

### Key Modules

| Module | Files | Purpose |
|--------|-------|---------|
| **R0010** | 91 | System menu, navigation, and transaction management |
| **R0011** | 81 | Person registration and data entry |
| **R0012** | 8 | Batch processing and mass calculations |
| **R0013** | 7 | Core pension calculation engines |
| **R0014** | 118 | Disability pension (largest module) |
| **R0015** | 59 | Old age pension processing |
| **R0016** | 22 | Survivor benefits (widows, widowers, orphans) |
| **R0017** | 23 | Occupational injury compensation |
| **R0018** | 15 | Special case processing |
| **R0019** | 93 | Queries, reports, and information retrieval |

## 🎯 Purpose of This Documentation

This comprehensive documentation was created to:

1. **Preserve institutional knowledge** - Document 51 years of business logic and policy
2. **Enable modernization** - Provide roadmap for refactoring to modern architecture
3. **Support learning** - Help developers understand this historic system
4. **Show empathy** - Make the codebase accessible to future maintainers

As the request noted: "docs are a sign of empathy!" 💙

## 🚀 Quick Start for Developers

1. **Read** the [Architecture Overview](docs/ARCHITECTURE.md) to understand the system
2. **Review** the [Module Structure](docs/MODULE_STRUCTURE.md) to see how code is organized
3. **Study** the [Developer Guide](docs/DEVELOPER_GUIDE.md) for detailed onboarding
4. **Reference** the [Glossary](docs/GLOSSARY.md) when encountering Norwegian terms
5. **Visualize** with [Diagrams](docs/DIAGRAMS.md) to see system relationships

## 📊 System Statistics

- **Total source files**: ~1,481 (including utilities and maps)
- **PL/I programs**: 717 main programs
- **Lines of code**: Hundreds of thousands (exact count TBD)
- **Largest program**: R0010410.pli (468KB) - Transaction builder
- **Data per person**: 62,608 bytes master record
- **Date range**: 1967-2014 (income data spans 47+ years per person)

## 🔧 Technology Stack

- **Language**: PL/I (Programming Language One)
- **Transaction System**: CICS (Customer Information Control System)
- **Database**: DB2 and VSAM
- **Screen Management**: BMS (Basic Mapping Support)
- **Platform**: IBM z/OS Mainframe
- **Security**: ACF2

## 🏛️ Historical Significance

DSF represents a remarkable achievement:
- **Longevity**: 51 years of continuous operation
- **Scale**: Handled pension data for Norway's entire population
- **Reliability**: Critical national infrastructure
- **Evolution**: Adapted to countless legislative changes
- **Transition**: Successfully replaced by modern systems

## 🤝 Contributing

This is a historical archive. The system is no longer in production. However:

- **Documentation improvements** are welcome
- **Analysis and insights** help understand legacy systems
- **Modernization case studies** based on DSF are valuable for academic purposes, I suggest make use of Github Coding Agents and modern practises!
- **Educational use** - learn from 51 years of software evolution

## 📄 License

See [LICENSE.md](LICENSE.md) for license information.

## 🔗 Related Systems

- [Presys](https://github.com/navikt/presys) - DSF's successor for person data
- Pesys - DSF's successor for pension data

## 📖 Learn More

- [NAV (Norwegian Labour and Welfare Administration)](https://www.nav.no/)
- [Norwegian National Insurance Act (Folketrygdloven)](https://lovdata.no/dokument/NL/lov/1997-02-28-19)
- [PL/I Language Reference](https://en.wikipedia.org/wiki/PL/I)
- [CICS Transaction Processing](https://en.wikipedia.org/wiki/CICS)

---

*Documentation created with empathy for future developers who will study and learn from this historic system.*
