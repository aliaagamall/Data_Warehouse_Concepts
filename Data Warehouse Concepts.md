# Complete Data Warehousing Guide

## Table of Contents

1. [Introduction to Data Warehousing](#1-introduction-to-data-warehousing)
2. [Data Storage Systems Comparison](#2-data-storage-systems-comparison)
   - 2.1 [Data Lakes vs Data Warehouses](#21-data-lakes-vs-data-warehouses)
   - 2.2 [OLTP vs OLAP Systems](#22-oltp-vs-olap-systems)
3. [Data Warehouse Architecture](#3-data-warehouse-architecture)
   - 3.1 [Core Characteristics](#31-core-characteristics)
   - 3.2 [Types of Data Warehouses](#32-types-of-data-warehouses)
   - 3.3 [Data Warehousing Process](#33-data-warehousing-process)
4. [Dimensional Modeling](#4-dimensional-modeling)
   - 4.1 [Fundamental Concepts](#41-fundamental-concepts)
   - 4.2 [Schema Types](#42-schema-types)
   - 4.3 [Facts and Measures](#43-facts-and-measures)
   - 4.4 [Dimension Types](#44-dimension-types)
5. [ETL Process](#5-etl-process)
   - 5.1 [Data Extraction](#51-data-extraction)
   - 5.2 [Data Transformation](#52-data-transformation)
   - 5.3 [Data Loading](#53-data-loading)
6. [OLAP Systems](#6-olap-systems)
   - 6.1 [OLAP Operations](#61-olap-operations)
   - 6.2 [OLAP Implementation Models](#62-olap-implementation-models)
   - 6.3 [Analysis Types](#63-analysis-types)
7. [Business Benefits](#7-business-benefits)
8. [Glossary](#8-glossary)

---

## 1. Introduction to Data Warehousing

**Data Warehousing** is the comprehensive process of collecting, managing, and organizing data from various sources to provide meaningful business insights. It encompasses the entire lifecycle from data collection to analysis, enabling strategic decision-making through business intelligence.

**Key Distinction:**
- **Data Warehouse**: The physical repository where processed data is stored
- **Data Warehousing**: The complete process including collection, transformation, storage, and analysis

---

## 2. Data Storage Systems Comparison

### 2.1 Data Lakes vs Data Warehouses

| Dimension | Data Warehouse | Data Lake |
|-----------|----------------|-----------|
| **Data Type** | Structured, processed | Raw, native (all formats) |
| **Processing Approach** | Schema-on-write (predefined) | Schema-on-read (flexible) |
| **Query Speed** | Very fast (optimized) | Slower (requires processing) |
| **Cost** | Expensive for large volumes | Low-cost storage |
| **Agility** | Less flexible, fixed structure | Highly flexible |
| **Security** | Mature, well-secured | Improving, less mature |
| **Primary Users** | Business professionals | Data scientists |
| **Use Case** | Structured reporting, BI | Exploratory analytics, ML |

**Relationship**: Complementary technologies - warehouses excel at structured analysis while lakes handle diverse data exploration.

### 2.2 OLTP vs OLAP Systems

| Criteria | OLTP (Operational) | OLAP (Analytical) |
|----------|-------------------|-------------------|
| **Purpose** | Day-to-day operations | Decision-making analysis |
| **Data Model** | Normalized (3NF) | Denormalized (star/snowflake) |
| **Data Volatility** | Highly volatile | Non-volatile |
| **Query Focus** | Simple transactions | Complex aggregations |
| **Response Time** | Milliseconds | Seconds to minutes |
| **Data Volume** | Current data | Historical data |
| **Users** | Operational staff | Analysts, executives |
| **Redundancy** | Minimized | Intentionally added |

---

## 3. Data Warehouse Architecture

### 3.1 Core Characteristics

Data warehouses are defined by four fundamental characteristics:

```
┌─────────────────────────────────────────────────────────────┐
│                    DATA WAREHOUSE                           │
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│  │  SUBJECT-   │  │ INTEGRATED  │  │ TIME-       │          │
│  │  ORIENTED   │  │             │  │ VARIANT     │          │
│  │             │  │ Multiple    │  │             │          │
│  │ Organized   │  │ sources     │  │ Historical  │          │
│  │ by business │  │ unified     │  │ snapshots   │          │
│  │ subjects    │  │ format      │  │ with time   │          │
│  └─────────────┘  └─────────────┘  └─────────────┘          │
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│  │ NONVOLATILE │  │ WEB-BASED   │  │ METADATA    │          │
│  │             │  │             │  │             │          │
│  │ Static data │  │ Accessible  │  │ Data about  │          │
│  │ preserves   │  │ via web     │  │ data for    │          │
│  │ history     │  │ interfaces  │  │ navigation  │          │
│  └─────────────┘  └─────────────┘  └─────────────┘          │
└─────────────────────────────────────────────────────────────┘
```

**Detailed Characteristics:**

1. **Subject-Oriented**: Organized around business subjects (sales, customers, products) rather than applications
2. **Integrated**: Consolidates data from multiple sources with consistent naming and formats
3. **Time-Variant**: Stores historical data with time as a critical dimension
4. **Nonvolatile**: Data is read-only; updates create new records preserving history

### 3.2 Types of Data Warehouses

```
┌─────────────────────────────────────────────────────────────┐
│                    DATA WAREHOUSE TYPES                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │ 
│  ┌─────────────────┐    ┌─────────────────┐                 │
│  │ ENTERPRISE DW   │    │   DATA MARTS    │                 │
│  │                 │    │                 │                 │
│  │ • Organization- │    │ • Department-   │                 │
│  │   wide scope    │    │   specific      │                 │
│  │ • All subjects  │    │ • Focused       │                 │
│  │ • Central repo  │    │   subjects      │                 │
│  └─────────────────┘    └─────────────────┘                 │
│                                                             │
│  ┌─────────────────────────────────────────┐                │
│  │        OPERATIONAL DATA STORE           │                │
│  │                                         │                │
│  │ • Near real-time data                   │                │
│  │ • Staging area for DW                   │                │
│  │ • Short-term operational decisions      │                │
│  └─────────────────────────────────────────┘                │
└─────────────────────────────────────────────────────────────┘
```

**Data Mart Types:**
- **Dependent**: Built from enterprise data warehouse, ensures consistency
- **Independent**: Standalone system for specific departments, faster implementation

### 3.3 Data Warehousing Process

```
┌─────────────────────────────────────────────────────────────┐
│                 DATA WAREHOUSING PROCESS                    │
└─────────────────────────────────────────────────────────────┘
           │
           ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   DATA SOURCES  │    │   ETL PROCESS   │    │  DATA WAREHOUSE │
│                 │    │                 │    │                 │
│ • ERP Systems   │───▶│ • Extract       │───▶│ • Facts        │
│ • Legacy Systems│    │ • Transform     │    │ • Dimensions    │
│ • Web Logs      │    │ • Load          │    │ • Aggregates    │
│ • External Data │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
           │                     │                       │
           │                     ▼                       ▼
           │            ┌─────────────────┐    ┌─────────────────┐
           │            │ STAGING AREA    │    │  APPLICATIONS   │
           │            │                 │    │                 │
           │            │ • Data cleaning │    │ • Reporting     │
           │            │ • Transformation│    │ • OLAP          │
           │            │ • Validation    │    │ • Data Mining   │
           │            └─────────────────┘    │ • Dashboards    │
           │                                   └─────────────────┘
           ▼
┌─────────────────┐
│    METADATA     │
│                 │
│ • Data catalog  │
│ • Lineage info  │
│ • Business rules│
└─────────────────┘
```

---

## 4. Dimensional Modeling

### 4.1 Fundamental Concepts

**Dimensional Modeling** is a logical design technique optimized for data warehousing and business intelligence, focusing on ease of understanding and query performance.

**Core Components:**

**Facts**: Numeric measurements of business events
- **Characteristics**: Quantitative, measurable, often additive
- **Examples**: Sales amount, quantity sold, profit margin
- **Grain**: Level of detail (daily sales vs. monthly sales)

**Dimensions**: Descriptive context for facts
- **Characteristics**: Qualitative, descriptive, provide "who, what, where, when, why"
- **Examples**: Customer, Product, Time, Geography
- **Hierarchies**: Natural drill-down paths (Year → Quarter → Month → Day)

### 4.2 Schema Types

#### Star Schema
```
                    ┌─────────────────┐
                    │   TIME DIM      │
                    │                 │
                    │ • Date_Key (PK) │
                    │ • Date          │
                    │ • Year          │
                    │ • Quarter       │
                    │ • Month         │
                    └─────────────────┘
                            │
                            │
    ┌─────────────────┐     │     ┌─────────────────┐
    │ PRODUCT DIM     │     │     │ CUSTOMER DIM    │
    │                 │     │     │                 │
    │ • Prod_Key (PK) │     │     │ • Cust_Key (PK) │
    │ • Product_Name  │     │     │ • Customer_Name │
    │ • Category      │     │     │ • City          │
    │ • Brand         │─────┼─────│ • State         │
    └─────────────────┘     │     └─────────────────┘
                            │
                            │
                  ┌─────────────────┐
                  │   SALES FACT    │
                  │                 │
                  │ • Date_Key (FK) │
                  │ • Prod_Key (FK) │
                  │ • Cust_Key (FK) │
                  │ • Store_Key(FK) │
                  │ • Sales_Amount  │
                  │ • Quantity      │
                  │ • Discount      │
                  └─────────────────┘
                            │
                            │
                    ┌─────────────────┐
                    │   STORE DIM     │
                    │                 │
                    │ • Store_Key(PK) │
                    │ • Store_Name    │
                    │ • City          │
                    │ • Region        │
                    └─────────────────┘
```

**Star Schema Characteristics:**
- Central fact table surrounded by dimension tables
- Denormalized dimensions (single table per dimension)
- Simple structure, fewer joins
- Optimal query performance
- Data redundancy in dimensions

#### Snowflake Schema
```
    ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
    │   YEAR      │     │  QUARTER    │     │   MONTH     │
    │             │────▶│             │────▶│            │
    │ • Year_Key  │     │ • Qtr_Key   │     │ • Month_Key │
    │ • Year      │     │ • Quarter   │     │ • Month     │
    └─────────────┘     │ • Year_Key  │     │ • Qtr_Key   │
                        └─────────────┘     └─────────────┘
                                                    │
                                                    │
                            ┌─────────────────────────────────┐
                            │        SALES FACT               │
                            │                                 │
                            │ • Month_Key (FK)                │
                            │ • Product_Key (FK)              │
                            │ • Customer_Key (FK)             │
                            │ • Sales_Amount                  │
                            │ • Quantity                      │
                            └─────────────────────────────────┘
                                    │              │
                                    │              │
                          ┌─────────────┐  ┌─────────────┐
                          │  PRODUCT    │  │  CUSTOMER   │
                          │             │  │             │
                          │ • Prod_Key  │  │ • Cust_Key  │
                          │ • Prod_Name │  │ • Cust_Name │
                          │ • Cat_Key   │  │ • City_Key  │
                          └─────────────┘  └─────────────┘
                                  │              │
                          ┌─────────────┐  ┌────────────┐
                          │  CATEGORY   │  │    CITY    │
                          │             │  │            │
                          │ • Cat_Key   │  │ • City_Key │
                          │ • Category  │  │ • City     │
                          └─────────────┘  │ • State    │
                                           └────────────┘
```

**Snowflake Schema Characteristics:**
- Normalized dimension tables
- Reduces data redundancy
- More complex with additional joins
- Better storage efficiency
- More maintenance overhead

| Feature | Star Schema | Snowflake Schema |
|---------|-------------|------------------|
| **Structure** | Denormalized | Normalized |
| **Query Performance** | Faster (fewer joins) | Slower (more joins) |
| **Storage Space** | More (redundancy) | Less (normalized) |
| **Maintenance** | Easier | More complex |
| **Data Integrity** | Lower | Higher |

### 4.3 Facts and Measures

#### Fact Table Types

**1. Transaction Fact Tables**
```
SALES_TRANSACTION_FACT
┌─────────────┬─────────────┬──────────────┬──────────────┐
│ Date_Key    │ Product_Key │ Customer_Key │ Sales_Amount │
├─────────────┼─────────────┼──────────────┼──────────────┤
│ 20241201    │ P001        │ C123         │ 150.00       │
│ 20241201    │ P002        │ C124         │ 75.50        │
│ 20241202    │ P001        │ C125         │ 200.00       │
└─────────────┴─────────────┴──────────────┴──────────────┘
```
- **Grain**: One row per transaction
- **Growth**: Rapid, with each business event
- **Use**: Detailed transactional analysis

**2. Periodic Snapshot Fact Tables**
```
MONTHLY_SALES_FACT
┌─────────────┬─────────────┬──────────────┬──────────────────┐
│ Month_Key   │ Product_Key │ Store_Key    │ Total_Sales      │
├─────────────┼─────────────┼──────────────┼──────────────────┤
│ 202411      │ P001        │ S001         │ 5000.00          │
│ 202411      │ P002        │ S001         │ 3200.00          │
│ 202412      │ P001        │ S001         │ 5500.00          │
└─────────────┴─────────────┴──────────────┴──────────────────┘
```
- **Grain**: One row per time period
- **Growth**: Predictable, based on time periods
- **Use**: Trend analysis, periodic reporting

**3. Accumulating Snapshot Fact Tables**
```
ORDER_LIFECYCLE_FACT
┌───────────┬──────────────┬──────────────┬──────────────┬─────────────────┐
│ Order_Key │ Order_Date   │ Ship_Date    │ Delivery_Date│ Days_to_Deliver │
├───────────┼──────────────┼──────────────┼──────────────┼─────────────────┤
│ O001      │ 2024-12-01   │ 2024-12-03   │ 2024-12-05   │ 4               │
│ O002      │ 2024-12-02   │ 2024-12-04   │ NULL         │ NULL            │
└───────────┴──────────────┴──────────────┴──────────────┴─────────────────┘
```
- **Grain**: One row per process instance
- **Growth**: Updates existing rows as process progresses
- **Use**: Process analysis, milestone tracking

#### Measure Types

| Measure Type | Description | Example | Aggregation |
|--------------|-------------|---------|-------------|
| **Additive** | Can sum across all dimensions | Sales Amount | SUM across all |
| **Semi-Additive** | Can sum across some dimensions | Account Balance | SUM across accounts, not time |
| **Non-Additive** | Cannot sum meaningfully | Profit Margin (%) | Cannot SUM |
| **Derived** | Calculated from other measures | Total = Qty × Price | Computed |

### 4.4 Dimension Types

#### Essential Dimension Types

**1. Conformed Dimensions**
- Shared across multiple fact tables
- Ensures consistent analysis
- Example: Date dimension used by Sales and Inventory facts

**2. Slowly Changing Dimensions (SCD)**

**Type 1 (Overwrite):**
```
BEFORE: Customer_ID: 123, Name: "John", City: "Cairo"
AFTER:  Customer_ID: 123, Name: "John", City: "Alexandria"
```
- Loses historical data
- Simple implementation

**Type 2 (Add New Row):**
```
┌─────────────┬──────┬────────────┬──────────────┬────────────────┬───────────┐
│ Customer_Key│ ID   │ Name       │ City         │ Effective_Date │ Is_Current│
├─────────────┼──────┼────────────┼──────────────┼────────────────┼───────────┤
│ 1001        │ 123  │ John       │ Cairo        │ 2023-01-01     │ False     │
│ 1002        │ 123  │ John       │ Alexandria   │ 2024-06-01     │ True      │
└─────────────┴──────┴────────────┴──────────────┴────────────────┴───────────┘
```
- Preserves complete history
- Most common approach

**3. Junk Dimensions**
- Combines low-cardinality flags/codes
- Reduces fact table complexity

```
TRANSACTION_FLAGS_DIM
┌─────────────┬──────────────┬──────────────┬─────────────┐
│ Flag_Key    │ Payment_Type │ Promotion    │ Gift_Wrap   │
├─────────────┼──────────────┼──────────────┼─────────────┤
│ 1           │ Credit Card  │ Discount     │ Yes         │
│ 2           │ Cash         │ None         │ No          │
│ 3           │ Debit        │ BOGO         │ Yes         │
└─────────────┴──────────────┴──────────────┴─────────────┘
```

**4. Role-Playing Dimensions**
- Single dimension used in multiple roles
- Example: Date dimension as Order_Date, Ship_Date, Delivery_Date

---

## 5. ETL Process

ETL (Extract, Transform, Load) is the backbone process that feeds data from source systems into the data warehouse.

### 5.1 Data Extraction

```
┌─────────────────────────────────────────────────────────────┐
│                    DATA EXTRACTION                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  SOURCE SYSTEMS           EXTRACTION METHODS                │
│                                                             │
│  ┌─────────────────┐      ┌─────────────────┐               │
│  │ • ERP Systems   │ ────▶ • Full Extract
│  │ • Legacy DB     │      │ • Incremental   │               │
│  │ • Web Logs      │      │ • Change Data   │               │
│  │ • External APIs │      │   Capture (CDC) │               │
│  │ • Flat Files    │      └─────────────────┘               │
│  └─────────────────┘                                        │
│                                                             │
│  EXTRACTION CONSIDERATIONS:                                 │
│  • Source identification and data mapping                   │
│  • Extraction frequency (daily, weekly, real-time)          │
│  • Job sequencing and dependencies                          │
│  • Error handling and data quality checks                   │
└─────────────────────────────────────────────────────────────┘
```

### 5.2 Data Transformation

**Key Transformation Operations:**

| Operation | Description | Example |
|-----------|-------------|---------|
| **Selection** | Choose specific records/fields | Select active customers only |
| **Conversion** | Standardize formats | Date: MM/DD/YYYY → YYYY-MM-DD |
| **Joining** | Combine data from multiple sources | Merge customer and order data |
| **Splitting** | Separate single fields | Full Name → First Name + Last Name |
| **Calculation** | Derive new values | Total = Quantity × Unit Price |
| **Aggregation** | Summarize data | Daily sales → Monthly totals |
| **Cleansing** | Fix data quality issues | Remove duplicates, fix nulls |
| **Enrichment** | Add business value | Add customer demographics |

**Data Quality Focus Areas:**

```
┌───────────────────────────────────────────────────────────┐
│                  DATA QUALITY DIMENSIONS                  │
├───────────────────────────────────────────────────────────┤
│                                                           │
│  ACCURACY    │  COMPLETENESS │  CONSISTENCY │  VALIDITY   │
│              │               │              │             │
│  • Correct   │  • No missing │  • Same      │  • Follows  │
│    values    │    values     │    format    │    rules    │
│  • Factual   │  • All fields │    across    │  • Valid    │
│    data      │    populated  │    sources   │    ranges   │
│              │               │              │             │
└───────────────────────────────────────────────────────────┘
```

### 5.3 Data Loading

**Loading Strategies:**

```
LOADING METHODS
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  INITIAL LOAD   │    │ INCREMENTAL     │    │  FULL REFRESH   │
│                 │    │      LOAD       │    │                 │
│ • First-time    │    │                 │    │ • Complete      │
│   population    │    │ • New/changed   │    │   replacement   │
│ • All historical│    │   records only  │    │ • Periodic      │
│   data          │    │ • Daily/hourly  │    │   rebuild       │
│ • One-time      │    │ • Efficient     │    │ • Clean slate   │
│   process       │    │   updates       │    │   approach      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

**Loading Techniques:**

| Technique | Action | Use Case |
|-----------|--------|----------|
| **Load** | Replace all data | Initial setup, full refresh |
| **Append** | Add new records | Log data, transaction records |
| **Destructive Merge** | Update existing, add new | Master data updates |
| **Constructive Merge** | Keep history, add new versions | SCD Type 2 implementation |

---

## 6. OLAP Systems

Online Analytical Processing (OLAP) enables multidimensional analysis of business data through interactive querying and reporting.

### 6.1 OLAP Operations

```
                    OLAP CUBE OPERATIONS
                    
         ROLL-UP (Summarize)              DRILL-DOWN (Detail)
              ▲                                   ▼
    ┌────────────────────────┐    ┌─────────────────────────┐
    │   QUARTERLY SALES      │    │    DAILY SALES          │
    │                        │    │                         │
    │ Q1: $50,000            │    │ Jan 1: $500             │
    │ Q2: $75,000            │    │ Jan 2: $750             │
    │ Q3: $60,000            │    │ Jan 3: $600             │
    │ Q4: $80,000            │    │ ...                     │
    └────────────────────────┘    └─────────────────────────┘
                    
         SLICE (Filter)               DICE (Multi-filter)
              ▼                                   ▼
    ┌─────────────────────────┐    ┌───────────────────────┐
    │   PRODUCT P1 ONLY       │    │ PRODUCTS P1,P2        │
    │                         │    │ STORES S1,S2          │
    │ Store1: $10,000         │    │ Q1-Q2 ONLY            │
    │ Store2: $15,000         │    │                       │
    │ Store3: $12,000         │    │ Filtered Results:     │
    └─────────────────────────┘    │ P1,S1,Q1: $5,000      │
                                   │ P1,S2,Q2: $8,000      │
                                   └───────────────────────┘
```

**OLAP Operation Details:**

| Operation | Description | Example |
|-----------|-------------|---------|
| **Roll-Up** | Aggregate to higher level | Daily → Monthly sales |
| **Drill-Down** | Break down to lower level | Regional → Store sales |
| **Slice** | Filter one dimension | Show only Q1 data |
| **Dice** | Filter multiple dimensions | Products A,B in Stores 1,2 |
| **Pivot** | Rotate data presentation | Rows ↔ Columns |

### 6.2 OLAP Implementation Models

```
┌────────────────────────────────────────────────────────────┐
│                    OLAP MODELS COMPARISON                  │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  MOLAP              ROLAP              HOLAP               │
│  (Multidimensional) (Relational)       (Hybrid)            │
│                                                            │
│  ┌─────────────┐     ┌─────────────┐   ┌─────────────┐     │
│  │Pre-computed │     │On-the-fly   │   │Aggregates   │     │
│  │aggregates   │     │calculations │   │in cubes     │     │
│  │in cubes     │     │in RDBMS     │   │             │     │
│  │             │     │             │   │Details in   │     │
│  │Fastest      │     │Most         │   │RDBMS        │     │
│  │queries      │     │scalable     │   │             │     │
│  │             │     │             │   │Balanced     │     │
│  │Limited      │     │Slower       │   │approach     │     │
│  │flexibility  │     │queries      │   │             │     │
│  └─────────────┘     └─────────────┘   └─────────────┘     │
└────────────────────────────────────────────────────────────┘
```

| Characteristic | MOLAP | ROLAP | HOLAP |
|----------------|-------|--------|-------|
| **Query Speed** | Fastest | Fast | Faster |
| **Scalability** | Limited by cube size | High | High |
| **Flexibility** | Low (rebuild required) | High | Medium |
| **Storage** | High (redundant data) | Efficient | Balanced |
| **Complexity** | Low | Medium | High |
| **Best For** | Predefined analysis | Large datasets | Mixed requirements |

### 6.3 Analysis Types

| Analysis Type | Characteristics | Use Cases |
|---------------|-----------------|-----------|
| **Ad Hoc** | • Spontaneous, one-off queries<br>• Custom reports<br>• Immediate business needs | • Investigating anomalies<br>• Special requests<br>• Exploratory analysis |
| **Interactive** | • Predefined dashboards<br>• Real-time interaction<br>• Iterative exploration | • Performance monitoring<br>• Trend analysis<br>• Regular reporting |

---

## 7. Business Benefits

Data warehousing delivers tangible business value across multiple dimensions:

**Cost Benefits:**
- Reduced infrastructure costs through consolidation
- Elimination of redundant systems
- Streamlined reporting processes

**Quality Improvements:**
- Enhanced data accuracy and consistency
- Improved decision-making capabilities
- Faster issue identification and resolution

**Strategic Advantages:**
- Unified view of business performance
- Historical trend analysis capabilities
- Standardized IT architecture
- Compliance and regulatory reporting support

---

## 8. Glossary

**Accumulating Snapshot Fact Table**: A fact table that tracks the lifecycle of a business process, updating a single row as the process progresses through various milestones.

**Additive Facts**: Numeric measures that can be meaningfully summed across all dimensions (e.g., sales amount, quantity).

**Change Data Capture (CDC)**: A technique to identify and capture changes in source data for incremental loading into the data warehouse.

**Conformed Dimension**: A dimension that is shared across multiple fact tables with consistent definitions and attributes, ensuring uniform analysis.

**Data Lake**: A centralized repository that stores vast amounts of raw data in its native format until needed for analysis.

**Data Mart**: A subset of a data warehouse focused on a particular subject area or department, containing relevant data for specific user groups.

**Data Mining**: The process of discovering patterns, correlations, and insights from large datasets using statistical and machine learning techniques.

**Data Warehouse**: A centralized repository of integrated data from multiple sources, designed for query and analysis rather than transaction processing.

**Data Warehousing**: The complete process of designing, building, and maintaining a data warehouse infrastructure.

**Degenerate Dimension**: A dimension key stored in a fact table without a corresponding dimension table, typically used for grouping related transactions.

**Denormalization**: The process of combining normalized tables to reduce joins and improve query performance, often used in dimensional modeling.

**Dimension**: Descriptive attributes that provide context to facts, answering "who, what, where, when, why" questions about business events.

**Dimensional Modeling**: A logical design technique that presents data in a standard framework that is intuitive and allows for high-performance access.

**Drill-Down**: An OLAP operation that navigates from summarized data to more detailed data by moving down a concept hierarchy.

**Enterprise Data Warehouse (EDW)**: A centralized data warehouse that serves the entire organization's analytical needs.

**ETL (Extract, Transform, Load)**: The process of extracting data from source systems, transforming it to fit business requirements, and loading it into a data warehouse.

**Fact**: A numeric measurement or metric that represents a business event or performance indicator.

**Fact Table**: The central table in a dimensional model that contains the facts and foreign keys to dimension tables.

**Grain**: The level of detail represented by each row in a fact table, defining what business event is measured.

**HOLAP (Hybrid OLAP)**: An OLAP implementation that combines MOLAP and ROLAP approaches, storing aggregates in multidimensional structures and detailed data in relational databases.

**Junk Dimension**: A dimension that combines multiple low-cardinality flags or indicators to avoid having many small dimension tables.

**Metadata**: Data about data, including structure, content, keys, indexes, and other information that describes the data warehouse contents.

**MOLAP (Multidimensional OLAP)**: An OLAP implementation that stores data in optimized multidimensional cube structures for fast query performance.

**Non-Additive Facts**: Measures that cannot be meaningfully summed across any dimension, such as ratios, percentages, or averages.

**Normalization**: A database design technique that reduces data redundancy by organizing data into multiple related tables.

**OLAP (Online Analytical Processing)**: A category of software tools that provides analysis of data from multiple perspectives and supports complex analytical queries.

**OLAP Cube**: A multidimensional data structure that enables fast analysis of data from multiple perspectives.

**OLTP (Online Transaction Processing)**: A class of systems that manages transaction-oriented applications, optimized for high-volume, real-time transaction processing.

**Operational Data Store (ODS)**: A database designed to integrate data from multiple sources for operational reporting and as a staging area for the data warehouse.

**Periodic Snapshot Fact Table**: A fact table that captures data at regular intervals, showing the state of business metrics at specific points in time.

**Pivot**: An OLAP operation that rotates the data axes to view information from different perspectives.

**ROLAP (Relational OLAP)**: An OLAP implementation that stores data in relational databases and uses SQL for queries and analysis.

**Role-Playing Dimension**: A single physical dimension that appears multiple times in the same fact table, each time with a different logical meaning.

**Roll-Up**: An OLAP operation that summarizes data by moving up a concept hierarchy or by reducing the number of dimensions.

**Schema-on-Read**: An approach where data structure is applied when the data is read, allowing flexibility in data formats (used by data lakes).

**Schema-on-Write**: An approach where data structure is defined before data is written, ensuring data quality and consistency (used by data warehouses).

**Semi-Additive Facts**: Measures that can be summed across some dimensions but not others, such as account balances that shouldn't be summed across time.

**Slice**: An OLAP operation that creates a subset of a multidimensional array by fixing one or more dimensions at specific values.

**Slowly Changing Dimension (SCD)**: A dimension that handles changes in attribute values over time, with different types defining how historical data is preserved.

**Snowflake Schema**: A dimensional model where dimension tables are normalized into multiple related tables, resembling a snowflake pattern.

**Star Schema**: A dimensional model with a central fact table connected to denormalized dimension tables, resembling a star pattern.

**Staging Area**: A temporary storage area where data is held during the ETL process for cleansing, transformation, and validation.

**Subject-Oriented**: A characteristic of data warehouses where data is organized around major business subjects rather than applications.

**Surrogate Key**: An artificial key used in dimension tables, typically a sequential integer that has no business meaning.

**Time-Variant**: A characteristic of data warehouses where data represents snapshots over time, allowing historical analysis.

**Transaction Fact Table**: A fact table that records individual business events or transactions as they occur.