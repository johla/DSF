# DSF Module Structure and Organization

## Module Naming Convention

The DSF system uses a systematic naming convention for its programs:

### Format: `[Type][Module][Submodule][Variant]`

- **Type**: Letter indicating program category (R, A, G, J)
- **Module**: 4-digit code indicating functional area (0010-0019)
- **Submodule**: 2-digit code for specific function within module
- **Variant**: Optional 1-2 characters for variations

### Example: `R0010301.pli`
- `R` = Registration/Runtime program
- `001` = Core system module
- `03` = Function selection submenu
- `01` = Main/first variant

## Core Modules

### R001 Series - Core System Foundation (197 files)
Main system infrastructure and foundational components.

Submodules:
- **R001A**: Administration functions (3 files)
- **R001B**: Batch processing utilities (5 files)
- **R001C**: Control and coordination (3 files)
- **R001H**: Help functions (1 file)
- **R001I**: Income/Inntekt processing (20 files)
- **R001N**: New registrations (86 files)
- **R001S**: System utilities (3 files)
- **R001T**: Transaction processing (24 files)
- **R001U**: Update/Update operations (51 files)
- **R001X**: Exit/termination (1 file)

### R0010 Series - System Menu and Navigation (91 files)
Controls system startup, user authentication, and menu navigation.

**Key Programs:**
- **R0010101**: Initial screen - User ID prompt
- **R0010201**: User ID validation and authentication
- **R0010301**: Main function selection menu
- **R0010401**: Registration of new forms (blanketter)
- **R0010410**: Transaction building and management (468KB - largest file)
- **R0010411**: Build transaction list
- **R0010412**: Map from transaction list
- **R0010420-427**: Menu and form handling
- **R0010430**: Main menu processing (234KB)
- **R0010440-452**: Transaction deletion/removal
- **R0010460-470**: Screen splitting and display
- **R0010480-490**: Wait transaction handling
- **R00104XX**: Various form mapping programs (A1, AF, AP, B6, BP, E1-E4, EE, EF, EN, EP, F7, FB, FO, FT, KF, O1-O2, U2-U3, UF, UP, US)

**Responsibilities:**
- User authentication and authorization
- System menu navigation
- Transaction initiation and routing
- Screen management and display
- Wait queue management

### R0011 Series - Person Registration (81 files)
Handles registration and maintenance of person records.

**Key Functions:**
- **R0011001-009**: Person data entry and validation
- **R0011020-022**: Person data updates
- **R0011101-122**: Income registration workflows
- **R0011201-222**: Family relationship management
- **R0011301-306**: Citizenship and residency
- **R0011401-460**: Various person attributes
- **R0011501-522**: Status changes
- **R0011601-622**: Historical data
- **R0011701-720**: Special cases
- **R0011801-840**: Complex validations
- **R0011901-922**: Final processing and confirmation

**Responsibilities:**
- Person master data management
- Family connections
- Income reporting
- Citizenship and residency tracking
- Historical record keeping

### R0012 Series - Batch Processing (8 files)
Batch job control and mass processing.

**Programs:**
- **R0012001-005**: Batch job setup and execution
- **R0012010**: Batch control
- **R0012101**: Batch reporting
- **R0012201**: Batch validation
- **R0012301**: Batch completion

**Responsibilities:**
- Mass updates and calculations
- End-of-day/month/year processing
- Report generation
- Data consistency checks

### R0013 Series - Pension Calculations (7 files)
Core pension benefit calculations and rules.

**Programs:**
- **R0013001**: Main calculation engine
- **R0013101**: Pension formula application
- **R0013110**: Supplementary calculations
- **R0013301**: Special pension rules
- **R0013501**: Validation of calculations
- **R0013520**: Recalculation routines
- **R0013601**: Historical calculation adjustments

**Responsibilities:**
- Old age pension calculations
- Disability pension calculations
- Survivor pension calculations
- Income-tested benefits
- Special rules and exceptions

### R0014 Series - Disability Pension (Uførepensjon) (118 files)
Largest module - handles all aspects of disability pension.

**Major Subgroups:**
- **R0014001-022**: Disability application intake
- **R0014101-199**: Disability assessment and grading
- **R0014121-149**: Degree of disability calculations
- **R0014151-187**: Medical evaluations
- **R0014201-270**: Work capacity assessments
- **R0014301-380**: Income considerations
- **R0014401-482**: Benefit calculations
- **R0014501-580**: Payment processing
- **R0014601-724**: Appeals and reconsiderations
- **R0014801-901**: Special cases and exceptions

**Responsibilities:**
- Disability assessment workflows
- Medical documentation management
- Work capacity evaluation
- Income testing
- Benefit amount calculation
- Payment authorization
- Rehabilitation tracking
- Appeal handling

### R0015 Series - Old Age Pension (Alderspensjon) (59 files)
Old age pension processing and management.

**Key Subgroups:**
- **R0015101**: Old age pension initiation
- **R0015201-220**: Standard pension calculations
- **R00152N5-NC**: Normalized pension variants
- **R00152U6-UJ**: Update routines
- **R0015301-320**: Enhanced pension rules
- **R00153N5-NC**: Normalized variants
- **R00153U6-UJ**: Update variants
- **R0015401-470**: Special pension types (AFP - early retirement)
- **R0015501-502**: Payment processing
- **R0015601-602**: Final validations

**Responsibilities:**
- Old age pension applications
- Early retirement (AFP) processing
- Pension amount calculation
- Spouse benefits
- Deferral bonuses
- Income testing
- Payment initiation

### R0016 Series - Survivor Benefits (22 files)
Widow/widower and orphan pension processing.

**Programs:**
- **R0016001**: Survivor benefit initiation
- **R0016022-039**: Widow/widower pension processing
- **R0016031-039**: Calculation variations
- **R0016051-053**: Orphan benefits
- **R0016101-601**: Various survivor benefit types

**Responsibilities:**
- Spouse survivor benefits
- Child survivor benefits
- Transitional benefits
- Income testing
- Remarriage handling
- Age-out processing for children

### R0017 Series - Occupational Injury (Yrkesskade) (23 files)
Work-related injury and illness benefits.

**Programs:**
- **R0017001**: Occupational injury registration
- **R0017021-050**: Injury assessment and classification
- **R0017031-045**: Degree of injury determination
- **R0017099**: Special cases
- **R0017101**: Final processing

**Responsibilities:**
- Workplace accident claims
- Occupational disease claims
- Injury degree assessment
- Compensation calculations
- Long-term disability from work injury
- Employer liability tracking

### R0018 Series - Special Processing (15 files)
Specialized calculation and validation routines.

**Programs:**
- **R0018010-090**: Various special processing needs
- **R0018A12, B12, C12**: Algorithm variants

**Responsibilities:**
- Edge case handling
- Complex multi-benefit scenarios
- Historical corrections
- Special legislative provisions

### R0019 Series - Inquiry and Reporting (93 files)
Query functions and report generation.

**Major Functions:**
- **R0019901-999**: Person inquiry functions
- **R0019920-990**: Various query types
- **R0019A01-A03**: Administrative queries
- **R0019D70**: Data retrieval
- **R0019E01-E04**: Export functions
- **R0019F01-F21**: Form generation
- **R0019H01-H60**: Help and information displays

**Responsibilities:**
- Person information retrieval
- Transaction history inquiry
- Payment history queries
- Statistical reporting
- Data export for analysis
- Ad-hoc queries
- Management reports

## Support Components

### A Series - Administrative Tools (1 file)
- **A0010301**: Administrative program (appears to be copy/variant of R0010301)

### G Series - General Utilities (1 file)
- **G0019999**: General utility functions

### J Series - Job Control (1 file)
- **J0010301**: Job control and scheduling

### GML Directory - COBOL Utilities (6 COBOL files + PL/I copies)
Historical/legacy programs, possibly from earlier system versions:
- **FO04D1X1.cobol**: Form processing
- **FO04F1X1.cobol**: Form handling
- **FRMERK.cobol**: Form marking
- **FRREDIG.cobol**: Form editing
- **PLUKKFR.cobol**: Form picking/selection
- **PLUKKFRN.cobol**: Form picking variant

Also contains PL/I copies of many R-series programs (possibly older versions or build artifacts).

## Shared Include Files (P00199XX series)

These are shared data structure definitions used across modules:

### Communication and Control
- **P0019906**: Transaction information area (TRANS_OPPL_OMR)
- **P0019908**: Communication area (KOM_OMR)
- **P0019910**: Control area (STYRINGS_OMR)
- **P0019912**: Various parameters (DIV_PARAM_OMR)
- **P0019991**: Security/ACF2 area

### Person Data Segments
- **P0019921**: Person master area (B01/B02) - 62,608 bytes
- **P0019926**: AFP after-settlement data
- **P0019927-P0019931**: Person identification and family
- **P0019932**: Old age pension data
- **P0019933**: Waiting periods
- **P0019934-P0019936**: Disability pension data and history
- **P0019937**: Survivor spouse data
- **P0019938**: Survivor child data
- **P0019939**: Special information
- **P0019961-P0019963**: Occupational injury data
- **P0019965**: Support allowance/income
- **P0019966**: EØS (European Economic Area) information
- **P0019971**: AFP history

### Query and Display Areas
Multiple P00199XX files define screen layouts and query result structures.

## Module Dependencies

### High-Level Flow
```
User Login (R0010101-201)
    ↓
Main Menu (R0010301)
    ↓
    ├─→ Registration (R0010401) → Person Processing (R0011XXX)
    ├─→ Inquiry (R0010410) → Query Functions (R0019XXX)
    ├─→ Calculations (R0013XXX)
    │       ├─→ Disability (R0014XXX)
    │       ├─→ Old Age (R0015XXX)
    │       ├─→ Survivor (R0016XXX)
    │       └─→ Occupational (R0017XXX)
    ├─→ Batch Processing (R0012XXX)
    └─→ Special Processing (R0018XXX)
```

### Common Dependencies
Most programs depend on:
1. **CICS services** (screen I/O, transaction control)
2. **DFHBMSCA** (BMS symbolic constants)
3. **P00199XX includes** (shared data structures)
4. **Database access** (DB2/VSAM)
5. **Map definitions** (S001XXX mapsets)

## File Size Analysis

**Largest Programs** (indicating complexity):
- R0010410.pli: 468KB - Transaction building (most complex)
- R0010430.pli: 234KB - Main processing
- R0010440.pli: 67KB - Transaction deletion
- R0010450.pli: 71KB - Transaction removal
- R0010451.pli: 50KB - Remove last transaction

These large files suggest significant business logic concentration in transaction management.

## Evolution Indicators

Evidence of system evolution:
- Comments showing multiple decade span (1997-2006 visible in samples)
- Multiple programmers (JDA2970, SPA2970, TSB2970, JDA7339, SPA7339)
- Fields added over time (marked as "NYFELT" - new field)
- Array expansions (e.g., AFPHIST from 20 to 30 elements)
- Year range extensions (PINNTEKT 1967:2010, later extended to 2014)
- EØS fields added for European coordination

## Code Characteristics

### Naming Conventions (Norwegian)
- **FNR**: Fødselsnummer (National ID number)
- **TKNR**: Trygdekontor nummer (Social security office number)
- **PI**: Pensionsgivende inntekt (Pension-qualifying income)
- **GP**: Grunnpensjon (Basic pension)
- **TP**: Tilleggspensjon (Supplementary pension)
- **ST**: Særtillegg (Special supplement)
- **BT**: Barnetillegg (Child supplement)
- **ET**: Ektefelletillegg (Spouse supplement)
- **UFG**: Uføregrad (Degree of disability)
- **YUG**: Yrkesskadens uføregrad (Occupational injury disability degree)

### Data Structure Pattern
Uses PL/I DECLARE with:
- **UNALIGNED** for compact storage
- **DEC FIXED** for packed decimal numbers
- **CHAR** for text fields
- Arrays with meaningful ranges (1967:2014 for years)
- Nested structures (3-4 levels deep)

## Modernization Considerations

When planning to modernize or refactor this system:

1. **Start with R0019**: Query/reporting is lowest risk
2. **Then R0012**: Batch processing can be isolated
3. **Preserve R0013-R0018**: Core calculation engines need careful extraction
4. **Last R0010-R0011**: Transaction processing and data entry are most integrated

Each module should be:
- Documented with its business rules
- Test cases created from production scenarios
- Data structures mapped to modern equivalents
- API boundaries defined for future service decomposition
