# CARE Backend - Complete Onboarding Guide
**For developers new to Java/Spring Boot (especially from .NET background)**

---

## Table of Contents
1. [Project Setup in IntelliJ](#project-setup-in-intellij)
2. [Folder Structure Breakdown](#folder-structure-breakdown)
3. [Understanding Domain Packages](#understanding-domain-packages)
4. [Architecture Overview](#architecture-overview)
5. [Technology Stack](#technology-stack)
6. [Request Flow Example](#request-flow-example)
7. [Next Steps](#next-steps)

---

## Project Setup in IntelliJ

### Cloning the Repository
1. Open IntelliJ IDEA
2. Click **"Get from VCS"** (or File → New → Project from Version Control)
3. Enter repository URL: `https://stash.tre.se/scm/care/care.git`
4. Choose directory: `C:\Users\[username]\Desktop\CARE`
5. Click **Clone**
6. When prompted "Trust this project?" → Click **Trust Project**
7. Wait for indexing to complete (5-10 minutes first time)

### What Happens After Clone
- IntelliJ downloads all code
- Gradle syncs dependencies (like NuGet restore in .NET)
- Project structure is indexed
- You may see Gradle JVM errors initially - **ignore for now** (won't run app yet, just reading code)

---

## Folder Structure Breakdown

### Root Level Structure

The root folder contains:
- **backend/** - Main backend code (your focus)
- **build.gradle** - Root build configuration (like .sln file)
- **settings.gradle** - Module configuration
- **README.md** - Project documentation (read first!)
- **docker/** - Docker configurations
- **frontend-new/** - Frontend code (ignore for now)
- **Jenkinsfile** - CI/CD pipeline config
- Other configuration files for Gradle, Docker, and CI/CD

### Backend Folder Structure

The backend folder contains:
- **src/main/** - Main application code
  - **java/** - All Java source code
  - **resources/** - Configuration files (application.yml, etc.)
  - **schema/** - Database schema files
- **src/test/** - Unit tests
- **src/functional-test/** - Integration/E2E tests
- **src/clients/** - API client code for external services
- **build.gradle** - Backend build configuration (like .csproj)
- **build/** - Compiled output (like bin/ in .NET) - ignore

### Main/Java Package Structure

The main Java code is organized in `src/main/java/se/tre/willow/` with these business domains:

- **account/** - Customer account management domain
- **acssp/** - ACSSP integration domain
- **admin.api/** - Admin API endpoints
- **agentlogin/** - Agent login functionality
- **billingprovisioning/** - Billing provisioning domain
- **cases/** - Case management domain
- **common/** - Shared utilities, base classes
- **communication/** - Messaging/notifications domain
- **configuration/** - App configuration, Spring config
- **consents/** - Customer consent management
- **customer/** - Customer data domain
- **dataavailability/** - Data availability checks
- **device/** - Device management (phones, routers)
- **document/** - Document handling
- **documentmanagement/** - Document management system integration
- **environment/** - Environment settings
- **extract/** - Data extraction domain
- **featuretoggles/** - Feature toggle management
- **gson/** - JSON serialization config
- **interaction/** - Customer interaction tracking
- **invoice/** - Billing/invoicing domain
- **my3login/** - My3 login functionality
- **orders/** - Order processing domain
- **portfoliomanagement/** - Portfolio management
- **product/** - Product catalog domain
- **provision/** - Provisioning domain
- **reservemisdn/** - Phone number reservation
- **rootcause/** - Root cause analysis
- **sales/** - Sales domain
- **savecause/** - Save cause tracking
- **security/** - Authentication, authorization
- **sendinvitation/** - Invitation sending
- **speed/** - Network speed testing (3Speed integration)
- **stores/** - Store management
- **usage/** - Usage tracking/analytics
- **user/** - User management
- **WillowApplication.java** - ENTRY POINT (Main class)

### Resources Folder Structure

The resources folder contains:
- **application.yml** - Main configuration (like appsettings.json)
- **application-dev.yml** - Development environment config
- **application-prod.yml** - Production environment config
- **db/migration/** - Database migration scripts (Flyway/Liquibase)
- **static/** - Static resources
- **templates/** - Template files

---

## Understanding Domain Packages

### What is a "Domain"?

A **domain** = a business area or feature area of your application.

Think of your application like a company with different departments:
- Accounting Department → `invoice/` domain
- Customer Service → `customer/` domain  
- Sales Department → `sales/` domain
- Order Processing → `orders/` domain

Each department handles its own **domain** (area of responsibility).

### Domain Package Structure

Each domain package typically contains:
- **Controller** files - API endpoints (e.g., CustomerController.java)
- **Service** files - Business logic (e.g., CustomerService.java)
- **Repository** files - Data access (e.g., CustomerRepository.java)
- **Entity** files - Database models (e.g., Customer.java)
- **dto/** folder - Data Transfer Objects (Request/Response classes)
- **exception/** folder - Domain-specific exceptions

### .NET Comparison

**Java/Spring Domain Organization:**
All related files grouped in one domain folder (e.g., se.tre.willow.customer/ contains CustomerController, CustomerService, CustomerRepository, Customer)

**Equivalent .NET Organization:**
Similar pattern (e.g., MyApp.Customer/ contains CustomerController.cs, CustomerService.cs, CustomerRepository.cs, Customer.cs)

### Why Organize by Domain?

**BAD (Technical organization):**
Organizing by file type - all Controllers in one folder, all Services in another folder, all Repositories in third folder

**GOOD (Domain organization):**
Organizing by business domain - all customer-related files in customer/ folder, all order-related files in orders/ folder

**Benefits:**
- Easy to find everything related to one feature
- Teams can work on different domains without conflicts
- Each domain is somewhat independent
- Easier to understand business logic
- Aligns with business structure

---

## Architecture Overview

### 1. High-Level System Architecture

**Components:**
- **Browser** - User interface
- **Proxy (care-proxy)** - Traffic cop that routes requests
- **Frontend** - UI application
- **Backend (CARE)** - Main application
- **Database** - Data storage (H2 local / real DB in production)
- **Other APIs** - External systems

**Request Flow:**
1. User clicks button in browser
2. Frontend calls backend API
3. Request goes through Proxy
4. Backend processes request (business logic)
5. Backend talks to database or external systems
6. Backend returns response
7. Frontend shows result to user

### 2. Backend Internal Architecture (Layered)

**Architecture Layers (Top to Bottom):**

1. **CONTROLLERS (API Layer)** - Receives HTTP requests
2. **SERVICES (Business Logic)** - Business rules, decisions
3. **REPOSITORIES (Data Access Layer)** - Database queries
4. **DATABASE (H2/SQL)** - Actual data storage

**Layer Responsibilities:****

| Layer | What it does | .NET Equivalent |
|---|---|---|
| **Controller** | Receives HTTP requests, validates input, returns JSON | ASP.NET Web API Controller with `[ApiController]` |
| **Service** | Business logic, orchestration, transactions | Application/Business Service class |
| **Repository** | Database queries, CRUD operations | EF Core Repository / DbContext |
| **Entity** | Data model, represents DB table | EF Core Model/Entity class |

### 3. Architecture Patterns Used

1. **Layered Architecture** - Clear separation: Controller → Service → Repository
2. **Domain-Driven Design (DDD)** - Organized by business domains, not technical layers
3. **Dependency Injection** - Spring manages object creation and wiring
4. **RESTful APIs** - HTTP endpoints following REST principles
5. **BFF Pattern** - Backend For Frontend - thin translation layer

### 4. What Makes CARE Backend "Thin"?

From the README, the backend is designed to be **thin** (lightweight):

**What it DOES:**
- ✅ Translate data formats between systems
- ✅ Transform/reshape data for frontend needs
- ✅ Expose REST APIs
- ✅ Temporarily cache data during request
- ✅ Route requests to external systems

**What it DOES NOT do:**
- ❌ Own complex business logic (lives in other systems)
- ❌ Store data permanently (mostly transient/temporary)
- ❌ Make heavy business decisions
- ❌ Be the source of truth for data

**Why thin?**
Real business logic and data ownership live in external systems. CARE backend is the **middleman/translator** - like a waiter in a restaurant (doesn't cook, just carries food and translates orders).

---

## Technology Stack

### Java/Spring Boot vs .NET Comparison

| Component | Java/Spring | .NET Equivalent |
|---|---|---|
| **Framework** | Spring Boot 3.x | ASP.NET Core 6+ |
| **Language** | Java 17+ | C# 10+ |
| **Build Tool** | Gradle | MSBuild / dotnet CLI |
| **Package Manager** | Gradle dependencies | NuGet |
| **Dependency Injection** | Spring IoC Container | Built-in DI Container |
| **ORM** | JPA (Hibernate) | Entity Framework Core |
| **Database (local dev)** | H2 in-memory | SQL Server LocalDB / SQLite |
| **Configuration** | application.yml | appsettings.json |
| **REST APIs** | @RestController | [ApiController] |
| **Web Server** | Embedded Tomcat | Kestrel |
| **Testing** | JUnit 5 + Mockito | xUnit/NUnit + Moq |
| **Logging** | SLF4J + Logback | ILogger / Serilog |

### Key Spring Boot Annotations (vs .NET Attributes)

| Spring Annotation | Purpose | .NET Equivalent |
|---|---|---|
| `@SpringBootApplication` | Main entry point | `[Assembly]` + Program.cs |
| `@RestController` | REST API controller | `[ApiController]` |
| `@Service` | Business logic service | No direct equivalent (convention) |
| `@Repository` | Data access layer | No direct equivalent (convention) |
| `@Entity` | JPA entity/model | EF Core entity class |
| `@Autowired` | Dependency injection | Constructor injection (best practice in both) |
| `@GetMapping` | HTTP GET endpoint | `[HttpGet]` |
| `@PostMapping` | HTTP POST endpoint | `[HttpPost]` |
| `@RequestBody` | Bind JSON to object | `[FromBody]` |
| `@PathVariable` | URL parameter | `[FromRoute]` |
| `@RequestParam` | Query parameter | `[FromQuery]` |

### Build Configuration Files

| File | Purpose | .NET Equivalent |
|---|---|---|
| `build.gradle` (root) | Solution-level config | `.sln` file |
| `build.gradle` (backend) | Project-level dependencies | `.csproj` file |
| `settings.gradle` | Multi-module configuration | Solution structure |
| `gradlew` / `gradlew.bat` | Gradle wrapper | `dotnet` CLI |

---

## Request Flow Example

### Scenario: User wants to view customer profile

**Request Flow:**

1. **User clicks "View Profile"** - Initiates the request
2. **Browser calls API** - `GET https://localhost:8186/willow/api/customer/12345`
3. **Proxy routes request** - care-proxy forwards to backend
4. **DispatcherServlet receives** - Spring's front controller
5. **Routes to Controller** - Matches URL to endpoint
6. **Controller processes** - Calls service layer
7. **Service executes logic** - Orchestrates business operations
8. **Repository queries data** - Uses JPA interface
9. **Hibernate generates SQL** - Creates database query
10. **Database returns data** - Row data retrieved
11. **JPA maps to Entity** - Converts to Customer object
12. **Repository returns entity** - Passes back to service
13. **Service transforms to DTO** - Maps to CustomerResponse
14. **Controller returns response** - Sends DTO back
15. **Spring converts to JSON** - Automatic serialization
16. **Response sent to browser** - Via proxy
17. **User sees profile** - Frontend displays data

**Explanation:**

The code follows this pattern:
-  **Controller Layer** receives HTTP request and calls the service
- **Service Layer** contains business logic, calls repository, and transforms data
- **Repository Layer** provides data access interface (JPA)

**It's the SAME pattern in .NET!** Just different syntax.

---

## Configuration & Startup

### Understanding WillowApplication.java - The Entry Point

**Location:** `backend/src/main/java/se/tre/willow/WillowApplication.java`

This is the **"ON switch"** for the entire application - like `Program.cs` in .NET!

#### Key Annotations Explained

**@SpringBootApplication - THE MOST IMPORTANT!**

**This ONE annotation does EVERYTHING:****

1. **Marks this as the entry point**
2. **Auto-configures Spring** (web server, database, JSON converters, etc.)
3. **Scans the `se.tre.willow` package** to find all your:
   - Controllers (REST API endpoints)
   - Services (business logic)
   - Repositories (database access)
   - And wires them together automatically!

**Think of it as a robot that:**
- Walks through all 30+ domain folders
- Finds every class with `@Controller`, `@Service`, `@Repository`
- Creates instances and connects them via dependency injection
- All automatically - you don't write any wiring code!

**.NET Equivalent:** Similar to `WebApplication.CreateBuilder(args)` and `builder.Services.AddControllers()` for auto-setup

**@Configuration**
- **What it means:** This class contains Spring configuration
- **.NET equivalent:** Setting up services in `Program.cs`

**@EnableCaching**
- **What it means:** Turns on caching support
- **Use case:** Store frequently-used data in memory for faster access
- **.NET equivalent:** `builder.Services.AddMemoryCache()`

**@EnableScheduling**
- **What it means:** Allows scheduled background tasks
- **Use case:** Run cleanup jobs every night, send reports every Monday
- **.NET equivalent:** `IHostedService` or `BackgroundService`

**@EnableAspectJAutoProxy**
- **What it means:** Enables AOP (Aspect-Oriented Programming)
- **Use case:** Add logging, security, or transactions automatically to methods
- **.NET equivalent:** Middleware or ActionFilters

**@EnableAsync**
- **What it means:** Allows asynchronous method execution
- **Use case:** Run long tasks in background without blocking
- **.NET equivalent:** `async/await` methods

**@ConfigurationPropertiesScan**
- **What it means:** Scans for configuration classes that read from `application.yml`
- **.NET equivalent:** `IOptions<T>` pattern for reading `appsettings.json`

**The Main Method**
- **What it means:** The entry point - where Java starts execution
- **.NET equivalent:** `static void Main(string[] args)` in Program.cs
- **Why:** Every Java application needs a `main()` method to run

---

**SpringApplication.run()**

The main method calls `SpringApplication.run()` which starts everything!

**What happens when this runs:**

1. Spring Boot initialization begins
2. Scans se.tre.willow package
3. Finds all @Controller, @Service, @Repository classes
4. Creates bean instances (objects)
5. Wires dependencies (dependency injection)
6. Loads application.yml configuration
7. Connects to database (H2)
8. Starts embedded Tomcat web server on port 8128
9. Registers all REST API endpoints
10. Application ready - can now accept HTTP requests!

**.NET Equivalent:** Similar to `var app = builder.Build(); app.Run();`

**Error Handling:**

If the app fails to start, errors are printed to console to help with debugging startup problems. This is equivalent to catching exceptions in .NET.

---

### What Happens After WillowApplication Starts?

**Imagine Spring Boot as a detective:**

1. **Detective enters building** (`se.tre.willow` package)
2. **Opens every room** (account/, customer/, orders/, invoice/, etc.)
3. **Looks for people with badges:**
   - 🎯 `@RestController` badge → "You handle HTTP requests!"
   - 🔧 `@Service` badge → "You contain business logic!"
   - 💾 `@Repository` badge → "You talk to database!"
4. **Introduces everyone to each other** (dependency injection)
5. **Starts the phone system** (Tomcat web server)
6. **Now customers can call in!** (HTTP requests can be handled)

**Example of what Spring finds:**

When Spring scans the customer/ folder:
- Finds CustomerController (has @RestController)
- Finds CustomerService (has @Service)
- Finds CustomerRepository (has @Repository)

Spring then:
- Creates 1 instance of each
- Wires them together (Controller gets Service, Service gets Repository)
- Registers endpoint: GET /willow/api/customer/{id}

This happens **automatically** for all 30+ domains!

---

### Comparison: Java vs .NET

| What it does | Java (WillowApplication) | .NET (Program.cs) |
|---|---|---|
| **Entry point** | `main()` method | `Main()` method or top-level statements |
| **Start framework** | `SpringApplication.run()` | `app.Run()` |
| **Scan for components** | `@SpringBootApplication` | `builder.Services.AddControllers()` |
| **Enable caching** | `@EnableCaching` | `builder.Services.AddMemoryCache()` |
| **Enable async** | `@EnableAsync` | Built-in with `async/await` |
| **Load configuration** | Reads `application.yml` | Reads `appsettings.json` |
| **Start web server** | Tomcat (embedded) | Kestrel (embedded) |
| **Default port** | 8080 (or configured) | 5000 (or configured) |

---

### Quick Facts

✅ **You only have ONE WillowApplication file** - it's the single entry point  
✅ **You never need to edit this file** - it's already configured  
✅ **All the magic happens automatically** - component scanning, wiring, startup  
✅ **It's like Program.cs in .NET** - same purpose, different syntax  
✅ **Without this file, nothing runs!** - It's the "ON switch"

---

### Summary in 3 Sentences

1. **WillowApplication.java is the entry point** - like Program.cs in .NET
2. **The `@SpringBootApplication` annotation scans all folders** for Controllers/Services/Repositories and wires them together
3. **`SpringApplication.run()` starts the web server** and makes your APIs available

**That's it!** Once this runs, your entire application with all 30+ domains is up and running! 🚀

---

## Key Architectural Concepts

### 1. Dependency Injection (DI)

**Spring Boot:** Uses constructor injection where dependencies are passed into the class constructor. The service class receives the repository dependency automatically.

**.NET:** Same pattern - uses constructor injection where dependencies are passed into the class constructor.

**Both use constructor injection (best practice).**

### 2. Configuration Management

**Spring Boot:** Uses `application.yml` files for configuration including server port, database connection, logging levels, etc.

**.NET:** Uses `appsettings.json` for configuration including Kestrel endpoints, connection strings, logging levels, etc.

**Same purpose, different format.**

### 3. REST API Design

**Typical CARE endpoint pattern:**

Controllers expose REST endpoints with standard HTTP methods:
- GET (without ID) - Retrieve all items
- GET (with ID) - Retrieve specific item by ID 
- POST - Create new item
- PUT (with ID) - Update existing item by ID
- DELETE (with ID) - Delete item by ID

**Same RESTful conventions as .NET Web API.**

---

## Important Files Reference

### Must-Read Files (in order)

1. **README.md** (root) - How to run, architecture overview
2. **build.gradle** (root) - Solution-level dependencies
3. **settings.gradle** - Module structure
4. **backend/build.gradle** - Backend dependencies and versions
5. **backend/src/main/resources/application.yml** - Main configuration
6. **WillowApplication.java** - Entry point

### Configuration Files

| File | Purpose |
|---|---|
| `application.yml` | Main config (DB, server, logging) |
| `application-dev.yml` | Development overrides |
| `application-prod.yml` | Production overrides |
| `gradle.properties` | Gradle settings |
| `build.gradle` | Dependencies, versions, plugins |

### Infrastructure Files

| File | Purpose |
|---|---|
| `Dockerfile` | Docker image build instructions |
| `docker-compose.yml` | Multi-container orchestration |
| `Jenkinsfile` | CI/CD pipeline definition |
| `startApp.sh` | Quick start script (runs all services) |

---

## Domain Packages Quick Reference

| Package | Business Domain | Key Responsibilities |
|---|---|---|
| `customer` | Customer data | Customer profiles, details, management |
| `orders` | Order processing | Order creation, tracking, history |
| `invoice` | Billing | Invoice generation, payment tracking |
| `product` | Product catalog | Products/services offered |
| `device` | Device management | Customer devices (phones, routers, modems) |
| `communication` | Messaging | SMS, email, push notifications |
| `featuretoggles` | Feature toggles | ON/OFF switches for features |
| `security` | Authentication | Login, permissions, authorization |
| `account` | Account management | Account creation, updates |
| `sales` | Sales | Sales processes, campaigns |
| `acssp` | ACSSP integration | Third-party system integration |
| `interaction` | Interactions | Customer interaction tracking |
| `document` | Documents | Document handling |
| `speed` | Speed testing | Network speed tests (3Speed) |
| `common` | Shared utilities | Reusable code, base classes |
| `configuration` | App config | Spring configuration, beans |

---

## Next Steps - Learning Path

### Day 1: Entry Point & Configuration
1. Open `WillowApplication.java` - understand entry point
2. Read `application.yml` - see configuration
3. Explore `common/` package - shared utilities

### Day 2: Pick One Domain End-to-End
1. Choose simple domain (e.g., `featuretoggles`)
2. Find the Controller → trace to Service → Repository → Entity
3. Document the flow in your own words

### Day 3: Compare Another Domain
1. Pick different domain (e.g., `customer`)
2. Trace same pattern
3. Notice what's similar vs different

### Day 4-5: Understand Configuration & Security
1. Explore `configuration/` package
2. Understand Spring beans and DI
3. Look at `security/` for auth patterns

### Week 2: Deep Dive
1. Understand database migrations
2. Learn integration patterns with external systems
3. Study error handling and exceptions

---

## Common Spring Boot Patterns You'll See

### 1. Constructor Injection (Preferred)
Dependencies are injected through the class constructor. The repository is passed as a constructor parameter and stored as a final field.

### 2. Optional Handling
When fetching data that might not exist, use `.orElseThrow()` to handle the case where an item is not found, throwing a custom exception.

### 3. DTO Mapping
Convert entity objects to response DTOs using a mapping method. This separates internal models from API responses.

### 4. Exception Handling
Use global exception handlers to centralize error handling logic. Custom exceptions can be caught and converted to appropriate HTTP responses.

---

## Troubleshooting Common Issues

### Gradle Errors
- **Issue:** "Invalid Gradle JVM configuration"
- **Fix:** File → Settings → Build Tools → Gradle → Set Gradle JVM to JDK 17+

### Cannot Find Symbols
- **Issue:** Red underlines, "cannot find symbol"
- **Fix:** File → Invalidate Caches → Invalidate and Restart

### Application Won't Start
- **Issue:** Port already in use
- **Fix:** Check `application.yml` for port, or stop other processes using that port

---

## Glossary - Java/Spring Terms

| Java/Spring Term | Meaning | .NET Equivalent |
|---|---|---|
| **Bean** | Object managed by Spring container | Service registered in DI container |
| **Annotation** | Metadata decorator (@Something) | Attribute ([Something]) |
| **JPA** | Java Persistence API (ORM standard) | Entity Framework |
| **Hibernate** | JPA implementation | EF Core (implementation) |
| **Repository** | Data access interface | Repository pattern |
| **Entity** | Database table model | Entity/Model class |
| **DTO** | Data Transfer Object | DTO / ViewModel |
| **Autowired** | Dependency injection | Constructor injection |
| **Component Scan** | Auto-discovery of beans | Service registration |
| **Application Context** | Spring DI container | IServiceProvider |

---

## Resources for Learning

### Java Basics (if new to Java)
- Focus on: classes, interfaces, packages, generics, lambda expressions
- Skip: Swing, AWT (old GUI frameworks)

### Spring Boot Essentials
- Dependency Injection
- @RestController and REST APIs
- JPA and database access
- Configuration with application.yml
- Exception handling

### Tools
- IntelliJ IDEA (IDE)
- Postman (API testing)
- H2 Console (database viewing - http://localhost:8128/h2-console/)

---

**Document created:** March 2, 2026  
**For:** New developers joining CARE backend team  
**Based on:** CARE repository structure and README.md

---


