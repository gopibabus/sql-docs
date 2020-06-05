# 🔥Designing Databases

<img alt="db design" height="300" width="400" src="/assets/images/db_design.png" />

### ✳Data Modelling

?> **Data modeling** is a process used to define and analyze data requirements needed to support the business processes within the scope of corresponding information systems in organizations.

?> Data Modeling refers to the practice of documenting software and business system design. The **modeling** of these various systems and processes often involves the use of diagrams, symbols, and textual references to represent the way the data flows through a software application or the Data Architecturewithin an enterprise. Data Modeling also includes practices such as business process modeling which deals with larger conceptual business process and decision making flows of entire organizations.

> Data Modelling involves following steps:

1. Understand the requirements
2. Build a conceptual model.
3. Build a logical model.
4. Build a physical model.

### ✳Conceptual Models

?> Represents the entities and their relationships.

?> We can visually represent entities and their relationships

1. Entity Relationship(ER)
2. UML

I am more comfortable representing entities in ER diagrams and I use [draw.io](https://draw.io) to draw ER diagrams.

<img alt="db design" width="500" src="/assets/images/cm.png" />

### ✳Logical Models

<img alt="db design" width="500" src="/assets/images/lm.png" />

### ✳Physical Models

<img alt="db design" width="500" src="/assets/images/pm.png" />

### ✳Foreign Key Constraints

<img alt="db design" width="500" src="/assets/images/f-key.png" />

## ⚡Normalization

?> **Normalization** is a systematic approach of decomposing tables to eliminate data redundancy(repetition) and undesirable characteristics like Insertion, Update and Deletion Anomalies. It is a multi-step process that puts data into tabular form, removing duplicated data from the relation tables.

There are 7 normal forms. But 90% of time, we can resolve all duplicated data using first 3 normal forms.

> [Best article on Database Normalization](https://www.guru99.com/database-normalization.html)

### ✳1NF-First Normal Form

?> Each cell should have a single value and we cannot have repeated columns.

<img alt="1nf" width="500" src="/assets/images/1nf.png" />

### ✳2NF-Second Normal Form

!> Before implementing 2NF , tables should be in 1NF.

?> Every table should describe one entity, and every column in that table should describe that entity.

<img alt="2nf" width="500" src="/assets/images/2nf.png" />

### ✳3NF-Third Normal Form

!> Before implementing 3NF , tables should be in 2NF.

?> A column in a table should not be derived from other columns.

?> **Pragmatic Advice:** Don't try to forcly implement normal forms to tables in your applications, Instead try to identify the duplicated data and try to change design of your database to avoid duplicated data/records.

!> Don't model the universe(don't model your database for future requirements). Keep your database design simple focussing on present business requirements. If business need any extra tables needed to be added to database, we can modify them in future. Make sure to design databases by keeping simpliciy in mind.

### ✳Important Resources

<a class="btn btn-lg mt-3 form-control bg-danger text-light" href="/db_dump/school-model.sql">Download school-model</a>
<a class="btn btn-lg mt-3 form-control bg-danger text-light" href="/db_dump/school-database.sql">Download school-database</a>

> **MySQL Workbench** is such a wonderful tool, which helps us in providing following options:

1. [Creating a Data Model](https://dev.mysql.com/doc/workbench/en/wb-getting-started-tutorial-creating-a-model.html)
2. [Forward Engineering](https://dev.mysql.com/doc/workbench/en/wb-forward-engineering-live-server.html)
3. [Synchronizing a Model](https://dev.mysql.com/doc/workbench/en/wb-database-synchronization.html)
4. [Reverse Engineering a Database](https://dev.mysql.com/doc/workbench/en/wb-reverse-engineer-create-script.html)
