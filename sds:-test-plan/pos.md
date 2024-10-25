## Test Cases:

### Inventory

#### Unit Testing

#### Integration Testing

Test Case #1

This test is targeting the inventory updating system, if a user requests an item be edited, and the admin approves that change request, then the catalog should be updated.

INPUT:
```
User.login(admin, manager123)                           #  logs in with admin login
Inventory.get_item(1234aaa)                             #  gets item before edits are made
Inventory.edit_inventory(Inventory.get_item(1234aaa))   #  enters user interactive loop to find changes
>> Item.price = 15
>> Item.name = Satin Red Dress
Inventory.approve_changes(true)                         #  as admin, approve changes
Inventory.get_item(1234aaa)                             #  get item after edits are made
```
OUTPUT:
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

INPUT:
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

OUTPUT:
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

INPUT:
```
User.login(admin, manager123)                           #  logs in with admin login
Inventory.get_item(1236abc)                             #  gets item before edits are made
Inventory.remove_from_inventory(Inventory.get_item(1236abc))  #  remove item
Inventory.approve_changes(true)                         #  as admin, approve changes
Inventory.get_item(1236abc)                             #  get item after edits are made
```

OUTPUT:
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

INPUT:
```
User.login(employee, password123)                       #  logs in with employee login
Inventory.get_item(1237bbc)                             #  gets item before edits are made
Inventory.edit_inventory(Inventory.get_item(1237bbc))   #  enters user interactive loop to find changes
>> Item.quantity = 100
>> Item.price = 12
Inventory.approve_changes(true)                         #  approve changes
Inventory.get_item(1237bbc)                             #  get item after edits are made
```

OUTPUT:
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



### Checkout

#### Unit Testing

#### Integration Testing

Test Case #1

This test case is targeting the checkout, making sure that the checkout can access inventory items and transaction objects, verifying through adding an item to the cart and checking out the item.

INPUT:
```
Checkout.checkout_customer()                                  #  enters user interactive loop to add items to cart
>> Transaction.add_item_to_cart(Inventory.get_item(1234aaa))  #  scans item, which adds to cart
>> complete transaction
```
OUTPUT:
```
RECEIPT:
Red Dress . . . . . . . . . . . . . $10.00
Tax . . . . . . . . . . . . . . . . $ 1.25
------------------------------------------
Total . . . . . . . . . . . . . . . $11.25
```

Test Case #2

This test case is targeting the checkout, making sure that the checkout can access inventory items and transaction objects, verifying through adding an item to the cart and then canceling the transaction

INPUT:
```
Checkout.checkout_customer()                                  #  enters user interactive loop to add items to cart
>> Transaction.add_item_to_cart(Inventory.get_item(1236abc))  #  scans item, which adds to cart
>> cancel transaction
```
OUTPUT:
```
Transaction canceled
```

Test Case #3

This test case is targeting the checkout, making sure that the checkout can access inventory items and transaction objects, verifying through adding multiple items to the cart and checking out the items.

INPUT:
```
Checkout.checkout_customer()                                  #  enters user interactive loop to add items to cart
>> Transaction.add_item_to_cart(Inventory.get_item(1234aaa))  #  user scans item, which adds to cart
>> Transaction.add_item_to_cart(Inventory.get_item(1235aba))  #  user scans item, which adds to cart
>> Transaction.add_item_to_cart(Inventory.get_item(1236abc))  #  user scans item, which adds to cart
>> complete transaction
```
OUTPUT:
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

