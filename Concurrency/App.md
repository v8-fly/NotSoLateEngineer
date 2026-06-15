# SwiggyX — Capstone App

### Building everything we learned into a real working backend.

> Java 21 + Spring Boot + PostgreSQL
> Every concept from the learning journey — active, visible, running.

---

## What We Are Building

A Food Delivery Backend that actively uses:

| Feature                           | Concept Used                          |
| --------------------------------- | ------------------------------------- |
| Handle 1000 orders simultaneously | Virtual Thread Pool                   |
| Two users ordering last item      | Race Condition + Mutex (synchronized) |
| DB connection limiting            | Semaphore                             |
| Payment + Inventory together      | Deadlock Prevention (Lock Ordering)   |
| Background notifications          | Message Queue simulation              |
| Fraud detection                   | Platform Thread Pool (CPU work)       |

---

## Tech Stack

```
Language:      Java 21
Framework:     Spring Boot 3.2.5
Database:      PostgreSQL 14
Build Tool:    Maven
IDE:           IntelliJ
OS:            Mac (Apple Silicon)
```

---

## Environment Setup

### PostgreSQL Setup

```
Database:  swiggyx
User:      swiggyx_user
Password:  swiggyx123
Port:      5432
```

Commands used to create:

```sql
CREATE DATABASE swiggyx;
CREATE USER swiggyx_user WITH PASSWORD 'swiggyx123';
GRANT ALL PRIVILEGES ON DATABASE swiggyx TO swiggyx_user;
```

Verified in PgAdmin:

```
Databases → swiggyx ✅
Login/Group Roles → swiggyx_user ✅
```

---

## Project Setup

### pom.xml — Explained

`pom.xml` is Maven's configuration file. Answers three questions:

```
1. What is this project?     → groupId, artifactId, version
2. What does it need?        → dependencies
3. How should it be built?   → build plugins
```

#### Project Identity

```xml
<groupId>com.swiggyx</groupId>      <!-- organisation name -->
<artifactId>swiggyx</artifactId>    <!-- project name -->
<version>1.0-SNAPSHOT</version>     <!-- first version, in development -->
```

#### Spring Boot Parent

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.5</version>
</parent>
```

Our project inherits from Spring Boot's parent.
Gives us:

```
→ All Spring Boot default configurations
→ Compatible versions of all libraries automatically
→ No need to specify versions manually
```

#### Java Version

```xml
<properties>
    <java.version>21</java.version>
</properties>
```

Java 21 — required for Virtual Threads.

#### Dependencies

**Spring Web** — receive HTTP requests

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Gives us:

```
→ HTTP server (Tomcat runs inside our app)
→ REST API support (@RestController, @GetMapping)
→ JSON conversion automatically
→ Without this — app can't receive HTTP requests
```

**Spring Data JPA** — talk to database

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

JPA = Java Persistence API.
Gives us:

```
→ Talk to DB without writing SQL manually
→ Define Java class → JPA creates DB table automatically
→ Save, find, delete objects without SQL
→ Hibernate runs underneath

// Instead of SQL:
// SELECT * FROM orders WHERE id = 101
// You write:
orderRepository.findById(101); // JPA handles SQL
```

**PostgreSQL Driver** — USB cable between Java and PostgreSQL

```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

```
→ Actual connector between Java and PostgreSQL
→ scope=runtime: needed when running, not compiling
→ Without this — Java can't talk to PostgreSQL
```

**Lombok** — reduces boilerplate

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
```

```
Without Lombok:                    With Lombok:
public class Order {               @Data
    private String id;             public class Order {
    public String getId()...           private String id;
    public void setId()...             private String userId;
    public String toString()...    }
    // 50 lines of boilerplate
}
```

#### Build Plugin

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```

```
→ mvn spring-boot:run → starts our server
→ Packages everything into one runnable JAR
→ Without this → can't run Spring Boot from Maven
```

#### Complete pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.swiggyx</groupId>
    <artifactId>swiggyx</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.5</version>
        <relativePath/>
    </parent>

    <properties>
        <java.version>21</java.version>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>

        <!-- Spring Web — REST APIs -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Spring Data JPA — database operations -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- PostgreSQL Driver -->
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- Lombok — reduces boilerplate -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- Spring Boot Test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

---

_Document continues as we build..._

_Last updated: pom.xml setup_

---

## Java & Maven Setup

### Problem Faced

Maven was using Java 25 (Homebrew) instead of Java 17.
Fix: enable jenv maven plugin so jenv controls Maven's Java version.

```bash
jenv enable-plugin maven
source ~/.zshrc
```

### .zshrc — Permanent Fix

```bash
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH
export PATH="$HOME/.jenv/bin:$PATH"
eval "$(jenv init -)"
```

### Verify Setup

```bash
java -version   → 17.0.7 ✅
mvn -version    → 17.0.7 ✅
```

---

## application.properties

Location: `src/main/resources/application.properties`

```properties
# Database connection
spring.datasource.url=jdbc:postgresql://localhost:5432/swiggyx
spring.datasource.username=swiggyx_user
spring.datasource.password=swiggyx123
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA settings
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

# Server port
server.port=8080
```

What each line does:

```
datasource.url      → where PostgreSQL is running
                      localhost = our machine
                      5432 = PostgreSQL default port
                      swiggyx = our database

datasource.username → swiggyx_user (created earlier)
datasource.password → swiggyx123

ddl-auto=update     → JPA auto creates/updates DB tables
                      from our Java classes
                      no manual CREATE TABLE SQL needed

show-sql=true       → prints every SQL query in console
                      helps see what JPA is doing underneath

server.port=8080    → app runs on port 8080
```

---

## Running the App

```bash
cd ~/Desktop/HARDIK/JAVA-AGAIN-2025/Spring-Boot-2026/SwiggyX
mvn clean spring-boot:run
```

Success output:

```
Tomcat started on port 8080 ✅
Started SwiggyXApplication in 1.506 seconds ✅
```

Verify in browser: http://localhost:8080
"Whitelabel Error Page" = server running, no routes yet ✅

---

## Current Status

```
✅ PostgreSQL database created (swiggyx)
✅ User created (swiggyx_user)
✅ Spring Boot project setup
✅ Maven configured with Java 17 permanently
✅ Database connected via application.properties
✅ Server running on port 8080
```

---

## Project Structure (planned)

```
com.swiggyx
├── SwiggyXApplication.java     ← main entry point ✅
├── controller
│   └── OrderController.java    ← receives HTTP requests
├── service
│   └── OrderService.java       ← business logic, concurrency lives here
├── repository
│   └── OrderRepository.java    ← talks to database
├── model
│   └── Order.java              ← Order object, maps to DB table
└── config
    └── ThreadPoolConfig.java   ← thread pools (VT + PT)
```

---

_Document continues as we build..._
_Last updated: Server running successfully on port 8080_

---

## Step 1 — Order Model

### What is a Model?

A Java class that represents one row in a database table.
JPA reads the class and creates the table automatically — no SQL needed.

```
orders table in PostgreSQL:
┌────┬──────────┬───────────────┬────────┬─────────────────────┐
│ id │ user_id  │ restaurant_id │ status │ created_at          │
├────┼──────────┼───────────────┼────────┼─────────────────────┤
│  1 │ USER_101 │ REST_55       │ PLACED │ 2026-06-14 20:00:00 │
│  2 │ USER_202 │ REST_55       │ PLACED │ 2026-06-14 20:00:01 │
└────┴──────────┴───────────────┴────────┴─────────────────────┘

Each row = one Order object in Java
```

### What JPA Does

```
Without JPA — you write SQL:
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id VARCHAR(255),
    ...
);

With JPA — you write a Java class:
JPA reads it → creates the table automatically
No SQL needed
```

### Annotations Explained

```
@Entity              → "This class maps to a DB table"
@Table               → "The table name is orders"
@Id                  → "This field is the primary key"
@GeneratedValue      → "DB auto generates this value (1, 2, 3...)"
@Column              → "This field maps to a column in the table"
@Data                → Lombok: generates getters, setters, toString
@Builder             → Lombok: lets us build objects cleanly
@NoArgsConstructor   → Lombok: generates empty constructor
@AllArgsConstructor  → Lombok: generates constructor with all fields
```

### Order Fields

```
id            → unique identifier for each order
userId        → who placed the order
restaurantId  → which restaurant
itemName      → what they ordered
quantity      → how many
totalAmount   → how much it costs
status        → PLACED, CONFIRMED, DELIVERED, CANCELLED
createdAt     → when was it placed
```

### Note on Virtual Threads

```
Virtual Threads = Java 21 feature
We have Java 17 → cannot use Virtual Threads

Java 17 replacement:
IO work  → CompletableFuture + Fixed Thread Pool (cores × 2)
CPU work → Fixed Thread Pool (= num of cores)

All other concepts fully available:
✅ Race conditions  → synchronized
✅ Semaphore        → DB connection pool
✅ Deadlock         → lock ordering
✅ Starvation       → ReentrantLock(fair)
✅ Async flow       → CompletableFuture
✅ Thread pools     → ExecutorService

Upgrade to Java 21 later → change one line:
Executors.newFixedThreadPool(50)
→ Executors.newVirtualThreadPerTaskExecutor()
Everything else stays the same.
```

---

_Last updated: Order Model — concepts explained, ready to code_

---

## Order Model — Code

Location: `src/main/java/com/swiggyx/model/Order.java`

```java
package com.swiggyx.model;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "orders")
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "user_id", nullable = false)
    private String userId;

    @Column(name = "restaurant_id", nullable = false)
    private String restaurantId;

    @Column(name = "item_name", nullable = false)
    private String itemName;

    @Column(nullable = false)
    private Integer quantity;

    @Column(name = "total_amount", nullable = false)
    private Double totalAmount;

    @Column(nullable = false)
    private String status;

    @Column(name = "created_at")
    private LocalDateTime createdAt;
}
```

### What Each Part Does

**Class level annotations:**

```
@Entity              → tells JPA this class maps to a DB table
@Table(name="orders")→ table name is "orders"
                       (can't use "order" — reserved SQL word)
@Data                → Lombok generates getters, setters,
                       toString, equals, hashCode
@Builder             → lets us create objects cleanly:
                       Order.builder().userId("U_101").build()
@NoArgsConstructor   → empty constructor — JPA needs this
@AllArgsConstructor  → constructor with all fields — Builder needs this
```

**Fields:**

```
@Id                              → primary key
@GeneratedValue(IDENTITY)        → DB auto generates (1, 2, 3...)
id: Long                         → unique order identifier

@Column(name="user_id")
userId: String                   → who placed the order

@Column(name="restaurant_id")
restaurantId: String             → which restaurant

@Column(name="item_name")
itemName: String                 → what they ordered

quantity: Integer                → how many items
totalAmount: Double              → total cost
status: String                   → PLACED/CONFIRMED/DELIVERED/CANCELLED

@Column(name="created_at")
createdAt: LocalDateTime         → when order was placed
```

### JPA Generated SQL

```sql
create table orders (
    id bigserial not null,
    created_at timestamp(6),
    item_name varchar(255) not null,
    quantity integer not null,
    restaurant_id varchar(255) not null,
    status varchar(255) not null,
    total_amount float(53) not null,
    user_id varchar(255) not null,
    primary key (id)
)
```

Zero SQL written by us. JPA generated everything from the Java class. ✅

Verified in PgAdmin:

```
swiggyx database → Schemas → public → Tables → orders ✅
```

---

_Last updated: Order Model complete, table created in PostgreSQL_

---

## Step 2 — Order Repository

Location: `src/main/java/com/swiggyx/repository/OrderRepository.java`

### What is a Repository?

Instead of writing SQL manually:

```sql
INSERT INTO orders (...) VALUES (...)
SELECT * FROM orders WHERE id = 1
SELECT * FROM orders WHERE user_id = 'USER_101'
```

Spring Data JPA generates all SQL automatically.
You just create an interface extending JpaRepository.

### What JpaRepository gives for FREE

```
save(order)        → INSERT into orders table
findById(id)       → SELECT WHERE id = ?
findAll()          → SELECT all orders
delete(order)      → DELETE from orders
count()            → COUNT all orders
existsById(id)     → check if order exists

Zero SQL. Zero implementation. Spring generates everything.
```

### Code

```java
package com.swiggyx.repository;

import com.swiggyx.model.Order;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public interface OrderRepository extends JpaRepository<Order, Long> {

    // Spring generates: SELECT * FROM orders WHERE user_id = ?
    List<Order> findByUserId(String userId);

    // Spring generates: SELECT * FROM orders WHERE restaurant_id = ?
    List<Order> findByRestaurantId(String restaurantId);

    // Spring generates: SELECT * FROM orders WHERE status = ?
    List<Order> findByStatus(String status);
}
```

### Annotations Explained

```
@Repository
→ tells Spring: this is a data access component
→ Spring manages it, handles DB exceptions properly

extends JpaRepository<Order, Long>
→ Order = which entity we work with
→ Long  = type of primary key (our id field)
→ gives us all CRUD operations for free
```

### Magic of Method Naming

```
Spring reads method name → generates SQL automatically

findBy + FieldName    →  WHERE field_name = ?
findByUserId          →  WHERE user_id = ?
findByStatus          →  WHERE status = ?
findByRestaurantId    →  WHERE restaurant_id = ?

Combine fields:
findByUserIdAndStatus →  WHERE user_id = ? AND status = ?
```

### Why List and not ArrayList?

```
List      → interface (defines what operations are possible)
ArrayList → class (one implementation of List)

Returning List:
→ "I give you a collection of orders"
→ Spring chooses best implementation internally
→ flexible, not locked in

Returning ArrayList:
→ "I give you specifically an ArrayList"
→ if Spring returns LinkedList internally → error
→ rigid, locked in

Rule: Always program to the interface, not the implementation.
Use List not ArrayList.
Use Map not HashMap.
Use Set not HashSet.
```

---

_Last updated: Order Repository complete_

---

## Step 3 — Thread Pool Config

Location: `src/main/java/com/swiggyx/config/ThreadPoolConfig.java`

### What is a @Configuration class?

Creates shared objects once at startup.
Makes them available everywhere in the application.
Spring calls all @Bean methods at startup — once.

### Code

```java
package com.swiggyx.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Semaphore;

@Configuration
public class ThreadPoolConfig {

    // Number of CPU cores on this machine
    private static final int CPU_CORES =
        Runtime.getRuntime().availableProcessors();

    // IO Thread Pool — cores × 2
    // threads mostly waiting for IO, can have more than cores
    @Bean(name = "ioThreadPool")
    public ExecutorService ioThreadPool() {
        System.out.println("Creating IO Thread Pool with "
            + (CPU_CORES * 2) + " threads");
        return Executors.newFixedThreadPool(CPU_CORES * 2);
    }

    // CPU Thread Pool — exactly num of cores
    // heavy calculation, cache stays hot, no over-switching
    @Bean(name = "cpuThreadPool")
    public ExecutorService cpuThreadPool() {
        System.out.println("Creating CPU Thread Pool with "
            + CPU_CORES + " threads");
        return Executors.newFixedThreadPool(CPU_CORES);
    }

    // DB Connection Semaphore — max 20 simultaneous DB connections
    @Bean(name = "dbSemaphore")
    public Semaphore dbSemaphore() {
        System.out.println("Creating DB Semaphore with 20 permits");
        return new Semaphore(20);
    }
}
```

### Annotations Explained

```
@Configuration
→ tells Spring: this class creates shared objects
→ Spring runs this at startup
→ all @Bean methods called once

@Bean(name = "ioThreadPool")
→ creates ONE ExecutorService object
→ stored with name "ioThreadPool"
→ any class can ask Spring for it
→ Spring gives SAME object every time (singleton)

Runtime.getRuntime().availableProcessors()
→ asks OS: how many CPU cores?
→ pool sizes calculated dynamically
→ works on any machine automatically
```

### Why @Bean and not just new?

```
Without Spring:
OrderService:   ExecutorService pool = Executors.newFixedThreadPool(8);
PaymentService: ExecutorService pool = Executors.newFixedThreadPool(8);
FraudService:   ExecutorService pool = Executors.newFixedThreadPool(8);
Result: 3 pools, 24 threads, wasted resources

With Spring @Bean:
ONE pool created once, shared everywhere
OrderService, PaymentService, FraudService → same object
Result: 1 pool, 8 threads, efficient
```

### Output on Startup (8-core Mac)

```
Creating IO Thread Pool with 16 threads   (8 cores × 2) ✅
Creating CPU Thread Pool with 8 threads   (= cores) ✅
Creating DB Semaphore with 20 permits     ✅
```

### Concepts Active Here

```
IO Pool (16 threads)  → Concept 4 (Thread Pool)
                         Concept 18 (IO → more threads fine)
CPU Pool (8 threads)  → Concept 18 (CPU → threads = cores)
                         Concept 3 (cache stays hot, no over-switching)
Semaphore (20)        → Concept 8 (DB connection limiting)
```

---

_Last updated: Thread Pool Config complete, all pools running_

---

## Step 4 — Order Service

Location: `src/main/java/com/swiggyx/service/OrderService.java`

### What is a Service?

```
Controller  → receives HTTP request → calls Service
Service     → ALL business logic lives here ← this is it
Repository  → talks to DB
```

### What OrderService does — Full Flow

```
Step 1 — Validate order        → CPU work, instant, no locks
Step 2 — Fraud detection       → CPU intensive, runs on CPU pool in background
Step 3 — Check inventory       → synchronized with ReentrantLock(fair)
Step 4 — Wait for fraud result → fraudFuture.get()
Step 5 — Save to DB            → Semaphore controls max 20 connections
Step 6 — Charge payment        → ReentrantLock(fair), lock ordering
Step 7 — Send notification     → fire and forget, background
```

### Why ReentrantLock instead of synchronized?

```
synchronized:
→ simpler to write
→ no fairness guarantee
→ threads compete randomly → starvation possible

ReentrantLock(true):
→ fair = true → strict FIFO ordering
→ threads served in order they arrived
→ no starvation (Concept 10)
→ requires manual lock/unlock in try/finally
```

### Lock Ordering — Deadlock Prevention

```
Always acquire in this order:
1. inventoryLock FIRST
2. paymentLock SECOND

Never reversed. Deadlock impossible.
(Concept 9 — circular wait prevented)
```

### Lombok Fix

```
IntelliJ Community Edition doesn't show Lombok generated methods.
Fix: Install Lombok plugin
Preferences → Plugins → Marketplace → search "Lombok" → Install → Restart
```

### Complete Code

```java
package com.swiggyx.service;

import com.swiggyx.model.Order;
import com.swiggyx.repository.OrderRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Service;

import java.time.LocalDateTime;
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Semaphore;
import java.util.concurrent.locks.ReentrantLock;

@Service
public class OrderService {

    // Lock for inventory — always acquire FIRST (lock ordering)
    private final ReentrantLock inventoryLock = new ReentrantLock(true);

    // Lock for payment — always acquire SECOND (lock ordering)
    private final ReentrantLock paymentLock = new ReentrantLock(true);

    // Spring injects automatically
    @Autowired
    private OrderRepository orderRepository;

    @Autowired
    @Qualifier("ioThreadPool")
    private ExecutorService ioThreadPool;

    @Autowired
    @Qualifier("cpuThreadPool")
    private ExecutorService cpuThreadPool;

    @Autowired
    @Qualifier("dbSemaphore")
    private Semaphore dbSemaphore;

    // Shared inventory — race condition risk
    // ALL threads can read/write this → needs protection
    private int inventory = 10;

    // Step 1 — Validate order
    // Parameters live on STACK → thread private → no lock needed
    private boolean validateOrder(String userId,
                                  String itemName,
                                  int quantity,
                                  double amount) {
        if (userId == null || userId.isEmpty()) {
            System.out.println("Invalid userId");
            return false;
        }
        if (itemName == null || itemName.isEmpty()) {
            System.out.println("Invalid itemName");
            return false;
        }
        if (quantity <= 0) {
            System.out.println("Invalid quantity");
            return false;
        }
        if (amount <= 0) {
            System.out.println("Invalid amount");
            return false;
        }
        return true;
    }

    // Step 2 — Fraud Detection
    // CPU intensive — runs on CPU thread pool
    // Returns CompletableFuture immediately — runs in background
    private CompletableFuture<Boolean> checkFraud(String userId,
                                                   double amount) {
        return CompletableFuture.supplyAsync(() -> {
            System.out.println("Fraud check started for: " + userId
                + " on thread: " + Thread.currentThread().getName());

            try {
                Thread.sleep(200); // simulate ML model — 200ms CPU work
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }

            boolean isFraud = amount > 10000; // simple rule: >10000 = fraud

            System.out.println("Fraud check done for: " + userId
                + " isFraud: " + isFraud);

            return isFraud;

        }, cpuThreadPool); // runs on CPU pool — NOT IO pool
    }

    // Step 3 — Check and Deduct Inventory
    // CRITICAL SECTION — race condition protection
    // ReentrantLock(fair) — no starvation
    // inventoryLock acquired FIRST — lock ordering
    private boolean checkAndDeductInventory(String userId, int quantity) {
        inventoryLock.lock();
        try {
            System.out.println("Inventory check for: " + userId
                + " | Current: " + inventory
                + " | Thread: " + Thread.currentThread().getName());

            if (inventory < quantity) {
                System.out.println("Insufficient inventory for: " + userId);
                return false;
            }

            inventory = inventory - quantity; // protected LOAD-MODIFY-STORE
            System.out.println("Inventory deducted for: " + userId
                + " | Remaining: " + inventory);
            return true;

        } finally {
            inventoryLock.unlock(); // ALWAYS unlock in finally
        }
    }

    // Step 4 — Save Order to DB
    // Semaphore controls max 20 simultaneous DB connections
    private Order saveOrderToDB(String userId,
                                String restaurantId,
                                String itemName,
                                int quantity,
                                double amount) {
        try {
            dbSemaphore.acquire(); // blocks if 20 threads already in DB
            System.out.println("DB connection acquired for: " + userId
                + " | Thread: " + Thread.currentThread().getName());

            Order order = Order.builder()
                    .userId(userId)
                    .restaurantId(restaurantId)
                    .itemName(itemName)
                    .quantity(quantity)
                    .totalAmount(amount)
                    .status("PLACED")
                    .createdAt(LocalDateTime.now())
                    .build();

            Order savedOrder = orderRepository.save(order);
            System.out.println("Order saved: " + savedOrder.getId()
                + " for: " + userId);

            return savedOrder;

        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            throw new RuntimeException("Interrupted waiting for DB connection");
        } finally {
            dbSemaphore.release(); // ALWAYS release in finally
            System.out.println("DB connection released for: " + userId);
        }
    }

    // Main method — connects all steps
    // Called by Controller
    public CompletableFuture<String> processOrder(String userId,
                                                   String restaurantId,
                                                   String itemName,
                                                   int quantity,
                                                   double amount) {

        return CompletableFuture.supplyAsync(() -> {

            System.out.println("Order started for: " + userId
                + " | Thread: " + Thread.currentThread().getName());

            // Step 1 — Validate
            if (!validateOrder(userId, itemName, quantity, amount)) {
                return "FAILED: Invalid order details";
            }

            // Step 2 — Start fraud check in background (CPU pool)
            CompletableFuture<Boolean> fraudFuture =
                checkFraud(userId, amount);

            // Step 3 — Check inventory (inventoryLock — FIRST lock)
            boolean inventoryAvailable =
                checkAndDeductInventory(userId, quantity);

            if (!inventoryAvailable) {
                return "FAILED: Item not available";
            }

            // Step 4 — Get fraud result (fraud ran in parallel with inventory check)
            try {
                boolean isFraud = fraudFuture.get();
                if (isFraud) {
                    // Restore inventory — order blocked
                    inventoryLock.lock();
                    try {
                        inventory = inventory + quantity;
                        System.out.println("Inventory restored — fraud: " + userId);
                    } finally {
                        inventoryLock.unlock();
                    }
                    return "FAILED: Fraud detected";
                }
            } catch (Exception e) {
                return "FAILED: Fraud check error";
            }

            // Step 5 — Save to DB (Semaphore limits to 20 connections)
            Order savedOrder = saveOrderToDB(userId, restaurantId,
                itemName, quantity, amount);

            // Step 6 — Payment (paymentLock — SECOND lock, always after inventoryLock)
            paymentLock.lock();
            try {
                System.out.println("Payment processing for: " + userId
                    + " | Amount: " + amount);
                Thread.sleep(30); // simulate payment gateway
                System.out.println("Payment done for: " + userId);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                paymentLock.unlock();
            }

            // Step 7 — Notify (fire and forget — user doesn't wait)
            CompletableFuture.runAsync(() -> {
                System.out.println("Notification sent for order: "
                    + savedOrder.getId());
            }, ioThreadPool);

            return "SUCCESS: Order placed. ID: " + savedOrder.getId();

        }, ioThreadPool); // entire flow runs on IO thread pool
    }
}
```

### Concepts Active in OrderService

```
Concept 5  → Race condition: inventory protected by ReentrantLock
Concept 7  → Mutex: ReentrantLock(fair) on inventory and payment
Concept 8  → Semaphore: dbSemaphore(20) limits DB connections
Concept 9  → Deadlock: inventoryLock always before paymentLock
Concept 10 → Starvation: ReentrantLock(true) = fair = FIFO ordering
Concept 12 → Futures: CompletableFuture for fraud check
Concept 13 → Async: fraud check runs in background while inventory checked
Concept 18 → Threads vs Async: IO pool for order flow, CPU pool for fraud
```

### Parallel Execution Timeline

```
Time 0ms:   processOrder() starts on IO thread pool
Time 1ms:   validateOrder() — instant
Time 2ms:   checkFraud() STARTS on CPU pool (background)
Time 3ms:   checkAndDeductInventory() — instant (with lock)
Time 203ms: fraudFuture.get() — fraud check done by now
Time 223ms: saveOrderToDB() — DB call with semaphore
Time 253ms: paymentLock acquired — payment processed
Time 253ms: notification fired (background, fire and forget)
Time 253ms: return SUCCESS
```

### App Startup Output

```
Creating IO Thread Pool with 16 threads   (8 cores × 2) ✅
Creating CPU Thread Pool with 8 threads   (= cores) ✅
Creating DB Semaphore with 20 permits     ✅
Started SwiggyXApplication in 1.953s     ✅
HikariPool connected to PostgreSQL        ✅
```
