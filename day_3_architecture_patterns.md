# Day 3 – Layered, Hexagonal, Onion, and Clean Architectures

**Series:** Learn Software Architecture (Day 3 of N)

**Theme:** Organizing code for clarity, testability, and long-term maintenance.

---

## 🏡 Why Application Architecture Matters

System-level architecture (like monolith vs. microservices) tells you *how to deploy*. Application architecture tells you *how to think*. It defines how you structure code *inside* your modules or services.

A good application architecture:

- Improves **testability**
- Reduces **technical debt**
- Supports **separation of concerns**
- Adapts better to **change** (infra, business, team)

---

## 🔢 1. Layered Architecture (aka N-tier)

**Most common pattern**, especially in traditional enterprise apps.

### Structure:

```
Presentation Layer (Controllers/UI)
👇
Application Layer (Services, DTOs)
👇
Persistence Layer (Repositories, ORM)
👇
Database (SQL/NoSQL)
```

### Pros:

- Simple and widely understood
- Fast to develop CRUD apps

### Cons:

- Logic leaks across layers (tight coupling)
- Hard to test business rules in isolation
- Infra details (ORM, HTTP) often seep into core logic

**Best for:** Simple business flows, small teams, early-stage apps.

### Step-by-Step Development Approach:

1. Define entities and database schema.
2. Create repository interfaces and implementations.
3. Implement service layer logic.
4. Build controller or UI layer to interact with services.
5. Add tests for services and repositories.

---

## 🔍 2. Hexagonal Architecture (Ports and Adapters)

**Introduced by Alistair Cockburn**. Focuses on isolating business logic from delivery mechanisms (UI, database, APIs).

### Key Concept:

- Business logic talks to **ports** (interfaces)
- Infra components are **adapters** plugged into ports

```
        UI   DB   External APIs
         |     |      |
     [Adapters on outer ring]
             ⇓  
       [Ports/Interfaces]
             ⇓
       [Application Core]
```

### Pros:

- Business logic testable independently
- Easy to swap out infra (change DB, replace API client)

### Cons:

- Requires interface-driven design
- Slightly more boilerplate

**Best for:** Domain-centric systems, testing-focused teams.

### Step-by-Step Development Approach:

1. Identify core business logic and create interfaces (ports).
2. Develop application core using ports.
3. Implement adapters (DB, HTTP, external APIs) that fulfill the port contracts.
4. Wire everything together using a dependency injection framework.
5. Write isolated tests for the application core.

---

## 🏋️ 3. Onion Architecture

**Created by Jeffrey Palermo**, this extends the ideas of hexagonal with **concentric layers**.

### Layers (inward dependencies only):

```
[Entities / Domain Models]     <- center
[Use Cases / Application Logic]
[Interface Adapters]
[Frameworks & Drivers]        <- outer ring
```

- Outer layers depend on inner layers
- Infra can be swapped without touching core
- Domain is king; everything else serves it

### Pros:

- Enforces strong separation
- Technology-agnostic design

### Cons:

- Complexity may not be justified for CRUD apps
- Harder onboarding for junior devs

**Best for:** Complex domains, DDD (Domain-Driven Design), long-lived systems.

### Step-by-Step Development Approach:

1. Define domain models and core business logic in the center layer.
2. Implement use cases as services or application logic.
3. Create interface adapters (controllers, presenters, gateways).
4. Add infrastructure code and frameworks in the outermost layer.
5. Use dependency injection to connect inner and outer layers.

---

## 🧪 4. Clean Architecture

**Popularized by Robert C. Martin (Uncle Bob)**. Seen as an evolution of Hexagonal and Onion architectures.

### Core Ideas:

- Code is organized in concentric circles
- Inner circles contain business rules (Entities, Use Cases)
- Outer circles contain delivery mechanisms (UI, DB, Frameworks)
- **Dependency Rule:** Code dependencies always point inward. Outer layers depend on inner layers—not vice versa.

### Layers:

```
[Entities]           <- core domain models
[Use Cases]          <- application-specific rules
[Interface Adapters] <- controllers, presenters, gateways
[Frameworks]         <- DB, UI, external APIs
```

### Detailed Breakdown:

- **Entities:** Enterprise-wide business rules. These are the most stable parts of the system.
- **Use Cases:** Application-specific logic. Coordinates flow of data between UI and entities.
- **Interface Adapters:** Controllers, presenters, and gateways. Convert external inputs/outputs into formats the use cases can use.
- **Frameworks & Drivers:** External tools like DBs, UI frameworks, email services.

### Benefits:

- Highly testable: Business logic can be tested without UI or DB.
- Long-term maintainability: Frameworks can change, core logic remains.
- Encourages SOLID principles, especially Dependency Inversion.

### Trade-offs:

- Steeper learning curve for teams unfamiliar with the model.
- May introduce over-engineering for small or short-lived projects.

**Best for:** Enterprise-grade systems, large teams, business-critical domains with changing tech stacks.

### Step-by-Step Development Approach:

1. Define high-level entities that reflect core business rules.
2. Identify and implement use cases that coordinate between entities.
3. Create interfaces for data access, external APIs, and UI.
4. Build concrete adapters to implement these interfaces.
5. Place frameworks (Spring Boot, .NET, etc.) in the outermost layer.
6. Ensure all dependencies point inward; apply Dependency Inversion Principle.

---

## 🪜 Decision Matrix

| Criterion                | Layered | Hexagonal | Onion | Clean |
| ------------------------ | ------- | --------- | ----- | ----- |
| Fast prototyping         | ✅       | ❌         | ❌     | ❌     |
| Testability              | ❌       | ✅         | ✅     | ✅     |
| Clear domain separation  | ❌       | ✅         | ✅     | ✅     |
| Infra decoupling         | ❌       | ✅         | ✅     | ✅     |
| Code readability (early) | ✅       | ❌         | ❌     | ❌     |
| Scaling with team size   | ❌       | ✅         | ✅     | ✅     |

---

## 🔧 Practical Tips

- Start with **Layered**, move toward **Hexagonal** or **Modular Onion** as complexity grows.
- Create **domain-focused interfaces** early—even in Layered code.
- Keep **infrastructure code in a separate folder/package**.
- Write **unit tests for domain + use cases**, integration tests for adapters.
- Follow **SOLID** principles to improve maintainability and testability.
- Apply **Dependency Inversion** to separate core logic from tech frameworks.

---

## 🚀 Coming Next:

On Day 4, we’ll explore **API Gateway, BFF (Backend for Frontend), and Edge Architecture**: how clients interact with services and how to design *external-facing entry points*.

---

Happy architecting! 📊

