# Point of Sale System

## Table of Contents
- [Authors](#authors)
- [System Description](#system-description)
- [Documentation Updates](#documentation-updates)
- [Software Archetecture Diagram](#software-archetecture-diagram)
  - [Key](#key)
  - [Physical Interface](#physical-interface)
  - [Login](#login)
  - [Software Options](#software-options)
  - [Transactions](#transactions)
  - [Data Storage](#data-storage)
- [Data Managment Strategy](#data-managment-strategy)
  - [Users Table](#users-table)
  - [Inventory Table](#inventory-table)
  - [Transaction History Table](#transaction-history-table)
  - [Cart Table](#cart-table)
  - [Inventory Edit Requests Table](#inventory-edit-requests-table)
- [Unified Modeling Language Class Diagram](#unified-modeling-language-class-diagram)
  - [User Class](#user-class)
  - [Item Object](#item-object)
  - [Inventory Class](#inventory-class)
  - [User Interface](#user-interface)
  - [Checkout Class](#checkout-class)
  - [Transaction History Class](#transaction-history-class)
  - [Transaction Object](#transaction-object)
- [Development Plan and Timeline](#development-plan-and-timeline)
- [Test Cases](#test-cases)
  - [Inventory](#inventory)
    - [Inventory Unit Testing](#inventory-unit-testing)
    - [Inventory Integration Testing](#inventory-integration-testing)
    - [Inventory System Testing](#inventory-system-testing)
  - [Checkout](#checkout)
    - [Checkout Unit Testing](#checkout-unit-testing)
    - [Checkout Integration Testing](#checkout-integration-testing)
    - [Checkout System Testing](#checkout-system-testing)

## Authors
Isabelle Viraldo

Nicholas Moffat

Seven Blackwell

## System Description
This is the document describing the development of a catalog system for any clothing store to buy, and to implement. This system excels at documenting the inventory and managing transactions. This system will be utilized by managers and employees to ensure inventory information is efficiently and accurately maintained. 

The system is designed to streamline sales processes, enabling employees to complete transactions, tracking purchases and returns, while automatically updating inventory. Employees can checkout customers with automatic tax calculators, and after completing purchase, the system automatically updates the inventory catalog. Inventory features will include search capabilities by item ID or name, and allow employees to add new items with specific attributes such as price and size. These inventory changes made by the employees must be confirmed by the Admin of the system before implementation into the database. All data, including transaction history and inventory, will be stored in a cloud database. This system is an integral part of managing a clothing store.

## Documentation Updates

For the first iteration of this document, the document was created, UML Diagram was designed, and the project timeline was designed. 

Then, in the second iteration of this project, the SWA diagram was added, as well as the Test cases. 

Now, in this third itration of the document, the data managment strategy has been created.

## Software Archetecture Diagram
![Screenshot 2024-11-02 122328](https://github.com/user-attachments/assets/1ad64da5-5f58-48d7-90d7-3e6f50bd7e3b)


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

## Data Managment Strategy
![image](https://github.com/user-attachments/assets/4527dd1d-3c8a-4c49-b29e-57e6d1fe9354)

As shown above in the Software Architecture Diagram, this is the orginization of the databases. When it comes to the overall management of the data within our system, SQL has the best tools and speed to accomplish the system's needs. It works better than non-SQL programs because of the security it provides the system. A combination of different SQL databases are used to organize data for user management, inventory, inventory requests, cart and transactions. These databases being used in SQL show the teams focus on the integrity of data, scalability and efficiency of the system. Using separate databases allows for the ability to isolate certain data from general inventory data.

### Users Table

Within this database, the employees login information as well as credentials are stored. SQL is used to ensure the security of the information, as well provide easy control access to the data.

To initialize the database in SQL, do:
```
CREATE TABLE Users (employee_pin int, username string, password string, admin_privileges boolean);
```

### Inventory Table

This is where all the information regarding an item will be stored, including the item ID for quick access, price, stock, size and color. This table will work in tandem with the inventory edit requests database to update the inventory as needed as well as with the cart database.

To initialize the database in SQL, do:
```
CREATE TABLE Inventory (item_ID string, name string, price int, quantity int, size string, color string);
```

### Transaction History Table

This database is used to track all of the transactions that are processed by the system. Each transaction is given an ID, a purchase date, and a total. A return boolean is optional and is given on a case by case basis. 

To initialize the database in SQL, do:
```
CREATE TABLE Transaction History (transaction_ID string, purchase_date date, cart_ID int, total double, isReturn boolean);
```

### Cart Table

Before checking out, items that customers have chosen to buy are stored in the cart table. Each item, represented by the item fields, can be stored in each cart which is uniquely recognized by its cart_ID. To connect the cart to a completed transaction when the purchase is finished, the database additionally contains a transaction_ID. 

To initialize the database in SQL, do:
```
CREATE TABLE Cart (cart_ID int, item_1 string, item_2 string, item_3 string, item_4 string, item_5 string, item_6 string, transaction_ID string);
```

### Inventory Edit Requests Table

Inventory changes are managed by the inventory edit request table. Requests have an item_ID and if an item needs to be removed, the request will contain a remove flag. Otherwise, any changes to the item's name, price, quality, color, and size will be recorded in the other fields. Ultimately, the table makes sure that any changes to an item are reviewed to ensure accuracy. 

To initialize the database in SQL, do:
```
CREATE TABLE Inventory Edit Requests (item_ID string, remove boolean, name string, price int, quantity int, size string, color string);
```

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


## Test Cases:

### Inventory

#### Inventory Unit Testing

Test Case #1: Add to inventory

This test targets the inventory system, increasing the number of a specified item.

Input:
```
inventory.add_item(String itemID, 2)
```
Output
```
Inventory was updated:
Name: Hat
Item ID: 87hsk71
Price ($): 15
In stock: 15
Size: None
```

Test Case #2: Remove from inventory

This test again targets the inventory system, decreasing the number of a specified item. This would be used in conjunction with checking out customers.

Input:
```
inventory.remove_item(String itemID, 4)
```
Output
```
Inventory was updated:
Name: Hat
Item ID: 87hsk71
Price ($): 15
In stock: 11
Size: None
Color: Red
```
Test Case #3: Remove more than available

This test makes sure that the system does not allow you to remove more than is available of a certain item.

Input:
```
inventory.item_info(String itemID)
inventory.remove_item(String itemID, 3)
```
Output
```
Name: Hat
Item ID: 87hsk71
Price ($): 15
In stock: 1
Size: None
Color: Red
Cannot remove specified number as amount is invalid
```
Test Case #4: Invalid itemID

This test targets the inventory system and makes sure that invalid item ID’s are handled correctly

Input:
```
inventory.add_item(String itemID, 3)
```
Output
```
Specified item ID does not match any known items in inventory.
```

#### Inventory Integration Testing

Test Case #1

This test is targeting the inventory updating system, if a user requests an item be edited, and the admin approves that change request, then the catalog should be updated.

Input:
```
User.login(admin, manager123)                           #  logs in with admin login
Inventory.get_item(1234aaa)                             #  gets item before edits are made
Inventory.edit_inventory(Inventory.get_item(1234aaa))   #  enters user interactive loop to find changes
>> Item.price = 15
>> Item.name = Satin Red Dress
Inventory.approve_changes(true)                         #  as admin, approve changes
Inventory.get_item(1234aaa)                             #  get item after edits are made
```
Output:
```
True                                                    #  logs in

Name: Red Dress                                         #  prints out item object before edits are made
Item ID: 1234aaa
Price ($): 10
In stock: 12
Size: Medium
Color: Red

Name: Satin Red Dress                                   #  prints out item object after edits are made
Item ID: 1234aaa
Price ($): 15
In stock: 12
Size: Medium
Color: Red
```

Test Case #2

This test is targeting the inventory updating system, if a user requests an item be edited, and the admin disapproves that change request, then the catalog should not be updated.

Input:
```
User.login(admin, manager123)                           #  logs in with admin login
Inventory.get_item(1235aba)                             #  gets item before edits are made
Inventory.edit_inventory(Inventory.get_item(1235aba))   #  enters user interactive loop to find changes
>> Item.color = Teal
>> Item.price = 0
>> Item.quantity = 20
Inventory.approve_changes(false)                        #  as admin, disapprove changes
Inventory.get_item(1235aba)                             #  get item after edits are made
```

Output:
```
True                                                    #  logs in

Name: Pink Skirt                                        #  prints out item object before edits are made
Item ID: 1235aba
Price ($): 11
In stock: 16
Size: Small
Color: Pink

Name: Pink Skirt                                        #  prints out item object without any changes
Item ID: 1235aba
Price ($): 11
In stock: 16
Size: Small
Color: Pink
```

Test Case #3

This test is targeting the inventory updating system, if a user requests an item be deleted, and the admin approves that change request, then the catalog should be updated.

Input
```
User.login(admin, manager123)                           #  logs in with admin login
Inventory.get_item(1236abc)                             #  gets item before edits are made
Inventory.remove_from_inventory(Inventory.get_item(1236abc))  #  remove item
Inventory.approve_changes(true)                         #  as admin, approve changes
Inventory.get_item(1236abc)                             #  get item after edits are made
```

Output:
```
True                                                    #  logs in

Name: Blue Jeans                                        #  prints out item object before edits are made
Item ID: 1236abc
Price ($): 10
In stock: 3
Size: Large
Color: Blue

Error: item (1236abc) not found                         #  prints out error code because item no longer exists
```

Test Case #4

This test is targeting the inventory updating system, if a user requests an item be edited, and a user without admin credentials approves that change request, then the catalog should not be updated.

Input:
```
User.login(employee, password123)                       #  logs in with employee login
Inventory.get_item(1237bbc)                             #  gets item before edits are made
Inventory.edit_inventory(Inventory.get_item(1237bbc))   #  enters user interactive loop to find changes
>> Item.quantity = 100
>> Item.price = 12
Inventory.approve_changes(true)                         #  approve changes
Inventory.get_item(1237bbc)                             #  get item after edits are made
```

Output
```
True                                                    #  logs in

Name: Orange Flannel                                    #  prints out item object before edits are made
Item ID: 1237bbc
Price ($): 10
In stock: 30
Size: Medium
Color: Orange

Error: User does not have the correct credentials       #  prints out error code because user is Employee, not Admin

Name: Orange Flannel                                    #  prints out item object without any changes
Item ID: 1237bbc
Price ($): 10
In stock: 30
Size: Medium
Color: Orange
```

#### Inventory System Testing

Test Case #1

This test checks if staff can efficiently look for things in the inventory utilizing the item’s name or ID. It guarantees that search requests can be handled easily and correctly, even if valid or invalid. 

Test Steps:
- Login to system as staff member and go to the inventory page.
- Enter the item ID in the search field (ex. Z63yd8 = brown shoes)
- Verify that the details of the item that are returned by the system are correct.
- Clear the search, type the name of the item.
- Verify that the details are correct again.
- Clear the search, enter a non existent item name
- Make sure that the system returns an error message

Expected Results:
- When the item name and ID is searched, the correct details are displayed.
- An error is displayed if the item does not exist or if the ID/name is incorrect.

Test Case #2

This test verifies that any modifications in the inventory that are made by staff (ex. Changing item ID) requires admin permission. Additionally, it ensures that the administrator can effectively review said changes and approve them before there are official updates.

Test Steps:
- Enter login credentials as an employee and try to add an item to inventory.
- Make sure that in the inventory, the item’s status is pending. 
- After log out, log back in with administrative credentials.
- Access the pending item, make sure that the modifications are correct, and approve the change.
- Confirm that the item has been added to the inventory and is searchable by name and ID.
- Try to update the quantity of an item that was already in the inventory as an administrator, then confirm changes.
- Make sure that the system accurately shows the changes. 

Expected Results:
- Employee’s new item shouldn’t show up in the inventory unless the administrator approves.
- The new item should show once the administrator approves the modifications.
- After the admin gives permission, inventory updates should be implemented in any associated transactions as well.

### Checkout

#### Checkout Unit Testing

Test Case #1: Add items to cart

This test makes sure that multiple items can be added to the cart before a purchase.

Input:
```
transaction.add_item_to_cart(String itemID, 2)
transaction.add_item_to_cart(String itemID, 1)
transaction.add_item_to_cart(String itemID, 1)
transaction.display_cart()
```
Output:
```
Cart:
Name: Hat
Item ID: 87hsk71
Count: 2
Price ($): 30
—-------------
Name: Coat
Item ID: hsk972h
Price ($): 70
—-------------
Name: Jeans
Item ID: slab61s
Price ($): 45
```
Test Case #2: Remove items from cart

This test makes sure that items can be removed from the cart before purchase.

Input:
```
transaction.display_cart()
transaction.remove_item_from_cart(String itemID, 1)
transaction.remove_item_from_cart(String itemID, 1)
transaction.display_cart()
```
Output:
```
Cart:
Name: Hat
Item ID: 87hsk71
Count: 1
Price ($): 15
—-------------
Name: Coat
Item ID: hsk972h
Price ($): 70
—-------------
Name: Jeans
Item ID: slab61s
Price ($): 45

Cart:
Name: Hat
Item ID: 87hsk71
Count: 1
Price ($): 15
—-------------
Name: Jeans
Item ID: slab61s
Price ($): 45
```

Test Case #3: Promo code application before checkout

This test makes sure that after a promo code is applied, the price is updated accordingly.

Input:
```
transaction.display_total()
transaction.apply_promo_code(String promoID)
transaction.display_total()
```
Output:
```
Red Hat  . . . . . . . . . . $15.00
Jeans  . . . . . . . . . . . $45.00
------------------------------------------
Total . . . . . . . . . . . . . . . $60.00

Promo code applied!

Red Hat  . . . . . . . . . . $12.00
- $3.00 (20% promo code)
Jeans  . . . . . . . . . . . $36.00
	- $9.00 (20% promo code)
------------------------------------------
Total . . . . . . . . . . . . . . . $48.00
```

#### Checkout Integration Testing

Test Case #1

This test case is targeting the checkout, making sure that the checkout can access inventory items and transaction objects, verifying through adding an item to the cart and checking out the item.

Input:
```
Checkout.checkout_customer()                                  #  enters user interactive loop to add items to cart
>> Transaction.add_item_to_cart(Inventory.get_item(1234aaa))  #  scans item, which adds to cart
>> complete transaction
```
Output:
```
RECEIPT:
Red Dress . . . . . . . . . . . . . $10.00
Tax . . . . . . . . . . . . . . . . $ 1.25
------------------------------------------
Total . . . . . . . . . . . . . . . $11.25
```

Test Case #2

This test case is targeting the checkout, making sure that the checkout can access inventory items and transaction objects, verifying through adding an item to the cart and then canceling the transaction

Input:
```
Checkout.checkout_customer()                                  #  enters user interactive loop to add items to cart
>> Transaction.add_item_to_cart(Inventory.get_item(1236abc))  #  scans item, which adds to cart
>> cancel transaction
```
Output:
```
Transaction canceled
```

Test Case #3

This test case is targeting the checkout, making sure that the checkout can access inventory items and transaction objects, verifying through adding multiple items to the cart and checking out the items.

Input:
```
Checkout.checkout_customer()                                  #  enters user interactive loop to add items to cart
>> Transaction.add_item_to_cart(Inventory.get_item(1234aaa))  #  user scans item, which adds to cart
>> Transaction.add_item_to_cart(Inventory.get_item(1235aba))  #  user scans item, which adds to cart
>> Transaction.add_item_to_cart(Inventory.get_item(1236abc))  #  user scans item, which adds to cart
>> complete transaction
```
Output:
```
RECEIPT:
Red Dress . . . . . . . . . . . . . $10.00
Pink Skirt  . . . . . . . . . . . . $11.00
Blue Jeans. . . . . . . . . . . . . $10.00
Tax . . . . . . . . . . . . . . . . $ 3.75
------------------------------------------
Total . . . . . . . . . . . . . . . $34.75
```

#### Checkout System Testing

Test Case #1

This test makes sure that the checkout procedure is simple, the data is stored correctly, and that the inventory is updated; It emphasizes the system’s ability to manage transactions.

Test Steps:
- Launch a new transaction.
- Either manually input or scan an item twice.
- Make sure that the system computes the total accurately, including any other prices like tax.
- Accept the payment and wait for the system to verify.
- After the transaction, make sure that the inventory is updated. ex. Two less of x item.
- Check that the transaction was logged correctly, such as the time and date.

Expected Results:
- System performs the correct calculations.
- The inventory and stock information is updated.
- The purchase is added to the transaction history with the correct information.

Test Case #2

This test ensures that the user can login and complete a return, that the data is stored correctly, and the transaction history is updated. This showcases the use for both customer and manager.

Test Steps:
- Launch a return transaction.
- Enter the receipt ID.
- Make sure that the system returns the total accurately.
- Wait for the system to verify.
- After the transaction, make sure that the inventory is updated. ex. One more of x item.
- Check that the transaction was logged correctly, such as the time and date.

Expected Results:
- System returns correct amount.
- The inventory and stock information is updated.
- The return is added to the transaction history with the correct information.
