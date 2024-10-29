public class Product {
    private String id;
    private String name;
    private int quantity;

    public Product(String id, String name, int quantity) {
        this.id = id;
        this.name = name;
        this.quantity = quantity;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public int getQuantity() {
        return quantity;
    }

    public void addQuantity(int amount) {
        this.quantity += amount;
    }

    public boolean removeQuantity(int amount) {
        if (amount > quantity) {
            return false;
        }
        this.quantity -= amount;
        return true;
    }

    @Override
    public String toString() {
        return "Product{" +
                "id='" + id + '\'' +
                ", name='" + name + '\'' +
                ", quantity=" + quantity +
                '}';
    }
}


import java.util.HashMap;
import java.util.Map;

public class StorageLocation {
    private String locationId;
    private Map<String, Product> products;

    public StorageLocation(String locationId) {
        this.locationId = locationId;
        this.products = new HashMap<>();
    }

    public String getLocationId() {
        return locationId;
    }

    public boolean addProduct(Product product) {
        products.put(product.getId(), product);
        return true;
    }

    public Product getProduct(String productId) {
        return products.get(productId);
    }

    public Product removeProduct(String productId, int quantity) {
        Product product = products.get(productId);
        if (product != null && product.removeQuantity(quantity)) {
            if (product.getQuantity() == 0) {
                products.remove(productId); // Remove the product if quantity is zero
            }
            return product;
        }
        return null;
    }

    @Override
    public String toString() {
        return "StorageLocation{" +
                "locationId='" + locationId + '\'' +
                ", products=" + products.values() +
                '}';
    }
}

import java.util.HashMap;
import java.util.Map;

public class Warehouse {
    private Map<String, StorageLocation> storageLocations;

    public Warehouse() {
        storageLocations = new HashMap<>();
    }

    public void addStorageLocation(StorageLocation location) {
        storageLocations.put(location.getLocationId(), location);
    }

    public StorageLocation getStorageLocation(String locationId) {
        return storageLocations.get(locationId);
    }

    @Override
    public String toString() {


public class WarehouseManager {
    private Warehouse warehouse;

    public WarehouseManager(Warehouse warehouse) {
        this.warehouse = warehouse;
    }

    public void addProduct(String locationId, Product product) {
        StorageLocation location = warehouse.getStorageLocation(locationId);
        if (location != null) {
            location.addProduct(product);
            System.out.println("Added " + product + " to location " + locationId);
        } else {
            System.out.println("Storage location does not exist.");
        }
    }

    public void pickProduct(String locationId, String productId, int quantity) {
        StorageLocation location = warehouse.getStorageLocation(locationId);
        if (location != null) {
            Product product = location.removeProduct(productId, quantity);
            if (product != null) {
                System.out.println("Picked " + quantity + " of " + productId + " from location " + locationId);
            } else {
                System.out.println("Product not available or insufficient quantity.");
            }
        } else {
            System.out.println("Storage location does not exist.");
        }
    }

    public void displayWarehouse() {
        System.out.println(warehouse);
    }
}

public class Main {
    public static void main(String[] args) {
        // Create warehouse and storage locations
        Warehouse warehouse = new Warehouse();
        StorageLocation locationA = new StorageLocation("A");
        StorageLocation locationB = new StorageLocation("B");

        // Add locations to the warehouse
        warehouse.addStorageLocation(locationA);
        warehouse.addStorageLocation(locationB);

        // Create warehouse manager
        WarehouseManager manager = new WarehouseManager(warehouse);

        // Create products
        Product product1 = new Product("P001", "Widget", 100);
        Product product2 = new Product("P002", "Gadget", 50);

        // Add products to locations
        manager.addProduct("A", product1);
        manager.addProduct("B", product2);

        // Pick some products
        manager.pickProduct("A", "P001", 30);
        manager.pickProduct("B", "P002", 10);
        manager.pickProduct("B", "P002", 60); // Attempt to pick more than available
        
        // Display current state of the warehouse
        manager.displayWarehouse();
    }
}
