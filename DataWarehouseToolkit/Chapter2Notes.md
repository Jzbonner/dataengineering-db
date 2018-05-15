# Data Warehouse Toolkit 3rd Edition 
The Data Warehouse Toolkit Third Edition: The Definitive Guide to Dimensional Modeling. Covering a multitude of topics such as Data Warehousing, Business Intelligence, Kimball Dimension Modeling, ETL System Design and Development, and Big Data Analytics. 

## Kimball Dimensional Modelling Techniques Overview ~ Chapter 2
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