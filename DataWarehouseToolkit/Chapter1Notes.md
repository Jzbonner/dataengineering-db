# Data Warehouse Toolkit 3rd Edition 
The Data Warehouse Toolkit Third Edition: The Definitive Guide to Dimensional Modeling. Covering a multitude of topics such as Data Warehousing, Business Intelligence, Kimball Dimension Modeling, ETL System Design and Development, and Big Data Analytics. 

## Chapter 1 ~ Data Warehousing, Business Intelligence, and Dimensional Modeling Primer 
The Data Warehouse/Business Intelligence System must consider the needs of the business. This Chapter will cover the following in detail: 
* Business driven goals of data warehousing and business intelligence
* Publishing metaphor for DW/BI Systems 
* Dimensional Modeling Core Concepts and vocabulary, including fact and dimension tables 
* Kimball DW/BI architecture components and tenets 
* Comparison with alternative DW/BI architectures and the role of dimensional modeling within each
* Misunderstandings about Dimensional Modeling 

### Different World of Data Capture and Data Analysis 
Data in a business sense is primarily used for two purposes: operational record keeping and analytical decision making. Mainly the operational systems are were you but the data in and the DW/BI systems are where you get the data out. 

### Goals of Data Warehousing and Business Intelligence 
* The DW/BI System must make information easily accessible: Contents must be understandable and the data must be intuitive and obvious to the business user not just the developer. The business intelligence tools and applications that access the data must be simple and easy to use. They must also return queries to the user with minimal wait times
* The DW/BI System must present information consistently and with credibility. Data should be adequately cleansed, quality assured, and released in a fluid manner
* The DW/BI system must adapt to change. It must be designed to handle the inevitable changes systematically and without sacrificing the integrity of existing data 
* The DW/BI System must present information in a timely way
* The DW/BI System must be a secure bastion that protects the information assets. The DW/BI System must effectively control access to the organization's confidential information 
* The DW/BI System must serve as authoritative and trustworthy foundation for improved decision making. The developer is responsible for creating a decision support system 
* The business community must accept the DW/BI System to deem it successful. Business users will embrace the DW/BI System if it is the "simple and fast" source for actionable information 

### Dimensional Modeling Introduction 
Dimensional Modeling is widely accepted as the preferred technique for presenting analytic data because it addresses two simultaneous requirements: 
* Deliver data that's understandable to the business users
* Deliver fast query performance 

Entity relationship diagrams are drawings that communicate the relationship between tables. The use of normalized modeling in the DW/BI presentation area defeats te intuitive and high-performance retrieval of data. Dimensional modeling addresses the problem of overly complex schemas in the presentation area. 

#### Star Schemas vs. OLAP Cubes 
Dimensional models implemented in relational database management systems are referred to as _star schemas_. Dimensional models implemented in multidimensional database environments are referred to as _online analytical processing cubes_ or _(OLAP)_. When data is loaded into an OLAP cube, it is stored and indexed using formats and techniques that are designed for dimensional data. Performance aggregations or precalculated summary tables are often created and managed by OLAP cube engines. Typically OLAP cubes provide users with superior query performance because of the precalculations, indexing strategies and other optimizations. 

![Diagram 1](https://raw.githubusercontent.com/Jzbonner/DataEngineering/gh-pages/DataWarehouseToolkit/img-media/DWT%20Ch.%201%20Diagram%201.png)

* A Star Schema hosted in a relational database is a good physical foundation for building an OLAP cube, and is generally regarded as a more stable basis for backup and recover
* OLAP cubes have traditionally been noted for extreme performance advantages over RDBMS 
* OLAP cube data structures are more variable across different vendors than relational DBMS 
* OLAP cubes typically offer more sophisticated security options than RDBMS, such as limiting access to detailed data but providing more open access to summary data
* OLAP cubes offer significantly richer analysis capabilities than RDBMS which are saddled by the constraints of SQL
* OLAP cubes gracefully support slowly changing dimension type 2 changes, but cubes often need to be reprocessed partially or totally whenever data is rewritten 
* OLAP cubes typically support complex ragged hierarchies of indeterminate depth, such as organization charts or bills of material, using native query syntax that is superior to the approaches required for RDBMS 
* OLAP cubes may impose detailed constraints on the structure of dimension keys that implement  drill-down hierarchies compared to relational databases 

#### Fact Tables for Measurements 
The _fact table_ in a dimensional model stores the performance measurements resulting from an organization's business process events. You should strive to store the low-level measurement data resulting from a business process in a single dimensional model. Measurement data is arguably the largest set of data. 
* The term fact represents a business measure
* Each row in a fact table corresponds to a measurement event  
     * The data on each row is at a specific level of detail, referred to as the grain, such as one row per product sold on a sales transaction 

Additivity is crucial because BI applications rarely retrieve a single fact table row. Rather they bring back hundreds, thousands, or even millions of fact rows at a time, and the most useful thing to do with so many rows is to add them up. Facts are often described as continuously valued to help sort out what is a fact versus a dimension attribute. 

The designer should make every effort to put textual data in dimensions where they can be correlated more effectively with the other textual dimension attributes and consumer much less space. Fact tables tend to be deep in terms of the number of rows, but narrow in terms of the number of columns. All fact tables have two or more foreign keys that connect to the dimension tables' primary keys. When all the keys in the fact table correctly match their respective primary keys in the corresponding dimension tables, that tables satisfy _referential integrity_. 
* The fact table generally has its own primary key composed of a subset of the foreign keys. This key is called the _composite key_ 
* Every table that has a _composite key_ is a fact table 

#### Dimension Tables for Descriptive Context 
Dimension tables are integral companions to a fact table. The dimension tables contain the textual context associated with a business process measurement event. Dimension tables tend to have fewer rows than fact tables, but can be wide with many large text columns. Each dimension is defined by a single primary key which serves as the basis for referential integrity with any given fact table to which it is joined. 

Dimension tables attributes play a vital role in the Dw/BI system. Because they are the source of virtually all constraints and report labels, dimension attributes are critical to making the DW/BI usable and understandable.
* You should make standard decodes for the the operational codes available as dimension attributes to provide consistent labeling on queries, reports and BI applications  
* Rather than forcing users to interrogate or filter on substrings within the operational codes, pull out the embedded meanings and present them to users as separate dimension attributes that can easily be filtered, grouped or reported
* Dimension tables are highly denormalized with flattened many-to-one relationships within a single dimension table 

#### Facts and Dimensions Joined in a Star Schema 
![Diagram 2](https://raw.githubusercontent.com/Jzbonner/DataEngineering/gh-pages/DataWarehouseToolkit/img-media/DWT%20Ch.%201%20Diagram%202.png)

The first thing to notice about the dimensional schema is its simplicity and symmetry. The simplicity of the dimensional model has performance benefits. Database optimizers process these simple schemas with fewer joins more efficiently. Dimension models are gracefully extensible to accommodate change. The predictable framework of a dimensional model withstands unexpected changes in user behavior. Another way to think about the relationship between a dimension and a fact is to think in terms of a report, dimension attributes supply the report filters and labeling, whereas the fact tables supply the report's numeric values: 

![Diagram 3](https://raw.githubusercontent.com/Jzbonner/DataEngineering/gh-pages/DataWarehouseToolkit/img-media/DWT%20Ch.%201%20Diagram%203.png)

You can think about the SQL required to create the illustrated report from above as: 
```SQL
SELECT 
    store.district_name,
    product.brand,
    sum(sales_facts.sales_dollars) AS "Sales Dollars" 

FROM 
    store, 
    product, 
    date, 
    sales_facts

WHERE 
    date.month_name = "January" AND 
    date.year = 2013 AND 
    store.store_key = sales_facts.store_key AND 
    product.product_key = sales_facts.product_key AND 
    data.date_key = sales_facts.date_key 

GROUP BY 
    store.district_name, 
    product.brand 
```

### Kimball's DW/BI Architecture 
As illustrated in the figure below there are four separate and distinct components to consider in the DW/BI environment : operational source systems, ETL system, data presentation area, and business intelligence applications. 

![Diagram 4](https://raw.githubusercontent.com/Jzbonner/DataEngineering/gh-pages/DataWarehouseToolkit/img-media/DWT%20Ch.1%20Diagram%204.png)

#### Operational Source Systems 
Think of the source systems as outside the data warehouse because presumable you have little or not control over the content and format of the data in these operational systems. The main priorities of the source system are processing performance and availability. Source systems contain little historical data, a good data warehouse can relieve the source systems of much of the responsibility of being responsible for the past. In may cases the source systems are special purpose applications without any commitment to sharing common data (i.e. product, customer, geography, etc.) 

#### Extract, Transformation and Load Systems
The Extract, Transformation and Load Systems of the DW/BI environment consist of a work area, instantiated data structures and a set of processes. The ETL system is everything between operational source systems and the DW/BI presentation area. 
* Extraction is the first step in the process of getting data into the data warehouse environment (_Extracting_ means reading and understanding the source data and copying the data needed into ETL system for further manipulation)
* Then there are numerous potential transformations such as cleansing the data, combining data from multiple sources, and de-duplicating data 
    * In addition these activities can be architected to create diagnostic metadata, eventually leading to business process re-engineering to improve data quality in the source systems over time 
* The final step of the ETL process is the physical structuring and loading of data into the presentation area's target dimensional model

Many of these defined subsystems focus on dimension table processing such as surrogate key assignments, code lookups to provide appropriate descriptions, splitting or combining columns to present the appropriate data values or joining underlying third normal form table structures into flattened denormalized dimensions. 

#### Presentation Area to Support Business Intelligence 
The _DW/BI presentation area_ is where data is organized, stored and made available for direct querying by users, report writers, and other analytical BI applications. Ideas to consider for the presentation area: 

> Insist that the data be presented, stored, and accessed in dimensional schemas; for dimensional modeling is the most viable technique for delivering data to the DW/BI users. The second stake is that it must contain detailed, atomic data. Atomic data is required to withstand assaults from unpredictable ad hoc user queries. The most finely grained data should be available in the presentation area so that users can ask the most precise questions possible. The presentation data are should be built around business process measurement events. All the dimensional structures must be built using common, conformed dimensions (i.e. they must conform to the adherence of the _Enterprise Data Warehouse Bus Architecture_).

(_Data Warehouse Toolkit_, Ch. 1)

Using the bus architecture is the key to building distributed DW/BI systems. When the bus architecture is used as a framework, you can develop the enterprise data warehouse in agile, decentralized, realistically scoped, iterative manner. 

#### Business Intelligence Applications 
The term Business Intelligence loosely refers to the range of capabilities provided to business users to leverage the presentation area for analytical decision making. A BI application can be as simple as an ad hoc query tool or as complex as a sophisticated data mining or modeling application. 

### Alternative DW/BI Architectures 
> We strongly believe that rather than encouraging more consternation over our philosophical differences, the industry would be far better off devoting energy to ensure that our DW/BI deliverables are broadly accepted by the business to make better, more informed decisions. The architecture should merely be a means to this objective. 

(_Data Warehouse Toolkit_, Ch. 1)

#### Independent Data Mart Architecture 
![Diagram 5](https://raw.githubusercontent.com/Jzbonner/DataEngineering/gh-pages/DataWarehouseToolkit/img-media/DWT%20Ch.%201%20Diagram%205.png)

~ Addendum: Include a brief description of similarities and differences fom pages 26 - 30.

#### Hub-and-Spoke CCorporate Information Factory Inmon Architecture 
![Diagram 6](https://raw.githubusercontent.com/Jzbonner/DataEngineering/gh-pages/DataWarehouseToolkit/img-media/DWT%20Ch.%201%20Diagram%206.png)

~ Addendum: Include a brief description of similarities and differences fom pages 26 - 30.

#### Hybrid Hub-and-Spoke and Kimball Architecture 
![Diagram 7](https://raw.githubusercontent.com/Jzbonner/DataEngineering/gh-pages/DataWarehouseToolkit/img-media/DWT%20Ch.%201%20Diagram%207.png)

~ Addendum: Include a brief description of similarities and differences fom pages 26 - 30.

### Dimensional Modeling Myths 
* Myth 1: Dimensional Models are Only for Summary data 
* Myth 2 : Dimensional Models are Departmental, Not Enterprise 
* Myth 3: Dimensional Models are Not Scalable
* Myth 4: Dimensional Models are Only for Predictable Usage 
* Myth 5: Dimensional Models Can't Be Integrated 