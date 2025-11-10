# Enterprise Data Warehouse - AdventureWorks

Metadata-driven data warehouse with automated ETL framework and Slowly Changing Dimensions.

## Overview

Production-ready data warehouse featuring metadata-driven architecture, comprehensive ETL control framework, and multiple SCD types for sales analytics. Implements 4-layer architecture processing 121,317+ sales transactions with complete audit trails and incremental loading.

## Architecture

**4-Layer Design:**
```
Source (OLTP) → Integration (ETL Control) → Staging → Production (Star Schema)
```

**Star Schema:**
- **5 Dimensions**: DimCustomer, DimProduct, DimSalesperson, DimAddress, DimDate
- **1 Fact Table**: FactSales (121,317+ records)
- **Surrogate Keys**: Auto-generated for all dimensions
- **Referential Integrity**: Full FK constraints

## Key Features

### Metadata-Driven Development
- Dynamic DDL generation from Excel metadata
- 19 stored procedures (3,500+ LOC)
- Self-documenting architecture
- Scalable framework design

### Slowly Changing Dimensions
**SCD Type 0** (DimAddress): Fixed reference data  
**SCD Type 1** (StoreName, TerritoryID): Current values only  
**SCD Type 2** (FirstName, LastName, ListPrice): Full history with IsCurrent flag and validity dates

### Change Data Capture Patterns
- Timestamp-based change detection
- Cutoff date management for incremental loads
- Watermark tracking per table
- Delta identification (source vs target comparison)

### ETL Control Framework
**ETL Lineage Table:**
- Tracks execution metrics: row counts, duration, success/failure
- Complete audit trail for all loads
- Performance monitoring and troubleshooting

**ETL Cutoff Table:**
- Stores last successful load timestamp per table
- Enables incremental processing
- Reduces load times by processing only changed data

### Performance
- Sub-5 second incremental loads
- Parallel execution via SSIS
- Indexed dimensions for fast lookups
- Set-based processing

## Technology Stack

| Component | Technology |
|-----------|------------|
| **Platform** | SQL Server 2022 |
| **ETL** | SSIS + T-SQL Stored Procedures |
| **Architecture** | 4-layer dimensional model |
| **Development** | Metadata-driven automation |

## Implementation Metrics

**Code Base:**
- 43 SQL files organized in logical structure
- 312 KB of T-SQL code
- 19 stored procedures
- 1 SSIS master orchestration package

**Data Processing:**
- 30,000+ source OLTP records
- 15,000+ dimensional records
- 121,317+ fact records
- Sub-5 second incremental loads

## ETL Processing Workflow

### Incremental Load Process
1. Retrieve cutoff date from ETL_Cutoff table
2. Extract delta records modified after cutoff
3. Load to staging for validation
4. Compare staging vs production for changes
5. Apply SCD logic per attribute type
6. Load fact table with dimensional FK lookups
7. Record metrics in ETL_Lineage
8. Update cutoff date to current timestamp

### Dimension Processing
- **New records**: Insert with IsCurrent=1, ValidityStartDate=LoadDate
- **SCD Type 1**: Direct UPDATE to existing record
- **SCD Type 2**: UPDATE old (IsCurrent=0, ValidityEndDate=LoadDate), INSERT new (IsCurrent=1)

## Repository Structure

```
├── 01-Schemas/
├── 02-Tables/
│   ├── Integration/    # ETL control tables
│   ├── Staging/        # Temporary load tables
│   └── Production/     # Star schema tables
├── 03-StoredProcedures/
│   ├── Dimensions/     # SCD processing (5)
│   ├── Facts/          # Fact load (1)
│   ├── Integration/    # ETL control (3)
│   └── Staging/        # Staging load (10)
├── 04-SSIS/            # Master package
└── 05-Metadata/        # Excel metadata
```

## Skills Demonstrated

<table>
<tr>
<td width="55%" valign="top">

**Data Warehousing**
- Star schema dimensional modeling
- Fact and dimension design
- Surrogate key management
- Referential integrity

**ETL Architecture**
- Metadata-driven framework
- 4-layer architectural pattern
- Staging layer implementation
- Error handling and recovery

**Change Data Capture**
- Timestamp-based CDC patterns
- Watermark management
- Delta identification
- Incremental load strategies

**SCD Implementation**
- SCD Type 0, 1, 2 logic
- Historical tracking with validity dates
- IsCurrent flag management
- Change detection algorithms

</td>
<td width="45%" valign="top">

**SQL Development**
- T-SQL programming (3,500+ LOC)
- Stored procedure development
- Dynamic SQL generation
- Set-based processing

**Performance Optimization**
- Incremental loading patterns
- Parallel execution strategies
- Index optimization
- Query performance tuning

**SSIS Development**
- Package orchestration
- Control flow design
- Error handling
- Logging and auditing

**Data Quality**
- Type casting and validation
- Deduplication logic
- Referential integrity checks
- Audit trail implementation

</td>
</tr>
</table>