# Point of Sale System

## Table of Contents
- [Authors](#authors)
- [System Description](#system-Description)
- [Software Architecture Overview](#software-Architecture-Overview)
- [Development Plan and Timeline](#development-Plan-and-Timeline)
- [Class Descriptions](#class-Descriptions)

## Authors
Isabelle Viraldo
Nicholas Moffat
Seven Blackwell

## System Description
This is the document describing the development of a catalog system for any clothing store to buy, and to implement. This system excels at documenting the inventory and managing transactions. This system will be utilized by managers and employees to ensure inventory information is efficiently and accurately maintained. 

The system is designed to streamline sales processes, enabling employees to complete transactions, tracking purchases and returns, while automatically updating inventory. Employees can checkout customers with automatic tax calculators, and after completing purchase, the system automatically updates the inventory catalog. Inventory features will include search capabilities by item ID or name, and allow employees to add new items with specific attributes such as price and size. These inventory changes made by the employees must be confirmed by the Admin of the system before implementation into the database. All data, including transaction history and inventory, will be stored in a cloud database. This system is an integral part of managing a clothing store.

## Software Architecure Overview
Architectural diagram of all major components

![image](https://github.com/user-attachments/assets/514dbfa1-2907-4413-8a14-d0414af540cc)


Class #1

attribute#1

attribute#2

operation#1

operation#2

descriptions should be detailed and specify datatypes, function interfaces, parameters, etc..

## Development Plan and Timeline
To develop this software system, the team needs a team of 5 people, consisting of 2 Software Developers, a UI developer, a Project Manager, and a Quality Assurance Engineer. Working with a budget of $200,000, the product can be developed and launched in the span of 10 weeks by following the development plan below. 

**Week 1**:
  Meet with users at store to gather further requirements
  Develop documentation
  Define use cases for various functions 
  
**Week 2**:
  Design system architecture
  Develop wireframes and prototypes for the UI
  
**Weeks 3 - 8**:
  Develop UI
  Develop software 
  Integrate with hardware
  Integrate with data storage
  
**Week 9**:
  Test individual components
  Ensure all parts work together
  Have employees test the system
  
**Week 10**:
  Start using in a real store
  Complete documentation

## Class Descriptions
**User:**

The user is who will be interacting with the system. This could be a basic employee or an administrator. When the user logs into the system, if they are an admin, the admin pin will be entered after logging in with their username and password, otherwise they can continue as an employee. Logging in as an admin grants them admin privileges. This class has methods for retrieving the users login information in order to keep track of who logs in.

**Item:**

An item is anything in the store that can be purchased. Each item has an identifying ID that make it easy to search for in the inventory. When the barcode gets scanned, it searches in the item hashmap for the item. The item can also be searched for manually using the other identifiers, the name, color, size, and amount. This class has methods for retrieving the identifiers.

**Inventory:**

Each item object is stored here in the inventory hashmap. Using a hash is easier since it allows for O(1) runtime rather than O(n) runtime after accessing an array. Each item ID is the “key” and the item object is what is returned. The methods in this class allow you to add an item object, remove an item, retrieve an item or print a full list of the inventory.

**Transaction:**

Each transaction creates a new transaction object that gets added to the transaction history array, and since we are coding in python, this array is dynamically sized to fit each new transaction. Each transaction object has a purchase total, transaction ID and transaction date. This class has methods for retrieving each attribute.

**Transaction History:**

Each transaction object is stored in one instance of the transaction history class. When a transaction is either created or refunded, the list is updated. When you update the list, you declare whether its an addition or a removal as a second parameter to the transaction object. This then tells the system to remove or add it to the list.

