# DSF Module Dependency Diagram

This document provides visual representations of the DSF system architecture and module relationships.

## High-Level System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         DSF System                               │
│                  (Det Sentrale Folketrygdsystemet)              │
└─────────────────────────────────────────────────────────────────┘
                                │
                ┌───────────────┴───────────────┐
                │                               │
        ┌───────▼────────┐            ┌────────▼────────┐
        │   CICS/TSO     │            │   Database      │
        │  Transaction   │            │   DB2/VSAM      │
        │   Processing   │            │                 │
        └───────┬────────┘            └────────┬────────┘
                │                               │
        ┌───────┴────────────────────────┬──────┴─────┐
        │                                │            │
┌───────▼──────┐                ┌────────▼──────┐   ┌▼──────────┐
│  User        │                │  Batch Jobs   │   │  Reports  │
│  Interface   │                │  (R0012)      │   │  (R0019)  │
│  (Screens)   │                └───────────────┘   └───────────┘
└──────────────┘
```

## Transaction Flow Architecture

```
┌──────────────┐
│   Terminal   │
│   Operator   │
└──────┬───────┘
       │
       │ R010 (Login)
       ▼
┌──────────────────────┐
│   R0010101           │  Display Login Screen
│   User ID Prompt     │
└──────┬───────────────┘
       │
       │ R020 (Validate)
       ▼
┌──────────────────────┐
│   R0010201           │  Validate User ID
│   Authentication     │  Check ACF2 Security
└──────┬───────────────┘
       │
       │ R030 (Menu)
       ▼
┌──────────────────────┐
│   R0010301           │  Main Menu
│   Function Selection │  A/E/R/I/F/V/X
└──────┬───────────────┘
       │
       └───┬──────────┬──────────┬──────────┬──────────┬──────────┐
           │          │          │          │          │          │
    [A]    │   [R]    │   [I]    │   [F]    │   [V]    │   [X]    │
    Admin  │   Reg    │   Income │   Query  │   Wait   │   Exit   │
           ▼          ▼          ▼          ▼          ▼          ▼
```

## Module Organization

```
DSF System (717 PL/I Programs)
│
├── R001 Series (197 files) - Infrastructure
│   ├── R001A (3) - Administration
│   ├── R001B (5) - Batch utilities
│   ├── R001C (3) - Control
│   ├── R001I (20) - Income processing
│   ├── R001N (86) - New registrations
│   ├── R001T (24) - Transactions
│   └── R001U (51) - Updates
│
├── R0010 Series (91 files) - System Menu & Navigation
│   ├── R0010101-201 - Login/Auth
│   ├── R0010301 - Main Menu
│   ├── R0010401-412 - Registration Forms
│   ├── R0010410 - Transaction Builder (468KB)
│   ├── R0010420-490 - Menu Processing
│   └── R00104XX - Form Mappers
│
├── R0011 Series (81 files) - Person Registration
│   ├── R0011001-022 - Person Data Entry
│   ├── R0011101-122 - Income Registration
│   ├── R0011201-222 - Family Relations
│   └── R0011301+ - Other Attributes
│
├── R0012 Series (8 files) - Batch Processing
│   └── Mass calculations and reports
│
├── R0013 Series (7 files) - Core Calculations
│   └── Main pension calculation engines
│
├── R0014 Series (118 files) - Disability Pension ★
│   ├── R0014001-022 - Application Intake
│   ├── R0014101-199 - Assessment
│   ├── R0014201-270 - Work Capacity
│   ├── R0014301-380 - Income Evaluation
│   ├── R0014401-580 - Benefit Calculation
│   └── R0014601-901 - Special Cases
│
├── R0015 Series (59 files) - Old Age Pension
│   ├── R0015201-220 - Standard Calculation
│   ├── R0015301-320 - Enhanced Rules
│   └── R0015401-470 - AFP (Early Retirement)
│
├── R0016 Series (22 files) - Survivor Benefits
│   ├── R0016022-039 - Spouse Pensions
│   └── R0016051-053 - Orphan Benefits
│
├── R0017 Series (23 files) - Occupational Injury
│   └── Work-related injury claims
│
├── R0018 Series (15 files) - Special Processing
│   └── Edge cases and complex scenarios
│
└── R0019 Series (93 files) - Query & Reporting
    ├── R0019901-999 - Person Inquiries
    └── R0019A-H - Various Reports

★ = Largest module (118 files)
```

## Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                      Person Data                             │
│                                                              │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐ │
│  │ Identification│    │   Income     │    │   Status     │ │
│  │  • FNR       │    │  • PI (year) │    │  • Active    │ │
│  │  • Name      │    │  • Work      │    │  • Pensioner │ │
│  │  • Office    │    │  • Pension   │    │  • Deceased  │ │
│  └──────┬───────┘    └──────┬───────┘    └──────┬───────┘ │
│         │                   │                    │         │
└─────────┼───────────────────┼────────────────────┼─────────┘
          │                   │                    │
          └───────────────────┴────────────────────┘
                              │
          ┌───────────────────┴────────────────────┐
          │                                        │
    ┌─────▼─────┐                          ┌──────▼──────┐
    │ Validation │                          │ Calculation │
    │  (R0011)  │                          │   (R0013-   │
    │           │                          │    R0018)   │
    └─────┬─────┘                          └──────┬──────┘
          │                                       │
          │                                       │
    ┌─────▼──────────────────────────────────────▼─────┐
    │           Pension Decision                       │
    │  • Old Age (R0015)                              │
    │  • Disability (R0014)                           │
    │  • Survivor (R0016)                             │
    │  • Occupational (R0017)                         │
    └─────┬────────────────────────────────────────────┘
          │
          ▼
    ┌──────────────┐
    │   Payment    │
    │   Processing │
    └──────────────┘
```

## Module Interaction Matrix

```
                R0010  R0011  R0012  R0013  R0014  R0015  R0016  R0017  R0018  R0019
              ┌──────┬──────┬──────┬──────┬──────┬──────┬──────┬──────┬──────┬──────┐
R0010 (Menu)  │  ●   │  →   │      │      │      │      │      │      │      │  →   │
              ├──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┤
R0011 (Person)│  ←   │  ●   │      │  →   │      │      │      │      │      │      │
              ├──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┤
R0012 (Batch) │      │  →   │  ●   │  →   │  →   │  →   │  →   │  →   │  →   │  →   │
              ├──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┤
R0013 (Calc)  │      │  ←   │  ←   │  ●   │  ↔   │  ↔   │  ↔   │  ↔   │  ↔   │      │
              ├──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┤
R0014 (Disab) │      │  ←   │  ←   │  ←   │  ●   │      │      │      │      │      │
              ├──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┤
R0015 (OldAge)│      │  ←   │  ←   │  ←   │      │  ●   │      │      │      │      │
              ├──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┤
R0016 (Surviv)│      │  ←   │  ←   │  ←   │      │      │  ●   │      │      │      │
              ├──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┤
R0017 (OccInj)│      │  ←   │  ←   │  ←   │      │      │      │  ●   │      │      │
              ├──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┤
R0018 (Spec)  │      │  ←   │  ←   │  ←   │  ↔   │  ↔   │  ↔   │  ↔   │  ●   │      │
              ├──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┼──────┤
R0019 (Query) │  ←   │  →   │      │      │  →   │  →   │  →   │  →   │  →   │  ●   │
              └──────┴──────┴──────┴──────┴──────┴──────┴──────┴──────┴──────┴──────┘

Legend:
  ●  = Self (module interacts with itself)
  →  = Calls/invokes
  ←  = Called by
  ↔  = Bidirectional interaction
```

## Shared Data Structures (P00199XX)

```
                    ┌─────────────────────────┐
                    │   P0019908 (KOM_OMR)   │
                    │   Communication Area    │
                    └────────────┬────────────┘
                                 │
                 ┌───────────────┼───────────────┐
                 │               │               │
        ┌────────▼──────┐  ┌────▼─────┐  ┌──────▼────────┐
        │  P0019906     │  │ P0019910 │  │  P0019912     │
        │  Trans Info   │  │ Control  │  │  Div Params   │
        └───────────────┘  └──────────┘  └───────────────┘

                    ┌─────────────────────────┐
                    │  P0019921 (PERSON)     │
                    │  Master Person Record   │
                    │  62,608 bytes!          │
                    └────────────┬────────────┘
                                 │
         ┌───────────────────────┼───────────────────────┐
         │                       │                       │
    ┌────▼────┐          ┌──────▼──────┐        ┌──────▼──────┐
    │ P0019927│          │  P0019932   │        │  P0019934   │
    │ Identity│          │  Old Age    │        │  Disability │
    └─────────┘          │  Pension    │        │  Pension    │
                         └─────────────┘        └─────────────┘
```

## File Size Distribution

```
File Size Analysis (logarithmic scale)
═══════════════════════════════════════

R0010410 ████████████████████████████████████████ 468 KB  (Transaction Builder)
R0010430 ████████████████████████ 234 KB  (Main Processing)
R0010440 ████████ 67 KB   (Transaction Delete)
R0010450 █████████ 71 KB  (Transaction Remove)
R0010451 ██████ 50 KB     (Remove Last Trans)
R0010452 █████ 40 KB      (Remove All Trans)
R0010201 ████ 37 KB       (User Validation)
R0010301 █████ 46 KB      (Main Menu)
R0010401 ██ 22 KB         (Registration)
Most others █ < 20 KB

Note: Larger files = More complex business logic
```

## Technology Stack Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    User Interface                        │
│              (3270 Terminal Screens)                     │
└────────────────────┬────────────────────────────────────┘
                     │
                     │ BMS (Basic Mapping Support)
                     │
┌────────────────────▼────────────────────────────────────┐
│                   CICS Layer                             │
│  • Transaction Management                                │
│  • Screen I/O                                            │
│  • Program Control                                       │
└────────────────────┬────────────────────────────────────┘
                     │
                     │ PL/I Programs
                     │
┌────────────────────▼────────────────────────────────────┐
│              Application Layer (DSF)                     │
│  • R0010: Menu/Navigation                                │
│  • R0011: Data Entry                                     │
│  • R0012: Batch Jobs                                     │
│  • R0013-R0018: Calculations                             │
│  • R0019: Queries                                        │
└────────────────────┬────────────────────────────────────┘
                     │
                     │ Database I/O
                     │
┌────────────────────▼────────────────────────────────────┐
│                Database Layer                            │
│  • DB2: Relational data                                  │
│  • VSAM: Indexed file system                             │
└──────────────────────────────────────────────────────────┘
```

## Modernization Path

```
Current State (Mainframe)           Target State (Modern)
═════════════════════════           ════════════════════

┌─────────────────┐                 ┌─────────────────┐
│  3270 Terminal  │                 │   Web Browser   │
└────────┬────────┘                 │   Mobile App    │
         │                          └────────┬────────┘
         │                                   │
┌────────▼────────┐                 ┌────────▼────────┐
│      CICS       │                 │   API Gateway   │
└────────┬────────┘                 └────────┬────────┘
         │                                   │
┌────────▼────────┐      Phase    ┌─────────▼─────────┐
│   PL/I (DSF)    │  ══════════>  │  Microservices    │
│                 │    2-5 years   │  • Auth Service   │
│  • 717 files    │                │  • Person Service │
│  • 51 years old │                │  • Calc Service   │
└────────┬────────┘                │  • Query Service  │
         │                          └─────────┬─────────┘
┌────────▼────────┐                          │
│   DB2/VSAM      │                 ┌─────────▼─────────┐
└─────────────────┘                 │   PostgreSQL      │
                                    │   + Redis Cache   │
                                    └───────────────────┘

Approach: Strangler Fig Pattern
• Start with queries (R0019) - lowest risk
• Then batch (R0012) - isolated
• Then calculations (R0013-R0018) - critical
• Finally UI/data entry (R0010-R0011) - integrated
```

## Module Complexity Rating

```
Complexity Assessment (based on file count, size, and interconnections)

R0014 (Disability)    ████████████████████████ Very High (118 files)
R0010 (Menu/Nav)      ███████████████████ High (91 files, 468KB max)
R0019 (Query)         ████████████████ High (93 files)
R0011 (Person)        █████████████ Medium-High (81 files)
R0015 (Old Age)       ██████████ Medium (59 files)
R0016 (Survivor)      ████ Low-Medium (22 files)
R0017 (Occup Inj)     ████ Low-Medium (23 files)
R0018 (Special)       ███ Low (15 files)
R0012 (Batch)         ██ Low (8 files)
R0013 (Core Calc)     ██ Low* (7 files, but critical!)

* Low file count doesn't mean low importance!
  R0013 contains core calculation engines.
```

## Data Segment Relationships

```
PERSON (Master Record)
│
├─→ RF0PERSN (Identity)
│   ├─ FNR (National ID)
│   ├─ NAVN (Name)
│   └─ TKNR (Office)
│
├─→ PINNTEKT (Income 1967-2014)
│   └─ PI (Pension-qualifying income per year)
│
├─→ STATUS (Current Status)
│   ├─ STATUS_KODE
│   ├─ PENSJONSTYPE
│   ├─ SIVILSTAND
│   └─ TT (Insurance time)
│
├─→ TILKN (Family Connections)
│   └─ FNR_TILKN (Related person IDs)
│
├─→ ALDERSP (Old Age Pension)
│   ├─ GP (Basic pension)
│   ├─ TP (Supplementary)
│   └─ Various supplements
│
├─→ UFØRPENS (Disability Pension)
│   ├─ UFG (Disability degree)
│   └─ Calculation details
│
├─→ ETTEPENS (Survivor - Spouse)
│   └─ Widow/widower details
│
├─→ ETTEBARN (Survivor - Children)
│   └─ Orphan details
│
├─→ YRKEPENS (Occupational Injury)
│   ├─ YUG (Injury degree)
│   └─ Compensation details
│
├─→ AFPHIST (Early Retirement History)
│   └─ AFP transaction history
│
└─→ EØSINFO (EEA Coordination)
    └─ International pension coordination
```

## See Also

- [ARCHITECTURE.md](ARCHITECTURE.md) - System architecture details
- [MODULE_STRUCTURE.md](MODULE_STRUCTURE.md) - Detailed module descriptions
- [REFACTORING_GUIDE.md](REFACTORING_GUIDE.md) - Modernization strategy
- [DEVELOPER_GUIDE.md](DEVELOPER_GUIDE.md) - Getting started guide
- [GLOSSARY.md](GLOSSARY.md) - Norwegian terms explained
