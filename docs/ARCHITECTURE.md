# DSF System Architecture

## Overview

**DSF (Det Sentrale Folketrygdsystemet)** - The Central National Insurance System - was one of the oldest IT systems at NAV (Norwegian Labour and Welfare Administration). It ran in production for 51 years, from the introduction of the Norwegian National Insurance scheme in 1967 until January 2018, when it was replaced by Presys and Pesys.

## System Information

- **Original Language**: PL/I (Programming Language One)
- **Deployment**: IBM Mainframe with CICS (Customer Information Control System)
- **Operational Period**: 1967 - 2018
- **Total Source Files**: ~1,481 files
- **Primary Language Files**: 717 PL/I source files
- **Supporting Files**: COBOL utilities in GML directory

## Technology Stack

### Primary Technologies
- **PL/I**: Main programming language for business logic
- **CICS**: Transaction processing system
- **IBM DB2/VSAM**: Database systems
- **BMS (Basic Mapping Support)**: Screen management

### Development Environment
- IBM Mainframe environment
- TSO (Time Sharing Option) for development
- CICS regions for transaction processing

## System Characteristics

### Scale
- Over 700 PL/I programs
- Approximately 1,481 source files total
- Handles Norway's entire national insurance data
- Processes pension calculations, disability benefits, survivor benefits

### Architecture Pattern
- **Transaction-based**: Each user interaction is a CICS transaction
- **Menu-driven**: Hierarchical menu system for navigation
- **Online/Interactive**: Real-time user interaction with immediate database updates
- **Batch processing**: Supplementary batch jobs for mass operations

## Data Structures

### Main Data Areas
Based on database.txt and program includes, the system uses structured data areas:

- **Person Data (PERSON)**: Core identity and demographic information
- **Income Data (PINNTEKT)**: Pension-qualifying income by year
- **Pension Status (STATUS)**: Current pension status and codes
- **Family Connections (TILKN)**: Spouse, children, parents relationships
- **Pension Details**: Old age pension, disability pension, survivor pension
- **EØS Information (EØSINFO)**: European Economic Area coordination

### Shared Data Structures (P00199XX series)
The system uses many shared include files for data structures:
- P0019906: Transaction information area (BASED)
- P0019908: Communication area (BASED) 
- P0019910: Control area (BASED)
- P0019912: Various parameters area
- P0019921 - P0019971: Specific segment definitions

## Historical Context

This system is a remarkable piece of software engineering history:
- Maintained the pension records for the entire Norwegian population
- Survived multiple hardware and software migrations
- Adapted to numerous legislative changes over 51 years
- Successfully handled Norway's demographic growth
- Represents institutional knowledge of Norwegian welfare policy

## Modern Considerations

When studying this system for modernization:
1. **Business Logic Preservation**: The calculations and rules encode 51 years of policy
2. **Data Migration**: Person records span decades and must be perfectly preserved
3. **Regulatory Compliance**: Must maintain audit trails and historical accuracy
4. **Incremental Approach**: Cannot be replaced overnight due to risk
5. **Knowledge Transfer**: Original developers likely retired; documentation crucial
