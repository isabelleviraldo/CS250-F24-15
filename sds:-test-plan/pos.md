## Test Cases:

### Inventory

#### Unit Testing

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

#### Integration Testing

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

#### System Testing

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

#### Unit Testing

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

#### Integration Testing

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

#### System Testing

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
