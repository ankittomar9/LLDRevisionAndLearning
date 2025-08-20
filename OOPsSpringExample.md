To illustrate the four pillars of OOP (Encapsulation, Inheritance, Polymorphism, and Abstraction) in a Spring Boot context, let's use a common scenario: building a simple **REST API for managing products**. We'll create a controller, a service, and a repository.

## Encapsulation

Encapsulation involves bundling data and methods into a single unit (a class) and controlling access to them. In Spring Boot, this is a fundamental practice for creating **model classes** or **entities**.

**Example:** A `Product` class.

```java
package com.example.demo.model;

import javax.persistence.Entity;
import javax.persistence.Id;

@Entity // Marks this class as a JPA entity
public class Product {

    @Id
    private Long id;
    private String name;
    private double price;

    // Getters and setters provide controlled access to private fields
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}
```

Here, the `id`, `name`, and `price` fields are **private**. You can't directly modify them from outside the class. Instead, you must use the `public` getter and setter methods (`getId()`, `setName()`, etc.). This protects the internal state of the `Product` object.

-----

## Inheritance

Inheritance allows a class to inherit properties and methods from another. While not as common for simple REST entities, it's very useful for creating a hierarchy of related services or components.

**Example:** Creating a generic `BaseService` for CRUD operations that more specific services can extend.

```java
// Base class with common methods
public abstract class BaseService<T, ID> {
    public abstract T save(T entity);
    public abstract void deleteById(ID id);
    // Other common methods
}

// Subclass inheriting from BaseService
@Service
public class ProductService extends BaseService<Product, Long> {
    private final ProductRepository productRepository;

    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    @Override
    public Product save(Product product) {
        return productRepository.save(product);
    }

    @Override
    public void deleteById(Long id) {
        productRepository.deleteById(id);
    }
    
    // Additional methods specific to ProductService
    public Product findById(Long id) {
        return productRepository.findById(id).orElse(null);
    }
}
```

Here, `ProductService` inherits the common `save` and `deleteById` methods, preventing code duplication. It then adds its own specific method, `findById()`.

-----

## Polymorphism

Polymorphism allows a single interface to be used for different underlying forms. In Spring, this is very common with **dependency injection** and **interfaces**.

**Example:** The `@Repository` interface provided by Spring Data JPA.

```java
package com.example.demo.repository;

import com.example.demo.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository // Marks this as a repository component
public interface ProductRepository extends JpaRepository<Product, Long> {
    // Spring Data JPA automatically provides CRUD methods
    // like save(), findById(), and deleteById()
}
```

The `JpaRepository` is an interface. When you use dependency injection (`@Autowired` or constructor injection) to get a `ProductRepository`, Spring provides a concrete implementation of that interface at runtime. You, as the developer, don't need to know the specific implementation details; you just use the methods defined in the interface. This is a powerful example of **polymorphism** in action, where the same `JpaRepository` interface can be used for different entities (e.g., `Product`, `Customer`, `Order`).

-----

## Abstraction

Abstraction involves hiding complex implementation details and showing only the essential features. Spring's framework itself is built on abstraction, particularly with **interfaces**.

**Example:** The `ProductService` interface and its implementation.

```java
// Abstract interface that defines the contract
public interface ProductService {
    Product getProductById(Long id);
    Product createProduct(Product product);
    void deleteProduct(Long id);
}

@Service // The concrete implementation
public class ProductServiceImpl implements ProductService {
    private final ProductRepository productRepository;

    // Constructor injection
    public ProductServiceImpl(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    @Override
    public Product getProductById(Long id) {
        // Implementation details are hidden
        return productRepository.findById(id).orElse(null);
    }

    @Override
    public Product createProduct(Product product) {
        // Implementation details are hidden
        return productRepository.save(product);
    }

    @Override
    public void deleteProduct(Long id) {
        // Implementation details are hidden
        productRepository.deleteById(id);
    }
}
```

In this case, the `ProductService` interface provides a public contract (what methods are available). A developer using this service in a controller, for example, only needs to know about the methods defined in the **interface**, not how they are actually implemented in the `ProductServiceImpl` class. This allows for easy swapping of implementations later without affecting the rest of the application.