Inventory Management System (IMS)

What is IMS?
Basically here there are products which are kept at warehouses and system to handle products

📝 Problem Statement

Build a simplified Inventory Management System (IMS) to manage products, warehouses, and their stock.

The system should allow:

Adding/removing products

Adding/removing warehouses

Adding, removing, and transferring stock

Handling deletion rules consistently

Querying stock levels

🎯 Design Approach

We model the problem around 3 core entities:

Product → uniquely identified item (e.g., Laptop, Phone).

Warehouse → physical storage location (e.g., Bangalore, Delhi).

WarehouseProduct (association) → binds a product to a warehouse with a stock quantity.

Managers

To ensure separation of concerns:

InventoryMgr → Product lifecycle (add/remove).

WarehouseMgr → Warehouse lifecycle (add/remove).

WarehouseProductMgr → Stock management (add/remove/transfer).

Facade

A InventoryManagementSystem class acts as the entry point, exposing clean high-level APIs.
Clients don’t interact with managers directly → they call IMS methods (like addProduct, transferStock).

⚙️ Functional Requirements & Flow

Here’s how each requirement is mapped to flow + handled in design:

1. Add/Remove Product

Flow:
IMS → InventoryMgr

Handled By:

InventoryMgr::addProduct() → Adds product globally.

InventoryMgr::removeProduct() → Removes product and triggers cleanup in WarehouseProductMgr.

2. Add/Remove Warehouse

Flow:
IMS → WarehouseMgr

Handled By:

WarehouseMgr::addWarehouse() → Registers a warehouse.

WarehouseMgr::removeWarehouse() → Deletes warehouse and cleans stock from WarehouseProductMgr.

3. Add Stock to Warehouse

Flow:
IMS → WarehouseProductMgr

Handled By:

WarehouseProductMgr::addStock(warehouseId, productId, qty) → Adds product quantity to the warehouse.

4. Remove Stock from Warehouse

Flow:
IMS → WarehouseProductMgr

Handled By:

WarehouseProductMgr::removeStock(warehouseId, productId, qty) → Decreases product quantity, validates non-negative.

5. Transfer Stock Between Warehouses

Flow:
IMS → WarehouseProductMgr

Handled By:

Validates source stock availability.

Deducts from source warehouse.

Adds to destination warehouse.

6. Delete Rules

Deleting a Product:

Remove from InventoryMgr.

Clear all warehouse-product stock associations in WarehouseProductMgr.

Deleting a Warehouse:

Remove from WarehouseMgr.

Remove all stock tied to that warehouse in WarehouseProductMgr.

7. Query Stock

Flow:
IMS → WarehouseProductMgr

Handled By:

getStock(warehouseId, productId) → Returns stock count for a given product in a warehouse.
