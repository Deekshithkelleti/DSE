# Day 1: DBMS Basics

## Introduction to DBMS
* **Data:** Facts that can be recorded (text, numbers, speech, images, audio, video).
* **Database (DB):** Collection of related data that represents real-world entities.
* **Information:** Meaningful / processed data.
* **DBMS (Database Management System):** Set of tools to manage the data.
  > **DB + DBMS = Database System**

### Need / Purpose
* **Storage:** Takes less amount of data space.
* **Retrieval:** Easy to retrieve data from the DBMS.

---

## Characteristics of DBMS
* Real-world entity
* Relation-based tables
* Isolation of data and applications
* Less redundancy
* Consistency
* Security
* Query Language
* **ACID Properties:**
  * Atomicity
  * Consistency
  * Isolation
  * Durability

---

## DBMS Users
* **Administrators:** They can create access profiles for users and apply limitations to maintain isolation and force security.
* **Designers:** They identify and design the whole set of entities, relations, constraints, and views.
* **End Users:** They can range from simple users who pay attention to logs or market rates to sophisticated users such as business analysts.

---

## DBMS Architecture
Denotes how users access the database. There are 3 types: 1-Tier, 2-Tier, and 3-Tier.

* **1-Tier:** User can directly access the database.
* **2-Tier:** `User -> Application -> Database`. The application is in between the user and the database.
* **3-Tier:**
  * **Presentation Layer:** User can interact with this layer.
  * **Application Layer:** Acts as a middleman (`User Request <-> Application <-> DB`).
  * **Database Layer:** Actual DB, containing tables, records, constraints, etc.

---

## Data Models
* It can define how the data is stored, organized, and connected to databases.
* Acts as a blueprint of the database.

### Types of Data Models
1. **Entity-Relationship (ER) Model:**
   * Used to design the database.
   * It consists of Entity, Attribute, and Relationship.
   * **Mapping Cardinality:** One-to-one, one-to-many, many-to-one, many-to-many.
2. **Relational Model:**
   * Values saved are atomic values.
   * Each row &rarr; unique values.
   * Each column &rarr; values from the same domain.

---

## Database Schema & Instance

### Database Schema
It defines how the data is organized and how the relations among them are associated.
* *Architecture flow:* `Views (View 1, View 2) -> Logical Schema (e.g., Student [ID, Name]) -> Physical Schema (Storage)`.

### Database Instance
* DB current data (Snapshot).
* Contains actual data.
* Frequently makes changes.

---

## Classification by Data Model
1. **RDBMS:** Data stored in rows and columns (e.g., MySQL).
2. **Object-Oriented DBMS:** It stores data as objects, similar to how programming languages do.
3. **Hierarchical DBMS:** Stores data in a tree-like structure. Each parent record has one or more child records, resembling a family tree.
4. **Network DBMS:** It allows multiple parent-child relationships, forming a web-like data model.
5. **NoSQL DBMS:** These are designed to handle large volumes of unstructured or semi-structured data.
   * *Types:* Document-based (MongoDB), Key-value stores, Column-family stores, Graph database.
