# Data Warehouse Toolkit 3rd Edition 
The Data Warehouse Toolkit Third Edition: The Definitive Guide to Dimensional Modeling. Covering a multitude of topics such as Data Warehousing, Business Intelligence, Kimball Dimension Modeling, ETL System Design and Development, and Big Data Analytics. 

## Kimball Dimensional Modelling Techniques Overview
> The Kimball techniques have been accepted as industry best practices. This section will provide a basic overview of the official techniques and design patterns (which include allusions to other write-ups for more detail). This section should primarily serve as a reference guide for data warehouse design and development 

### Fundamental Concepts 

#### Gather Business Requirements and Data Realities 
In dimensional modeling the development team needs to understand the needs of the business, as well as the realities of the underlying source data. References for review of this concept: 
* Chapter 1 (pg. 5), Chapter 3 (pg. 70), Chapter 11 (pg. 297), Chapter 17 (pg. 412), Chapter 18 (pg. 431), Chapter 19 (pg. 444)

#### Collaborative Dimensional Modeling Workshops 
Dimensional models should be designed in collaboration with subject matter experts and data governance representatives from the business. Workshops provide another opportunity to flesh out the requirements with the business. 
* Chapter 3 (pg. 70), Chapter 4 (pg. 135), Chapter 18 (pg. 429)

#### Four-Step Dimensional Design Process 
The four key decisions made during the design of a dimensional model include: 
* Select the business process 
* Declare the grain 
* Identify the dimensions 
* Identify the facts 

Answers to these questions are determined by considering the needs of the business along with the realities of the underlying source data during the collaborative modeling sessions. Business data governance representatives must participate in this detailed design activity to ensure business buy-in. 
* Chapter 3 (pg. 70), Chapter 11 (pg. 300), Chapter 18 (pg. 434)

#### Business Process 
Business processes are the operational activities performed by your organization such as taking an order , processing an insurance claim, registering students for a class, or snapshotting every account each month. Each business process corresponds to a row in the enterprise data warehouse bus matrix. 
* Chapter 1 (pg. 10), Chapter 3 (pg. 70), Chapter 17 (pg. 414), Chapter 18 (pg. 435)

#### Grain 
Declaring the grain is a pivotal step in a dimensional design. The _grain_ establishes exactly what a single fact table row represents. The grain declaration becomes a binding contract on the design. The grain must be declared before choosing dimensions or facts because every candidate dimension or fact must be consistent with the grain. This consistency enforces a uniformity on all dimensional designs that is critical to BI application performance and ease of use. _Atomic grain_ refers to the lowest level at which data is captures by a given business process. 
* Chapter 1 (pg. 30), Chapter 3 (pg. 71), Chapter 4 (pg. 112), Chapter 6 (pg. 184), Chapter 11 (pg. 300), Chapter 12 (pg. 312), Chapter 18 (pg. 435)

#### Dimensions for Descriptive Context 
Dimensions provide the context surrounding a business process event. Dimension tables contain the descriptive attributes used by BI applications for filtering and grouping the facts. With the grain of a fact table firmly in mind, all the possible dimensions can be identified. Dimension tables contain the entry points and descriptive labels that enable the DW/BI system to be leveraged for business analysis. 
* Chapter 1 (pg. 13), Chapter 3 (pg. 72), Chapter 11 (pg. 301), Chapter 18 (pg. 437), Chapter 19 (pg. 463)

#### Facts for Measurements 
Facts are the measurements that result from a business process event and are almost always numeric. A single fact table row has a one-to-one relationship to a measurement event as described by the fact table's grain. Fact tables correspond to a physical observable event and not to the demands of the of a particular report. Within a fact table, only facts consistent with the declared grain are allowed. 
* Chapter 1 (pg. 10), Chapter 3 (pg. 72), Chapter 4 (pg. 112), Chapter 18 (pg. 437)

#### Star Schemas and OLAP Cubes 
_Star schemas_ are dimensional structures deployed in a relational database management system (RDBMS). The characteristically consist of fact tables linked to associated dimension tables via primary/foreign key relationships. An _Online Analytical Processing Cube_ (OLAP Cube) is a dimensional structure implemented in a multidimensional database; it can be equivalent in content to a relational star schema. OLAP cubes contains dimensional attributes and facts but it is accessed through languages with analytical capabilities (typically XMLA and MDX). 
* Chapter 1 (pg. 8), Chapter 3 (pg. 94), Chapter 5 (pg. 149), Chapter 6 (pg. 170), Chapter 7 (pg. 226), Chapter 9 (pg. 273), Chapter 13 (pg. 335), Chapter 19 (pg. 481), Chapter 20 (pg. 519)

#### Graceful Extensions to Dimensional Models 
Dimensional models are resilient when data relationships change. 
* Facts consistent with the grain of an existing fact table can be added by creating new columns
* Dimensions can be added to an existing fact table by creating new foreign key columns, presuming they don't alter the fact table's grain
* Attributes can be added to an existing dimension table by creating new columns 
* The grain of a fact table can be made more atomic by adding attributes to an existing dimension table, and then restating the fact table at the lower grain, being careful to preserve the existing column names in the fact and dimension tables 
* Chapter 3 (pg. 95)

### Basic Fact Table Techniques 

#### Fact Table Structure
A fact table contains the numeric measures produced by an operational measurement event in the real world. At the lowest grain a fact table row corresponds to a measurement event and vice versa. Fact tables are the primary target of computations and dynamic aggregations arising from queries. 
* Chapter 1 (pg. 10), Chapter 3 (pg. 76), Chapter 5 (pg. 143), Chapter 6 (pg. 169)

#### Additive, Semi-Additive, Non-Additive Facts 
The numeric measures in a fact table fall into three categories. The most flexible and useful facts are fully _Additive_; additive measures can be summed across any of the dimensions associated with the fact table. _Semi-Additive_ measures can be summed across some dimensions, but not all; balance amounts are common semi-additive facts because they are additive across all dimensions except time. Finally, some measures are completely _Non-Additive_, such as ratios. 
* Chapter 1 (pg. 10), Chapter 3 (pg. 76), Chapter 4 (pg. 114), Chapter 7 (pg. 204)

#### Nulls in Fact Tables 
Null-valued measurements behave gracefully in fact tables, however nulls must be avoided in the fact table's foreign keys because these nulls would automatically cause a referential integrity violation. Rather than a null foreign key, the associated dimension table must have a default row (and surrogate key) representing the unknown or not applicable condition. 
* Chapter 3 (pg. 92), Chapter 20 (pg. 509)

#### Conformed Facts 
If the same measurement appears in separate fact tables, care must be taken to make sure the technical definitions of the facts are identical igf they are to be compared or computed together. If the separate fact definitions are consistent, the conformed facts should be identically named; but if they are incompatible, they should be differently named to alert the business users and BI applications. 
* Chapter 4 (pg. 138), Chapter 16 (pg. 386)

#### Transaction Fact Tables 
A row in a transaction fact table corresponds to a measurement event at a point in space and time. Atomic transaction grain fact tables are the most dimensional and expressive fact tables; this robust dimensionality enables the maximum slicing and dicing of transaction data. Transaction fact tables may be dense or sparse because rows exist only if measurements take place. These fact tables always contain a foreign key for each associated dimension, and potentially contain precise time stamps and degenerate dimension keys. The measured numeric facts must be consistent with the transaction grain. 
* Chapter 3 (pg. 79), Chapter 4 (pg. 116), Chapter 5 (pg. 142), Chapter 6 (pg. 168), Chapter 7 (pg. 206), Chapter 11 (pg. 306), Chapter 12 (pg. 312), Chapter 14 (pg. 351), Chapter 15 (pg. 363), Chapter 16 (pg. 379), Chapter 19 (pg. 473)

#### Periodic Snapshot Fact Tables 
A _periodic snapshot fact table_ summarizes many measurement events occurring over a standard period. The grain is the period, not the individual transaction. Periodic snapshot fact tables often contain many facts because any measurement event conistent with the fact table grain is permissible. These fact tables are uniformly dense in their foreign keys because even if no activity takes place during the period, a row typically inserted n the fact table containing a zero or null for each fact. 
* Chapter 4 (pg. 113), Chapter 7 (pg. 204), Chapter 9 (pg. 267), Chapter 10 (pg. 283), Chapter 13 (pg. 333), Chapter 14 (pg. 351), Chapter 16 (pg. 385), Chapter 19 (pg. 474)

#### Accumulating Snapshot Fact Tables 
A row in an _accumulating snapshot fact table_ summarizes the measurement events occurring at predictable steps between the beginning and the end of a process. Pipeline or workflow processes can be modeled with this type of fact table. There is a data foreign key in the fact table for each critical milestone in the process. An individual in an accumulating snapshot fact table, corresponding for instance to a line on an order, is initially inserted when the order line is created. As pipeline processes occur the accumulating snapshot fact table is revisited and updated. In addition to the date foreign keys associated with each critical process step, accumulating snapshot fact tables contain foreign keys for other dimensions and optionally contain degenerate dimensions. They often include numeric lag measurements consistent with the grain, along with milestone completion counters. 
* Chapter 4 (pg. 118), Chapter 5 (pg. 147), Chapter 6 (pg. 194), Chapter 13 (pg. 326), Chapter 14 (pg. 342), Chapter 16 (pg. 392), Chapter 19 (pg. 475)

#### Factless Fact Tables 
Although most measurement events capture numerical results, it is possible that the event merely records a set of dimensional entities coming together at a moment in time. _Factless Fact Tables_ can also be used to analyze what didn't happen. These queries always have two parts: a factless coverage table that contains all the possibilities of events that might happen and an activity table that contains the events that did happen. When the activity is subtracted from the coverage, the result is the set of events that did not happen. 
* Chapter 3 (pg. 97), Chapter 6 (pg. 176), Chapter 13 (pg. 329), Chapter 16 (pg. 396)

#### Aggregate Fact Tables and OLAP Cubes
_Aggregate Fact Tables_ are simple numeric rollups of atomic fact table data built solely to accelerate query performance. These aggregate fact table should be available to the BI layer at the same time as the atomic fact table so that BI tool smoothly choose the appropriate aggregate level at query time. This process, known as _aggregate navigation_, must be open so that every report writer, query tool, and BI application harvests the same performance benefits. A properly designed set of aggregates should behave like database indexes, which accelerate query performance but are not encountered directly by the BI applications or business users. Aggregate fact tables contain foreign keys to shrunken conformed dimensions, as well as aggregated facts created by summing measures form more atomic fact tables. 
* Chapter 15 (pg. 366), Chapter 19 (pg. 481), Chapter 20 (pg. 519)

#### Consolidated Fact Tables 
It is convenient to combine multiple fact tables together if they can be expressed at the same grain. These are known as _Consolidated Fact Tables_. Consolidated Fact Tables add burden to the ETL processing, but ease the analytic burden on the BI application. 
* Chapter 7 (pg. 224), Chapter 16 (pg. 395)

### Basic Dimension Table Techniques 

#### Dimension Table Structure 
Every dimension table has a single primary key column. This primary key is embedded as a foreign key in any associated fact table where the dimension row's descriptive context is exactly correct for that fact table row. Dimension tables are usually wide, flat, denormalized tables with many low-cardinality text attributes. Dimension table attributes are the primary target of constraints and grouping specifications from queries and BI applications. The descriptive labels on reports are typically dimension attribute domain values. 
* Chapter 1 (pg. 13), Chapter 3 (pg. 79), Chapter 11 (pg. 301)

#### Dimension Surrogate Keys 
A dimension table is designed with one column serving as a unique primary key. This primary key cannot be the operational system's natural key because there will be multiple dimension rows for that natural key when changes are tracked over time. The DW/BI system needs to claim control of the primary keys of all dimensions; rather than using explicit natural keys or natural keys with appended dates, you should create anonymous integer primary keys for every dimension. These dimension surrogate keys are simple integers, assigned in sequence, starting with the value 1, every time a new key is needed.
* chapter 3 (pg. 98), Chapter 19 (pg. 469), Chapter 20 (pg. 506)

#### Natural, Durable and Supernatural Keys 
Natural keys created by operational source systems are subject to business rules outside the control of the DW/BI system. The best durable keys have a format that is independent of the original business process and thus should be simple integers assigned in sequence beginning with 1. 
* Chapter 3 (pg. 100), Chapter 20 (pg. 510), Chapter 21 (pg. 539)

#### Drilling Down 
_Drilling Down_ is the most fundamental way data is analyzed by business users. It simply means adding a row header to an existing query; the new row header is a dimension attribute appended to the `GROUP BY` extension in an SQL query. 
* Chapter 3 (pg. 86)

#### Degenerate Dimensions 
_Degenerate Dimensions_ are most common with transaction and accumulating snapshot fact tables. Degenerate dimensions are dimensions that are defined but have no content except for its primary key.  
* Chapter 3 (pg. 93), Chapter 6 (pg. 178), Chapter 11 (pg. 303), Chapter 16 (pg. 383)

#### Denormalized Flattened Dimensions 
In general, dimensional designers must resist the normalization urges caused by years of operational database designs and instead denormalize the many-to-one fixed depth hierarchies into separate attributes on a flattened dimension row. 
* Chapter 1 (pg. 13), Chapter 3 (pg. 84)

#### Multiple Hierarchies in Dimensions 
Many dimensions may contain more than one natural hierarchy. The separate hierarchies can gracefully coexist in the same dimension table. 
* Chapter 3 (pg. 88), Chapter 19 (pg. 470)

#### Flags and Indicators as Textual Attributes 
Operational codes with embedded meaning within the code value should be broken down with each part of the code expanded into its own separate descriptive dimension attribute. 
* Chapter 3 (pg. 82), Chapter 11 (pg. 301), Chapter 16 (pg. 383)

#### Null Attributes in Dimensions 
Null-valued dimension attributes result when a given dimension row has not been fully populated, or when there are attributes that are not applicable to all the dimension's rows. Nulls in dimension attributes should be avoided because different databases handle grouping and constraining on nulls inconsistently. 
* Chapter 3 (pg. 92)

#### Calendar Date Dimensions 
_Calendar Date Dimensions_ are attached to virtually every fact table to allow navigation of the fact table through familiar dates, months, fiscal periods, and special days on the calendar. The calendar date dimension typically has many attributes describing characteristics such as week number, month name, fiscal period, and national holiday indicator. If business users constrain or group on time-of-day attributes, such as day part grouping or shift number, then you would add a separate time-of-day dimension foreign key to the fact table. 

#### Role-Playing Dimensions 
A single physical dimension can be reference multiple times in a fact table, with each reference linking to a logically distinct role for the dimension. For instance, a fact table can have several dates, each of which is represented by a foreign key to the date dimension. These separate dimension views are called _roles_. 
* Chapter 6 (pg. 170), Chapter 12 (pg. 312), Chapter 14 (pg. 345), Chapter 16 (pg. 380)

#### Junk Dimensions 
Transactional business processes typically produce a number of miscellaneous, low-cardinality flags and indicators. Rather than making separate dimensions for each flag and attribute, you can create a single junk dimension combining them together. 
* Chapter 6 (pg. 179), Chapter 12 (pg. 318), Chapter 16 (pg. 392), Chapter 19 (pg. 470)

#### Snowflaked Dimensions 
When a hierarchical relationship in a dimension table is normalized, low-cardinality attributes appear as secondary tables connected to the base dimension table by an attribute key. When this process is repeated with all the dimension table's hierarchies, a characteristic multilevel structure is created that is called a _snowflake_. You should avoid snowflakes because it is difficult for business users to understand and navigate them. 
* Chapter 3 (pg. 104), Chapter 11 (pg. 301), Chapter 20 (pg. 504)

#### Outrigger Dimensions 
A dimension can contain a reference to another dimension table. These secondary dimension references are called _outrigger dimensions_. Outrigger dimensions are permissible, but should be used sparingly. In most cases, the correlations between dimension should be demoted to a fact table, where both dimensions are represented as separate foreign keys. 
* Chapter 3 (pg. 106), Chapter 5 (pg. 160), Chapter 8 (pg. 243), Chapter 12 (pg. 321)

### Integration via Conformed Dimensions 
One of the marquee successes of the dimensional modeling approach has been to define a simple but powerful recipe for integrating data from different business processes.  

#### Conformed Dimensions 
Dimension tables conform when attributes in separate dimension tables have the same column names and domain contents. Information from separate fact tables can be combined in a single report by using conformed dimension attributes that are associated with each fact table. When a conformed attribute is used as the row header, the results from the separate fact tables can be aligned on the same rows in a drill-across report. This is the essence of integration in an enterprise DW/BI system. _Conformed dimensions_, defined once in collaboration with the business's data governance representatives, are reused across fact tables; they deliver both analytical consistency and reduced future development costs because the wheel is not repeatedly re-created. 
* Chapter 4 (pg. 130), Chapter 8 (pg. 256), Chapter 11 (pg. 304), Chapter 16 (pg. 386), Chapter 18 (pg. 431), Chapter 18 (pg. 461)

#### Shrunken Dimensions 
_Shrunken Dimensions_ are conformed dimensions that are a subset of rows and/or columns of a base dimension. These are required when constructing aggregate fact tables. They are also necessary for business processes that naturally capture data at a higher level of granularity, such as a forecast by month and brand. Another case of conformed dimension sub-setting occurs when two dimensions are at the same level of detail, but one represents only a subset of rows. 
* Chapter 4 (pg. 132), Chapter 19 (pg. 472), Chapter 20 (pg. 504)

#### Drilling-Across 
_Drilling-Across_ simply means making separate queries against two or more fact tables where the row headers of each query consist of identical conformed attributes. The answer sets from the two queries are aligned by performing a sort-merge operation on the common dimension attribute row headers. 
* Chapter 4 (pg. 130)

#### Value Chain 
A _value chain_ identifies the natural flow of an organization's primary business processes. Operational source systems typically produce transaction or snapshots at each step of the value chain. 
* Chapter 4 (pg. 111), Chapter 7 (pg. 210), Chapter 16 (pg. 377)

#### Enterprise Data Warehouse Bus Architecture
The _Enterprise Data Warehouse Bus Architecture_ provides an incremental approach to building the enterprise DW/BI system. This architecture decomposes the DW/BI planning process into manageable pieces by focusing on business processes, while delivering integration via standardized conformed dimensions that are reused across processes. The bus architecture is technology and database platform independent; both relational and OLAP dimensional structures can participate. 
* Chapter 1 (pg. 21), Chapter 4 (pg. 123)

#### Enterprise Data Warehouse Bus Matrix 
The _Enterprise Data Warehouse Bus Matrix_ is the essential tool for designing and communicating the enterprise data warehouse bus architecture. The rows of the matrix are business processes and the columns are dimensions. The shaded cells of the matrix indicate whether a dimension is associated with a given business process. Besides the technical design considerations, the bus matrix is used as input to prioritize the DW/BI projects with business management as teams should implement one row of the matrix at a time. 
* Chapter 4 (pg. 125), Chapter 5 (pg. 143), Chapter 6 (pg. 168), Chapter 7 (pg. 202), Chapter 9 (pg. 268), Chapter 10 (pg. 282), Chapter 11 (pg. 297), Chapter 12 (pg. 311), Chapter 13 (pg. 325), Chapter 14 (pg. 339), Chapter 15 (pg. 368), Chapter 16 (pg. 389)

#### Detailed Implementation Bus Matrix
The _Detailed Implementation Bus Matrix_ is a more granular bus matrix where each business process row has been expanded to show specific fact tables or OLAP cubes. 
* Chapter 5 (pg. 143), Chapter 16 (pg. 390)

#### Opportunity/Stakeholder Matrix 
The _Opportunity/Stakeholder Matrix_ helps identify which business groups should be invited to the collaborative design sessions for each process-centric row. 
* Chapter 4 (pg. 127)

### Dealing with Slowly Changing Dimension Attributes 
It is quite common to have attributes in the same dimension table that are handled with different change tracking techniques. Below you will find fundamental approaches for dealing with slowly changing dimension (SCD) attributes. 

#### Type 0: Retain Original  
With _type 0_, the dimension attribute value never changes, so facts are always grouped by this original value. Type 0 is appropriate for any attribute labeled "original", such as a customer's original credit score ofr a durable identifiers. 
* Chapter 5 (pg. 148)

#### Type 1: Overwrite 
With _type 1_, the old attribute value in the dimension row is overwritten with the new value; type 1 attributes always reflects the most recent assignment, and therefore this technique destroys history. Although this approach is easy to implement and does not create additional dimension rows. 
* Chapter 5 (pg. 149), Chapter 16 (pg. 380), Chapter 19 (pg. 465)

#### Type 2: Add New Row 
_Type 2_ changes add a new row in the dimension with the updated attribute values. When a new row is created for a dimension member, a new primary surrogate key is assigned and used as a foreign key in all fact tables from the moment of the update until a subsequent change creates a new dimension key and updated dimension row. 
* Chapter 5 (pg. 150), Chapter 8 (pg. 243), Chapter 9 (pg. 263), Chapter 16 (pg. 380), Chapter 19 (pg. 465), Chapter 20 (pg. 507)

#### Type 3: Add New Attribute 
_Type 3_ changes add a anew attribute in the dimension to preserve the old attribute value; the new value overwrite the main attribute as in a type 1 change. This kind of type 3 change is sometimes called an _alternative reality_. This slowly changing dimension technique is used relatively infrequently. 
* Chapter 5 (pg. 154), Chapter 16 (pg. 380), Chapter 19 (pg. 467)

#### Type 4: Add Mini-Dimension 
This situation is sometimes called a _rapidly changing monster dimension_. The type 4 mini dimension requires its own unique primary key; the primary keys of both the base dimension and mini-dimension are captured in the associated fact tables. 
* Chapter 5 (pg. 156), Chapter 10 (pg. 289), Chapter 16 (pg. 381), Chapter 19 (pg. 467)

#### Type 5: Add Mini-Dimension and Type 1 Outrigger 
The _Type 5_ technique is used to accurately preserve historical attribute values, plus report historical facts according to current attribute values. Type 5 builds on the type 4 mini-dimension by also embedding a current type 1 reference to the mini-dimension in the base dimension. The ETL team must overwrite this type 1 mini-dimension reference whenever the current mini-dimension assignment changes. 
* Chapter 5 (pg. 160), Chapter 19 (pg. 468)

#### Type 6: Add Type 1 Attributes to Type 2 Dimension 
Like type 5, _Type 6_ also delivers both historical and current dimension attribute values. Type 6 builds on the the type 2 technique by also embedding current type 1 versions of the same attributes in the dimension row so that fact rows can be filtered or grounded by either the type 2 attribute value in effec when the measurement occurred or the attribute's current value. 
* Chapter 5 (pg. 160), Chapter 19 (pg. 468)

#### Type 7: Dual Type 1 and Type 2 Dimensions
_Type 7_ is the final hybrid technique used to support both as-was and as-is reporting. A fact table can be accessed through a dimension modeled both as a type 1 dimension showing only the most current attribute values, or as a type 2 dimension showing correct contemporary historical profiles. The same dimension table enables both perspectives. Both the durable key and primary surrogate key of the dimension are placed in the fact table. 

For the Type 1 perspective, the current flag in the dimension is constrained to be current and the fact table is joined via the durable key. For the Type 2 perspective, the current flag is not constrained, and the fact table is joined via the surrogate primary key. These two perspectives would be deployed as separate views to the BI applications. 
* Chapter 5 (pg. 162), Chapter 19 (pg. 468)

### Dealing with Dimension Hierarchies 
