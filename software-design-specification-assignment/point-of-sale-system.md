# Point of Sale System

## Table of Contents
- [Authors](#authors)
- [System Description](#system-description)
- [Software Archetecture Diagram](#software-archetecture-diagram)
  - [Key](#key)
  - [Physical Interface](#physical-interface)
  - [Login](#login)
  - [Software Options](#software-options)
  - [Transactions](#transactions)
  - [Data Storage](#data-storage)
- [Unified Modeling Language Class Diagram](#unified-mdeling-language-class-diagram)
  - [User Class](#user-class)
  - [Item Object](#item-object)
  - [Inventory Class](#inventory-class)
  - [User Interface](#user-interface)
  - [Checkout Class](#checkout-class)
  - [Transaction History Class](#transaction-history-class)
  - [Transaction Object](#transaction-object)
- [Development Plan and Timeline](#development-plan-and-timeline)

## Authors
Isabelle Viraldo

Nicholas Moffat

Seven Blackwell

## System Description
This is the document describing the development of a catalog system for any clothing store to buy, and to implement. This system excels at documenting the inventory and managing transactions. This system will be utilized by managers and employees to ensure inventory information is efficiently and accurately maintained. 

The system is designed to streamline sales processes, enabling employees to complete transactions, tracking purchases and returns, while automatically updating inventory. Employees can checkout customers with automatic tax calculators, and after completing purchase, the system automatically updates the inventory catalog. Inventory features will include search capabilities by item ID or name, and allow employees to add new items with specific attributes such as price and size. These inventory changes made by the employees must be confirmed by the Admin of the system before implementation into the database. All data, including transaction history and inventory, will be stored in a cloud database. This system is an integral part of managing a clothing store.

## Software Archetecture Diagram
![Screenshot 2024-10-23 130053](https://github.com/user-attachments/assets/ecf3dd60-ff5d-4d1a-a823-29e666c7bc0d)

### Key
The physical components are purple rectangles, outside services are red, options for the user are yellow, viewing the inventory is green, and the ui is represented as blue (with darker blue only being accessibel by Admins)

### Physical Interface
The physical interface is how the user inputs everything they need to. Within the interface, There is a touchscreen display, for selecting each of the different options the user may want to do. A barcode scanner for scanning items when checking out a customer. A keyboard and mouse for general interactions. A payment terminal for the customer to complete the transaction, which also confirms with the bank before completing the transaction. All of the physical components talk to the software.

### Login
The only way the user can interact with the system is by entering a valid login.

### Software Options
Every employee has access to the following software options: refund, transaction, update inventory, and search. However, only the Admin can approve the inventory, and view the transaction history. Each of the options go through their respective use cases, refunding, completing a transaction, updating the inventory, searching for an item, approving the inventory changes, and viewing the transaction history.

### Transactions
To complete a transaction, the customer must input payment info in the payment terminal, which, once verified with the bank, and sends a confirmation to the software, is recorded in the transaction history. 

### Data Storage
The refund and transaction options will add their respective actions and all data associated with them to the transaction history stored in the data storage. The data storage is accessed whenever viewing the inventory catalog, and viewing the transaction history.

## Unified Modeling Language Class Diagram
![image](https://github.com/user-attachments/assets/7b3a38b7-7453-4630-aa3d-4d10db45977c)

### User Class
The user is who will be interacting with the system. This could be a basic employee or an administrator. When the user logs into the system, if they are an admin, the admin pin will be entered after logging in with their username and password, otherwise they can continue as an employee. Logging in as an admin grants them admin privileges. This class has methods for retrieving the users login information in order to keep track of who logs in.

Attributes: 
- Employee_pin: int
  - Each employee has a unique PIN that they must use to access the system. To access administrative features, admin users must have this.
- Username: string
  - A string that serves as the user’s system identity. During the login process, the username is used to verify the user’s identity. 
- Password: string
  - String for authentication when combined with the username.
- Admin_privileges: boolean
  - Gives a signal that the user has administrative powers; when true, the user may advance to unique system features. 

Operations:
- get_pin(): int
  - Integer that gives the PIN back; used to confirm access, especially for those who require admin power.
- get_username(): string
  - Obtains username of whoever is using the system; used to verify user upon logging in.
- get_password(): string
  - Retrieves users' saved password so that it can be evaluated during the process of authentication. Passwords must be hashed.
- Is_admin: boolean
  - If user has administrative power, true is returned, else: return false. This essentially aids when deciding what system features can access. 
- set_admin(employee_pin): int
  - Grants admin power if the employee PIN that has been entered is accurate. This process verifies that the PIN entered and the one on file matches. 
- login(username, password): boolean
  - Authenticates user, comparing the password and username entered as well. If correct: return true ; allowing system access 
- logout()
  - Logs user out; makes sure that no other operations are carried out after logging out.

### Item Object
An item is anything in the store that can be purchased. Each item has an identifying ID that make it easy to search for in the inventory. When the barcode gets scanned, it searches in the item hashmap for the item. The item can also be searched for manually using the other identifiers, the name, color, size, and amount. This class has methods for retrieving the identifiers.

Attributes: 
- quantity: int
  - Total number of units of x item that is in stock. Whenever there is new inventory, the value rises and falls corresponding to the number of things purchased.
- item_ID: string
  - Number given to every item in the store; used to look for the item in the system (manually) 
- price: int
  - The price of one unit of x product; price may be shown to the user or retrieved for processing a purchase.
- size: string
  - An item’s physical measurements or size classification. Ex: small, XL
- color: string
  - Makes it easier to distinguish goods that come in several color variations, allowing for a search in the inventory to be more precise.
- name: string
  - An item’s name; helps consumers and staff to recognize a product.
  
Operations:
- get_quantity(): int
  - Returns an item’s stock at the moment; verifies availability.
- get_item_ID(): string
  - Obtains item ID, used for manual searches in the inventory system.
- get_price(): int
  - Retrieves the item’s current price. Can be used to determine the entire cost or show price to user when checking out.
- get_size(): string
  - Obtains the object’s size, helpful for showing data about the item or looking up size variations. 
- get_color(): string
  - Returns item color; can help to distinguish items with the same name.
- get_name(): string
  - Returns item name, facilitates inventory searches. 
- sub_quantity()
  - Reduces item inventory by x amount, usually applied after a purchase.

### Inventory Class
Each item object is stored here in the inventory hashmap. Using a hash is easier since it allows for O(1) runtime rather than O(n) runtime after accessing an array. Each item ID is the “key” and the item object is what is returned. The methods in this class allow you to add an item object, remove an item, retrieve an item or print a full list of the inventory.

Attributes: 
- item_dictionary: hashmap
  - Hashmap with matching object as value as well as the item ID; used to store item objects
- inventory: array[item objects]
  - An array with every item object that is included in the system. An inventory list is provided by the array and can be repeatedly used for different things like checking stock.
- edit_queue: array[item objects]
  - An improvised array used to store item objects until modifications are approved. Before edits are applied, items in this queue that have been updated by staff must be evaluated.
  
Operations:
- get_item(item_ID): item object
  - Uses item ID to retrieve x item from dictionary. 
- get_inventory(): array[item objects]
  - Returns the inventory array’s complete list of item objects. 
- add_to_inventory(item object)
  - Adds new item object to inventory array and dictionary; keeps ordered list of all objects.
- remove_from_inventory(item object)
  - Item is removed from the inventory array and dictionary. 
- edit_inventory(item object)
  - Adds items to the edit queue, allowing inventory item info to be updated.
- approve_changes(boolean)
  - Approves or disapproves modifications made to the edit queue items. Relevant item objects are updated based on modifications. 

### User Interface
The system's interactive features and layout are controlled by the UI class. The interface is loaded to a ui file, presenting elements for user interaction.

Attributes:
- interface layout: .ui file
  - A ui file which specifies the interface’s graphical design for the user.
  
Operations:
- display_ui()
  - Uses what is specified in the ui file to load the user interface.
- update_ui()
  - In response to user input, the user interface is refreshed. 

### Checkout Class
The checkout option allows employees to checkout and give returns to customers. Utilizing transaction objects, employees are able to create custom objects and then update the transaction history through an itreative looping process at the time of checkout. The customer is delivered the cost including the tax, and once payment is confirmed, the system will update the history and the inventory list.

Attributes:
- n/a

Operations:
- checkout_customer()
  - Creates a transaction object and begins an interactive loop where the employee is prompted repeatedly to either enter a new item to check out, or complete the transaction. Each time the new item is scanned, the object is sent to the transaction object. Once the loop is ended by completing the transaction, and the payment is confirmed, the transaction object is sent to the transaction history, and a receipt is printed for the customer.
- return_object(transaction_ID)
  - Creates a new transaction and copies all the information over from the ID supplied by the customer. It then sets the total as negative the original total of the transaction. After confirming the return of money to the customer, this transaction object is then sent to the transaction history.

#### Transaction History Class
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

