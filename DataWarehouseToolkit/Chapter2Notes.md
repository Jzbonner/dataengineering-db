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
~ Refer to Notes on Github. 