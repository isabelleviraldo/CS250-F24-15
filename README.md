# Point of Sale System

## Table of Contents
- [Authors](#authors)
- [System Description](#system-description)
- [Software Architecture Overview](#software-architecure-overview)
  - [UML Class Diagram](#uml-class-diagram)
  - [Checkout Class](#checkout-class)
  - [Transaction History Class](#transaction-history-class)
  - [Transaction Object](#transaction-object)
- [Development Plan and Timeline](#development-plan-and-timeline)
- [Class Descriptions](#class-descriptions)

## Authors
Isabelle Viraldo

Nicholas Moffat

Seven Blackwell

## System Description
This is the document describing the development of a catalog system for any clothing store to buy, and to implement. This system excels at documenting the inventory and managing transactions. This system will be utilized by managers and employees to ensure inventory information is efficiently and accurately maintained. 

The system is designed to streamline sales processes, enabling employees to complete transactions, tracking purchases and returns, while automatically updating inventory. Employees can checkout customers with automatic tax calculators, and after completing purchase, the system automatically updates the inventory catalog. Inventory features will include search capabilities by item ID or name, and allow employees to add new items with specific attributes such as price and size. These inventory changes made by the employees must be confirmed by the Admin of the system before implementation into the database. All data, including transaction history and inventory, will be stored in a cloud database. This system is an integral part of managing a clothing store.

## Software Architecure Overview

### UML Class Diagram
![image](https://github.com/user-attachments/assets/7b3a38b7-7453-4630-aa3d-4d10db45977c)

### Checkout Class
The checkout option allows employees to checkout and give returns to customers. Utilizing transaction objects, employees are able to create custom objects and then update the transaction history through an itreative looping process at the time of checkout. The customer is delivered the cost including the tax, and once payment is confirmed, the system will update the history and the inventory list.

Attributes:
- n/a

Operations:
- checkout_customer()
  - Creates a transaction object and begins an interactive loop where the employee is prompted repeatedly to either enter a new item to check out, or complete the transaction. Each time the new item is scanned, the object is sent to the transaction object. Once the loop is ended by completing the transaction, and the payment is confirmed, the transaction object is sent to the transaction history, and a receipt is printed for the customer.
- return_object(transaction_ID)
  - Creates a new transaction and copies all the information over from the ID supplied by the customer. It then sets the total as negative the original total of the transaction. After confirming the return of money to the customer, this transaction object is then sent to the transaction history.

### Transaction History Class
Each transaction object is stored in one instance of the transaction history class. When a transaction is either created or refunded, the list is updated. When you update the list, you declare whether its an addition or a removal as a second parameter to the transaction object. This then tells the system to remove or add it to the list.

Attributes:
- transaction_history: array[transaction objects]
  - Stores all of the store’s past transactions as an array of transaction objects

Operations:
- get_transaction(transaction_ID): transaction object
  - Returns the transaction object with the ID that is supplied to the function
- update_list(transaction object)
  - Appends the transaction history with the transaction object given
- get_transaction_history(): array[transaction objects]
  - This function is only accessible by admin users, and returns the transaction history to the UI to display

### Transaction Object
Each transaction creates a new transaction object that gets added to the transaction history array, and since we are coding in python, this array is dynamically sized to fit each new transaction. Each transaction object has a purchase total, transaction ID and transaction date. This class has methods for retrieving each attribute.

Attributes:
- transaction_ID: string
  - An automatically created string variable, and is the ID belonging to the transaction
- cart: array[item objects]
  - An array of item objects a customer is checking out, added to with each scan by employee
- purchase_date: string
  - A string automatically assigned the current date
- Total: double
  - A double type (number with decimal points) that represents the total price of the transaction including tax, and is automatically calculated by adding up the prices of each of the item objects in the cart array, then adding sales tax

Operations:
- get_transaction_ID(): string
  - Returns the transaction_ID string value belonging to the transaction object
- get_cart(): array[item objects]
  - Returns the array containing all of the item objects being checked out by the customer
- get_purchase_date(): string
  - Returns the date the transaction occurred as a string value
- get_total(): double
  - Returns the total of the transaction as a double (including tax)
- add_item_to_cart(item object)
  - Gets an item object from the checkout class, and appends it to the cart list the customer is checking out


## Development Plan and Timeline
To develop this software system, the team needs a team of 5 people, consisting of 2 Software Developers (Nicholas Moffat and Seven Blackwell), a UI developer (Isabelle Viraldo), a Project Manager (to be hired), and a Quality Assurance Engineer (to be hired). Working with a budget of $200,000, the product can be developed and launched in the span of 10 weeks by following the development plan below. 

**Week 1**:
- Meet with users at store to gather further requirements
- Develop documentation
- Define use cases for various functions 
  
**Week 2**:
- Design system architecture
- Develop wireframes and prototypes for the UI
  
**Weeks 3 - 8**:
- Develop UI
- Develop software
- Integrate with hardware
- Integrate with data storage
  
**Week 9**:
- Test individual components
- Ensure all parts work together
- Have employees test the system
  
**Week 10**:
- Start using in a real store
- Complete documentation



## Class Descriptions
**User:**

The user is who will be interacting with the system. This could be a basic employee or an administrator. When the user logs into the system, if they are an admin, the admin pin will be entered after logging in with their username and password, otherwise they can continue as an employee. Logging in as an admin grants them admin privileges. This class has methods for retrieving the users login information in order to keep track of who logs in.

**Item:**

An item is anything in the store that can be purchased. Each item has an identifying ID that make it easy to search for in the inventory. When the barcode gets scanned, it searches in the item hashmap for the item. The item can also be searched for manually using the other identifiers, the name, color, size, and amount. This class has methods for retrieving the identifiers.

**Inventory:**

Each item object is stored here in the inventory hashmap. Using a hash is easier since it allows for O(1) runtime rather than O(n) runtime after accessing an array. Each item ID is the “key” and the item object is what is returned. The methods in this class allow you to add an item object, remove an item, retrieve an item or print a full list of the inventory.
