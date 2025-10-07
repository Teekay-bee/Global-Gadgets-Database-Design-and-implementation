# Global-Gadgets-Database-Design-and-implementation
An E-commerce Database Management System 

---
# Introduction 

In today’s digital economy, **e-commerce** has emerged as one of the fastest-growing sectors, enabling businesses to sell goods and services to customers through online platforms. As the scale of operations increases, managing large volumes of data related to **customers, suppliers, products, orders, shipments, and payments** becomes a significant challenge. 

Traditional manual systems and unstructured data storage methods often lead to inefficiencies such as **data redundancy, inconsistency,** and **difficulty in retrieving information**.

To address these challenges, **Relational Database Management Systems (RDBMS)** are employed to store, organize, and manage business-critical information. This project focuses on the **design and implementation of a database system** for e-commerce operations. The system ensures **accurate record-keeping**, **enforces business rules**, and **facilitates smooth data flow** between different entities such as customers, orders, inventory, and suppliers.

---
## problem Statement 

E-commerce businesses often struggle with managing **customer records**, **tracking orders**, **updating delivery status**, **handling product inventory**, and **processing payments** efficiently. Without a well-structured database:

- Customer details and addresses may be **duplicated or inconsistent**.  
- Orders and shipments may not be **accurately tracked**.  
- Inventory levels may not reflect **real-time stock availability**.  
- Refunds and reviews may not be **properly linked** to their respective transactions.  

These issues can result in **customer dissatisfaction**, **financial losses**, and **inefficiency in decision-making**. Therefore, there is a need for a **centralized database system** that integrates all business processes and ensures **data accuracy**.

---
## Aims and Objectives 

The aim of this project is to **design and implement a relational database management system (RDBMS)** for an e-commerce platform that ensures **efficient data management** and **operational automation**.

## Specific Objectives

The specific objectives of this project are to:

1. **Design** a relational database schema for managing customer, supplier, product, order, and inventory records.  
2. **Enforce data integrity** through the use of primary keys, foreign keys, and constraints.  
3. **Implement triggers** to automate business rules such as updating order status upon delivery.  
4. **Provide sample data insertion** to test the efficiency and functionality of the system.  
5. **Ensure scalability and reliability** of the database for future growth and expansion.
---
## Project Scope

This project is limited to the **design and implementation of a relational database** for an e-commerce system.  

The scope includes:

- Creation of tables for **customers**, **customer addresses**, **suppliers**, **product categories**, **products**, **inventory**, **orders**, **order details**, **payments**, **shipments**, **reviews**, **refunds**, and **employees**.  
- Application of **triggers**, **constraints**, and **stored procedures** to enforce business rules.  
- **Sample data insertion** for testing and validating the database design.

However, this project **does not include** the development of a full **e-commerce website** or **front-end application**. The focus remains on the **back-end database design and implementation**.

---
## project Significance 

This project is significant in several ways:

- It demonstrates the role of **database systems** in enhancing **efficiency** and **accuracy** in e-commerce operations.  
- It provides a **foundation for developers** who may wish to expand the system into a full-scale e-commerce application.  
- It serves as a **reference for researchers** interested in database design and implementation.  
- **Businesses** can adapt the system to improve their **order processing**, **customer management**, and **inventory control**.
---
## Database Design Overview 

The system was designed to capture all essential aspects of an e-commerce platform.  
The main entities include:

- **Customers:** Stores user details such as name, email, and contact information.  
- **Customer Address:** Maintains multiple shipping addresses for customers.  
- **Products:** Contains information about items available for sale.  
- **Product Categories:** Organizes products into logical groups.  
- **Suppliers:** Records supplier details and links them to employees for accountability.  
- **Inventory:** Tracks product stock levels and availability.  
- **Orders and OrderDetails:** Capture purchase transactions and their corresponding line items.  
- **Payment Methods:** Stores customers’ preferred payment options.  
- **Shipments:** Handles delivery processes and logistics tracking.  
- **Customer Authentication:** Manages verification codes and login security.  
- **Reviews:** Stores customer feedback and product ratings.  
- **Employees:** Contains details of internal staff managing operations.  
- **Refunds:** Records refund requests, processing, and approvals.
---
### ER Diagram 

--- 
### Normalization process

To ensure efficient database design, **normalization rules** were applied up to the **Third Normal Form (3NF)**.

- First Normal Form (1NF)
All attributes are **atomic** (no repeating groups).  
**Example:** Instead of storing a full name in one column, it was separated into `first_name` and `last_name` in the **Customers** table.

- Second Normal Form (2NF)
Eliminated **partial dependencies** by separating attributes that depend only on part of a composite key.  
**Example:** Customer addresses were moved to a separate **CustomerAddress** table so that multiple addresses can be stored for one customer.

- Third Normal Form (3NF)
Removed **transitive dependencies** (non-key attributes depending on other non-key attributes).  
**Example:** Product categories were separated into the **ProductCategory** table instead of storing category names in the **Products** table.

### Illustration

- **Unnormalized Table:**
Products( product_Id,name, category_name, category_description,Price)

- **Normalized Tables (3NF):**
  - Products( product_Id,name, price and category_id)
  - Products Category (Category_Id, Category _name,Category_description)

This separation avoids **duplication of category details** for each product and ensures **data consistency** across the database.

---
### Constraints Implementation 

Several **constraints** were applied during schema implementation to enforce business rules and maintain data integrity:

- **Primary Keys (PK):** Ensure each record is uniquely identified.  
  *Example:* `customer_id`, `order_id`.

- **Foreign Keys (FK):** Maintain **referential integrity** between related tables.  
  *Example:* `Orders → Customers`, `OrderDetails → Orders`.

- **NOT NULL:** Ensure essential fields are always filled.  
  *Example:* Customer names, order date.

- **UNIQUE:** Prevent duplicate entries.  
  *Example:* `email` in **Customers**, `category_name` in **ProductCategory**.

- **DEFAULT:** Automatically populate fields with predefined values.  
  *Example:* `created_at = GETDATE()`, `status = 'Pending'`.

- **CHECK:** Enforce valid and logical values.  
  *Example:* `product_price > 0`, `rating BETWEEN 1 AND 5`, `stock ≥ 0`.
---
### Schema Implementation 

--- 
## Data Concurrency in database Systems

**Concurrency control** is vital in environments where multiple users access and update shared data simultaneously.  
Without proper controls, issues such as **lost updates**, **dirty reads**, **non-repeatable reads**, and **phantom reads** may arise  
*(Silberschatz, Korth, & Sudarshan, 2020)*.

In **Microsoft SQL Server**, concurrency is managed to ensure data consistency and integrity.  
For example, if two users simultaneously attempt to update the stock levels of the same product, SQL Server ensures that **one transaction is completed before the other begins**, thereby avoiding conflicts.

SQL Server addresses concurrency through:

- **Transactions:**  
  Grouping multiple operations ensures that related actions — such as order placement and inventory reduction — occur together, or not at all.  

- **Locks and Isolation Levels:**  
  Prevent conflicting updates by controlling access to data during transactions, ensuring that no two orders reduce the same product’s stock simultaneously.
  
 **Project Example**
When a **customer places an order**, the system performs a series of operations within a single **transaction** to maintain data consistency:

1. **Insert** a new record into the **Orders** table.  
2. **Insert** corresponding **OrderDetails** records for each product purchased.  
3. **Decrement** the product stock levels in the **Inventory** table.

If any of these steps fail, the entire **transaction is rolled back**, preventing partial or inconsistent data updates.  
This ensures that no order exists without its details and that stock quantities remain accurate.

---
### Data integrity in this Database System 
**Data integrity** safeguards the **accuracy** and **consistency** of information throughout its lifecycle.  
In the implemented e-commerce database system, several types of integrity were enforced:

 1. **Entity Integrity**  
Each table was assigned a **Primary Key (PK)** to ensure records are **unique and identifiable**.  
*Example:* `customer_id` uniquely identifies a customer in the **Customers** table.

 2. **Referential Integrity**  
Maintained through **Foreign Keys (FK)** to ensure relationships between tables remain valid.  
*Example:* Each `order_id` in **OrderDetails** must exist in the **Orders** table.

 3. **Domain Integrity**  
Guaranteed through **CHECK constraints**, **data types**, and **default values**.  
*Example:* Product prices must be **positive**, and stock levels **cannot be negative**.

 4. **Business Rule Integrity**  
Implemented using **triggers** and **stored procedures** to enforce custom business logic.  
Examples include:  
- A **trigger** ensures that when an order is marked as *Cancelled*, the stock is automatically **replenished** in the inventory.  
- Another **trigger** ensures that when an order is updated to *Delivered*, its status is **consistently reflected** across related tables.

Such mechanisms prevent **invalid or inconsistent data** from entering the database, thereby preserving overall **data reliability and integrity**  
*(Connolly & Begg, 2015)*.

---
### Transactions Management 

**Transactions** were implemented in **T-SQL** to maintain **atomic operations** within the e-commerce database system.  
A transaction ensures that a group of related operations either **all succeed** or **none are applied**, preserving **data consistency**.

For example, when a **customer places an order**, the system performs the following steps as part of a single transaction:

1. **Insert** a new record into the **Orders** table.  
2. **Insert** related records into the **OrderDetails** table.  
3. **Decrement** the corresponding product quantities from the **Inventory** table.  
4. **Record** payment details in the **Payments** table.

If any of these operations **fail**, the entire **transaction is rolled back**, preventing **half-finished or inconsistent operations**.

This approach ensures that all dependent actions — such as order creation, stock update, and payment recording — occur **safely and reliably**, maintaining the **integrity of business processes**.

---
### Concurrency Control in the Project 

The system uses **pessimistic concurrency control** for sensitive operations such as stock updates.  
For example, when two customers attempt to order the **last unit of a product**, **SQL Server locks** the inventory record so that only one transaction can complete, thereby **preventing overselling**.

**Isolation levels** can be adjusted depending on the operation requirements:

- **For order placement:** `SERIALIZABLE` ensures that no **phantom sales** occur.  
- **For reporting queries:** `READ COMMITTED` prevents **dirty reads** while still allowing concurrency.-

## Functional Implementation 

Functional implementation in this project refers to the process of ensuring that the **designed database performs the intended operations effectively**.  
This includes:

- **Creation of tables** with appropriate constraints to maintain **data integrity**.  
- Use of **stored procedures** to handle repetitive tasks and **enforce business rules**.  
- Implementation of **triggers** to automate updates — such as adjusting inventory levels or changing order status.  
- Development of **views** to simplify data retrieval for end users.

Together, these functions ensure that the database is not only **structurally sound** but also **practically usable** in real-world e-commerce operations  
*(Elmasri & Navathe, 2016)*.

--- 
### Constraints 


---
### Triggers 

--- 
## Data Security Maintenance and Recovery 

--- 
### Data Security 

---
### Data Backup 

--- 
### Data Recovery 

--- 
#### Importance to the Project

---
## Conclusion 

---
## Advice 

---
## Summary 

--- 
## About me 

