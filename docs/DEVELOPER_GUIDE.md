# Developer Onboarding Guide

## Welcome to the DSF Codebase!

This guide will help you understand and navigate the DSF (Det Sentrale Folketrygdsystemet) system. Whether you're here to maintain, refactor, or learn from this historic system, this guide is your starting point.

## What is DSF?

DSF is a 51-year-old Norwegian National Insurance system written in PL/I that processed pensions, disability benefits, and survivor benefits for all of Norway from 1967 to 2018. Think of it as the entire Norwegian social security system's backend.

## Quick Start

### Understanding the Basics

1. **Read First**: 
   - [README.md](../README.md) - System history and context
   - [ARCHITECTURE.md](ARCHITECTURE.md) - High-level system design
   - [MODULE_STRUCTURE.md](MODULE_STRUCTURE.md) - Detailed module breakdown

2. **Key Concepts**:
   - **PL/I**: IBM's high-level programming language (1960s)
   - **CICS**: Transaction processing middleware
   - **Mainframe**: IBM z/OS operating system
   - **BMS**: Screen definition system (like modern forms)

3. **Norwegian Terms** (critical to understand):
   - **Folketrygd**: National Insurance
   - **Pensjon**: Pension
   - **Uførepensjon**: Disability pension
   - **Alderspensjon**: Old age pension
   - **Etterlatte**: Survivors (widows/widowers/orphans)
   - **Yrkesskade**: Occupational injury

## Repository Structure

```
DSF/
├── src/               # Source code (717 PL/I files)
│   ├── R0010*.pli    # Menu and navigation (91 files)
│   ├── R0011*.pli    # Person registration (81 files)
│   ├── R0012*.pli    # Batch processing (8 files)
│   ├── R0013*.pli    # Core calculations (7 files)
│   ├── R0014*.pli    # Disability pension (118 files)
│   ├── R0015*.pli    # Old age pension (59 files)
│   ├── R0016*.pli    # Survivor benefits (22 files)
│   ├── R0017*.pli    # Occupational injury (23 files)
│   ├── R0018*.pli    # Special processing (15 files)
│   ├── R0019*.pli    # Queries and reports (93 files)
│   ├── R001*.pli     # Core infrastructure (197 files)
│   └── GML/          # COBOL utilities (legacy)
├── docs/              # Documentation (you are here!)
│   ├── ARCHITECTURE.md
│   ├── MODULE_STRUCTURE.md
│   ├── REFACTORING_GUIDE.md
│   ├── GLOSSARY.md
│   └── DEVELOPER_GUIDE.md (this file)
└── README.md
```

## File Naming Convention

Understanding file names is crucial:

### Pattern: `[Type][Module][Sub][Variant].pli`

**Examples:**
- `R0010301.pli`: R=Runtime, 001=Core, 03=Menu, 01=Variant 1
- `R0014125.pli`: R=Runtime, 0014=Disability, 12=Assessment, 5=Variant 5
- `R0015201.pli`: R=Runtime, 0015=Old Age, 20=Calculation, 1=Variant 1

### Types:
- **R**: Regular/Runtime programs (most common)
- **A**: Administrative utilities
- **G**: General utilities  
- **J**: Job control
- **P**: Include files/data structures (P00199XX series)
- **S**: Screen definitions/maps

## How to Read PL/I Code

### Basic Structure

```pli
/* Comment header with change history */
R0010101: PROC OPTIONS(MAIN);

  /* Declarations */
  DCL variable_name CHAR(10);
  DCL counter FIXED DEC(5);
  
  /* Include files */
  %INCLUDE P0019908;  /* Shared data structures */
  
  /* Main logic */
  IF condition THEN
    DO;
      /* Code block */
    END;
    
  /* CICS commands */
  EXEC CICS READ FILE('PERSON') INTO(person_record);
  
END; /* R0010101 */
```

### Common Patterns

1. **Data Declarations**:
```pli
DCL FNR FIXED DEC(11);           /* National ID - 11 digits */
DCL NAVN CHAR(25);                /* Name - 25 characters */
DCL PENSJON_DATO FIXED DEC(9);   /* Date in YYYYMMDD format */
```

2. **Screen I/O**:
```pli
EXEC CICS SEND MAP('S001011') MAPSET('S001013');
EXEC CICS RECEIVE MAP('S001011') MAPSET('S001013');
```

3. **Database Access**:
```pli
EXEC CICS READ FILE('PERSON') INTO(person_area) RIDFLD(fnr);
EXEC CICS WRITE FILE('PERSON') FROM(person_area) RIDFLD(fnr);
EXEC CICS REWRITE FILE('PERSON') FROM(person_area);
```

4. **Error Handling**:
```pli
EXEC CICS HANDLE CONDITION ERROR(ERROR_ROUTINE);
EXEC CICS HANDLE CONDITION NOTFND(NOT_FOUND_ROUTINE);
```

### Key Data Structures (from database.txt)

**Person Master Record** (62,608 bytes per person!):
```pli
2 PERSON (1:14),                    /* Array of 14 person records */
  3 RF0PERSN UNALIGNED,
    4 FNR DEC FIXED(11),            /* National ID */
    4 NAVN CHAR(25),                 /* Name */
    4 TKNR DEC FIXED(5),            /* Social security office */
  3 PINNTEKT(1967:2010) UNALIGNED, /* Income by year */
    4 PI DEC FIXED(9),              /* Pension-qualifying income */
  3 STATUS UNALIGNED,                /* Pension status */
  3 ALDERSP UNALIGNED,              /* Old age pension data */
  3 UFØRPENS UNALIGNED,             /* Disability pension data */
  /* ... many more segments ... */
```

## Common Tasks

### Task 1: Find Where a Function is Implemented

**Example: "Where is disability degree calculated?"**

1. Check [MODULE_STRUCTURE.md](MODULE_STRUCTURE.md) - see R0014 series
2. Look for R0014121-149 (degree calculations)
3. Search for keywords like "UFG" (Uføregrad = disability degree)
4. Find R0014125.pli or similar

### Task 2: Understand a Calculation

**Example: "How is old age pension (GP) calculated?"**

1. Start with R0013 series (core calculations)
2. Then R0015 series (old age specifics)
3. Look for "GP" declarations (Grunnpensjon = basic pension)
4. Find formulas in R0015201-220

**Tip**: Calculations often reference Norwegian law (Folketrygdloven) - you may need policy documents.

### Task 3: Trace a Transaction Flow

**Example: "What happens when user selects 'Register new person'?"**

1. Start at R0010301 (main menu)
2. Look for function code 'R' (Registration)
3. Follow TRANSID to R0010401
4. Follow subsequent TRANSIDs through R0011XXX series

### Task 4: Understand Data Flow

1. **Person enters system**: R0010101 → Authentication
2. **Navigates menu**: R0010301 → Function selection
3. **Data entry**: R0011XXX → Validation and storage
4. **Calculation**: R0013-R0018 → Benefit computation
5. **Query**: R0019XXX → Information retrieval

## Critical Concepts

### 1. Transaction-Based Architecture

Each user interaction is a CICS transaction:
- **R010**: Initial login screen
- **R020**: User validation
- **R030**: Main menu
- **R040-R990**: Various functions

Transactions pass control using:
```pli
EXEC CICS RETURN TRANSID('R030') COMMAREA(kom_omr);
```

### 2. COMMAREA (Communication Area)

Data persists between transactions via COMMAREA:
```pli
%INCLUDE P0019908;  /* KOM_OMR structure */
EXEC CICS RETURN TRANSID('NEXT') COMMAREA(KOM_OMR);
```

This is like session state in modern web apps.

### 3. Screen Management (BMS)

Screens defined separately from code:
- **MAP**: Screen layout (S001011)
- **MAPSET**: Collection of maps (S001013)
- **SEND MAP**: Display screen to user
- **RECEIVE MAP**: Get user input

### 4. Date Handling

Dates stored as integers:
- `19670101` = January 1, 1967
- `20181231` = December 31, 2018

```pli
DCL DATO FIXED DEC(9);  /* YYYYMMDD */
```

### 5. Norwegian National ID (FNR)

11-digit identifier:
- First 6 digits: Birth date (DDMMYY)
- Next 3 digits: Individual number
- Last 2 digits: Checksums

```pli
DCL FNR FIXED DEC(11);  /* 11111111111 */
```

## Debugging Tips

### 1. Follow the Comments

Headers contain valuable information:
```pli
/*  SIST ENDRET PÅ PROD   2006.07.18 11.15.42 AV   JDA2970  */
/*  PROGRAM-IDENT : R0010301 - VALG AV FUNKSJON            */
/*  HENSIKT: Main function selection menu                   */
```

- Last changed date
- Program purpose
- Programmer ID

### 2. Search for Key Terms

| Norwegian | English | Search For |
|-----------|---------|------------|
| Fødselsnummer | National ID | FNR |
| Pensjon | Pension | GP, TP, PENSJON |
| Uføregrad | Disability degree | UFG |
| Beregning | Calculation | BEREGN |
| Inntekt | Income | INNT, PI |
| Dato | Date | DATO, ÅMD |

### 3. Large Files = Complex Logic

The biggest files contain the most complex logic:
- R0010410.pli (468KB): Transaction management
- R0010430.pli (234KB): Main processing

Start with smaller files in the same series to understand patterns first.

### 4. Include Files Are Your Friend

P00199XX files define shared structures:
- Always check the include files to understand data layouts
- One change affects many programs
- Comments often explain field meanings

## Domain Knowledge

### Norwegian Pension System Basics

1. **Three Tiers**:
   - **Grunnpensjon (GP)**: Basic pension - everyone gets this
   - **Tilleggspensjon (TP)**: Supplementary - based on income history
   - **Special supplements**: Various additions based on circumstances

2. **Benefit Types**:
   - **Alderspensjon**: Old age (retirement) - age 67 (historically)
   - **Uførepensjon**: Disability - unable to work
   - **Etterlattepensjon**: Survivors - spouse/children of deceased
   - **AFP**: Early retirement - available before 67
   - **Yrkesskade**: Occupational injury - work-related

3. **Key Concepts**:
   - **Trygdetid (TT)**: Insurance time - years in Norway
   - **Poengår (PÅ)**: Pension points - based on income
   - **Sats punkt (SPT)**: Supplementary pension points
   - **Grunnbeløp (G)**: Base amount - indexed annually

### Important Numbers

- **1967**: National Insurance Act came into effect
- **1991**: Major reform - changed calculations
- **2018**: DSF decommissioned
- **40 years**: Full pension requires 40 years residence
- **67**: Historical retirement age

## Tools You'll Need

### For Reading Code:
1. **Text Editor**: VS Code, Sublime Text, or vim
   - PL/I syntax highlighting available
   - Search across files is essential

2. **Search Tools**:
   ```bash
   # Find all files containing "UFØRPENS"
   grep -r "UFØRPENS" src/
   
   # Find program by name
   find src/ -name "R0014125*"
   
   # Count files in module
   ls src/R0014*.pli | wc -l
   ```

3. **Documentation Tools**:
   - Markdown editor for docs
   - Diagram tools (draw.io, PlantUML) for visualizing flows

### For Understanding Logic:
1. **Domain Expert**: Find someone who understands Norwegian pensions
2. **Policy Documents**: Folketrygdloven (National Insurance Act)
3. **Test Cases**: Production scenarios and expected results

## Learning Path

### Week 1: Orientation
- [ ] Read all documentation in docs/
- [ ] Understand file naming conventions
- [ ] Browse 5-10 small programs (< 100 lines)
- [ ] Learn basic PL/I syntax
- [ ] Understand CICS transaction flow

### Week 2: Basic Module Understanding
- [ ] Study R0010 series (menu system) - entry point
- [ ] Trace one complete transaction flow
- [ ] Understand COMMAREA and state management
- [ ] Review P00199XX include files
- [ ] Map out module dependencies

### Week 3: Domain Knowledge
- [ ] Learn Norwegian pension system basics
- [ ] Understand key calculations (GP, TP)
- [ ] Review sample person records
- [ ] Study one complete benefit type (start with old age)

### Week 4: Deep Dive
- [ ] Choose one module to specialize in
- [ ] Document 3-5 programs in detail
- [ ] Create flowcharts for complex logic
- [ ] Identify modernization opportunities

### Month 2-3: Contribution
- [ ] Document undocumented programs
- [ ] Create test scenarios
- [ ] Identify refactoring candidates
- [ ] Propose modernization strategies

## Common Pitfalls

### 1. Ignoring Norwegian Context
**Problem**: Treating this like any other system  
**Solution**: Understand it represents Norwegian law and policy

### 2. Rushing to Refactor
**Problem**: Changing code without understanding purpose  
**Solution**: Document first, change later

### 3. Underestimating Complexity
**Problem**: "It's just old code, how hard can it be?"  
**Solution**: 51 years of edge cases and special rules

### 4. Not Testing Thoroughly
**Problem**: "This small change won't break anything"  
**Solution**: Financial calculations demand perfection

### 5. Losing Institutional Knowledge
**Problem**: Not documenting discoveries  
**Solution**: Write down everything you learn

## Getting Help

### Internal Resources
1. **Documentation**: Start with docs/ directory
2. **Code Comments**: Often contain explanations
3. **Include Files**: Data structure definitions
4. **Module Guides**: See MODULE_STRUCTURE.md

### External Resources
1. **PL/I Language Reference**: IBM documentation
2. **CICS Documentation**: IBM CICS manuals
3. **Norwegian Policy**: Folketrygdloven
4. **NAV Website**: Current pension information

### Questions to Ask
- "What business rule does this implement?"
- "Why is this calculation done this way?"
- "What Norwegian law requires this?"
- "Has this ever changed historically?"
- "What are the edge cases?"

## Your First Contribution

### Good First Tasks:
1. **Document a small program** (< 200 lines)
2. **Create a glossary entry** for Norwegian terms
3. **Map a transaction flow** with diagrams
4. **Extract business rules** from calculations
5. **Write test scenarios** for a module

### Documentation Template:
```markdown
# Program: R0014125

## Purpose
Calculate disability degree percentage

## Inputs
- Person record (FNR)
- Medical assessment data
- Work capacity evaluation

## Processing
1. Retrieve person data
2. Calculate functional capacity
3. Compare with occupational requirements
4. Determine percentage (0-100%)

## Outputs
- UFG (Uføregrad) percentage
- Status code
- Effective date

## Business Rules
- Must be between 0-100%
- Minimum 50% for benefits
- Rounded to nearest 5%

## Related Programs
- R0014121: Initial assessment
- R0014126: Grade validation

## Notes
- Based on Folketrygdloven §12-4
- Changed in 1991 reform
```

## Conclusion

The DSF system is a masterpiece of sustained software engineering:
- **Scale**: Handled millions of Norwegians over 51 years
- **Reliability**: Continuous operation for half a century
- **Adaptability**: Accommodated countless policy changes
- **Complexity**: Encodes decades of pension policy

Treat it with respect. Every line of code represents someone's retirement security. Every calculation affects real lives. Every change must be perfect.

**Welcome to the team! Your work matters.**

---

## Quick Reference Card

### File Locations
- Source code: `src/*.pli`
- Documentation: `docs/*.md`
- Data structures: `src/database.txt`, `P00199*.pli`

### Module Cheat Sheet
- R0010: Menu & navigation (91)
- R0011: Person data (81)
- R0012: Batch jobs (8)
- R0013: Calculations (7)
- R0014: Disability (118) ← largest
- R0015: Old age (59)
- R0016: Survivors (22)
- R0017: Occupational (23)
- R0018: Special (15)
- R0019: Queries (93)

### Essential Abbreviations
- FNR: National ID
- GP: Basic pension
- TP: Supplementary pension
- UFG: Disability degree
- TT: Insurance time
- PÅ: Pension points
- PI: Pension-qualifying income
- CICS: Transaction system
- BMS: Screen manager

### Key Commands
```bash
# Find a program
find src/ -name "R0014125*"

# Search for term
grep -r "UFØRPENS" src/

# Count files in module
ls src/R0014*.pli | wc -l

# View program header
head -50 src/R0014125.pli
```

### Emergency Contacts
- **Architectural questions**: See ARCHITECTURE.md
- **Module details**: See MODULE_STRUCTURE.md
- **Refactoring**: See REFACTORING_GUIDE.md
- **Terms**: See GLOSSARY.md (when created)
