# Atlas Commerce Architecture

## 1. Overview

Atlas Commerce is a TypeScript monorepo ecommerce application designed to demonstrate frontend engineering quality, maintainable architecture, and clear domain modeling.

The project focuses on architectural clarity, explicit boundaries between system components, and shared domain contracts across applications.

The system consists of a frontend application, a backend API, and shared packages for domain types and database access. The goal is to model realistic ecommerce flows while keeping the system understandable and extensible.

This document defines the architectural structure, design decisions, and engineering conventions used in the repository.

---

# 2. Architectural Principles

The architecture follows several guiding principles.

### Clarity Over Cleverness

The system prioritizes readability and explicit structure over complex abstractions. Code should be easy to understand and maintain.

### Explicit Domain Boundaries

Each domain has clear responsibilities. Domains should not leak logic into unrelated parts of the system.

### Shared Contracts

Domain types and validation schemas are shared between applications to maintain consistency and reduce duplication.

### Validation at Boundaries

Input validation occurs at API boundaries to prevent invalid data from entering the system.

### Avoid Premature Complexity

Infrastructure complexity is intentionally limited. The architecture focuses on maintainability rather than production-scale optimization.

### Incremental Expansion

The system is structured so new features can be added without major architectural changes.

---

# 3. System Architecture

The system follows a layered architecture distributed across a monorepo:

1.  **Frontend Application (`apps/web`)**: A Next.js application structured with **Feature-Sliced Design (FSD)**.
2.  **Backend API (`apps/api`)**: A Fastify-based REST API handling business logic and orchestration.
3.  **Shared Domain Contracts (`packages/shared`)**: Framework-agnostic types and Zod schemas.
4.  **Database Layer (`packages/database`)**: Drizzle ORM schema and persistence logic.

---

# 4. Frontend Architecture: Feature-Sliced Design (FSD)

To manage complexity and ensure scalability, the `apps/web` application follows the **Feature-Sliced Design (FSD)** methodology. This prevents the "spaghetti code" common in large React projects by enforcing a strict hierarchy.

### The FSD Layer Hierarchy

The frontend is organized into layers. A key rule of FSD is that **a layer can only import from layers below it**:

| Layer        | Responsibility                                                                    |
| :----------- | :-------------------------------------------------------------------------------- |
| **App**      | Global initialization: Providers, global styles, and the main entry point.        |
| **Pages**    | Composition of widgets and features into full-screen views.                       |
| **Widgets**  | Large, self-contained UI blocks (e.g., `Header`, `ProductGrid`).                  |
| **Features** | User actions that provide business value (e.g., `AddToCart`, `SearchProduct`).    |
| **Entities** | Business domain models and their minimal logic (e.g., `ProductCard`, `CartItem`). |
| **Shared**   | Low-level, reusable primitives: UI components (Button, Input), hooks, and utils.  |

### Benefits for Atlas Commerce

- **Predictable Imports**: No circular dependencies between components.
- **High Cohesion**: Related logic is grouped into "slices" within layers.
- **Scalability**: New features can be added by creating new slices without touching existing logic.

---

# 5. Monorepo Structure

```
apps/
  web/
  api/

packages/
  shared/
  database/
```

### apps/web

The web application is built with Next.js.

Responsibilities include:

- Rendering UI
- Managing routing
- Handling user interactions
- Calling backend APIs
- Consuming shared types

The web application does not contain business logic that belongs to backend domains.

---

### apps/api

The API application is built with Fastify.

Responsibilities include:

- Defining REST endpoints
- Validating requests
- Coordinating domain logic
- Handling authentication
- Accessing the database layer

All server-side business logic resides in the API.

---

### packages/shared

The shared package contains framework‑agnostic domain contracts.

This includes:

- TypeScript types
- Zod validation schemas
- Domain constants
- Shared utilities

The shared package must remain independent of specific frameworks.

---

### packages/database

The database package contains persistence infrastructure.

It includes:

- Drizzle schema definitions
- Database client configuration
- Migration setup

This package isolates database concerns from application logic.

---

# 6. Domain Boundaries

The system is organized around several domains.

### Product Domain

Responsible for:

- Product catalog
- Product details
- Product listing queries

---

### Cart Domain

Responsible for:

- Cart item management
- Quantity updates
- Cart subtotal calculations

---

### Order Domain

Responsible for:

- Order creation
- Order item snapshots
- Order history retrieval

---

### Auth Domain

Responsible for:

- User identity
- Authentication
- Protected access control

---

### Database Domain

Responsible for:

- Persistence
- Schema definitions
- Data storage concerns

Each domain owns its logic and data responsibilities.

---

# 7. Core Domain Model

The primary entities in the system include:

User  
Product  
Cart  
CartItem  
Order  
OrderItem  
Payment

These entities represent the core ecommerce data model used across applications.

Shared domain contracts are defined in `packages/shared`.

---

# 8. Order Lifecycle

Orders move through several states.

Supported states include:

pending  
paid  
processing  
shipped  
delivered  
cancelled

The MVP currently implements a simplified flow.

```
pending → paid
```

Additional states exist to allow future expansion without schema redesign.

---

# 9. Payment Lifecycle

Payments are simulated rather than processed through an external provider.

Payment states include:

pending  
succeeded  
failed

Orders are created only after successful payment.

This approach allows the checkout flow to be implemented without integrating a real payment provider.

---

# 10. API Design

The backend exposes REST-style endpoints.

Example endpoints include:

```
GET /products
GET /products/:id
GET /cart
POST /cart/items
PATCH /cart/items/:productId
DELETE /cart/items/:productId
POST /checkout
GET /orders
GET /orders/:id
```

Design conventions include:

- Resources are represented as nouns
- HTTP verbs define actions
- Validation occurs at request boundaries
- Responses follow a predictable JSON structure

---

# 11. API Response Format

All API responses follow a consistent structure.

### Success Response

```
{
  "data": {},
  "meta": {}
}
```

### Error Response

```
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request payload",
    "details": []
  }
}
```

Common error codes include:

VALIDATION_ERROR  
UNAUTHORIZED  
FORBIDDEN  
NOT_FOUND  
CONFLICT  
INTERNAL_SERVER_ERROR

---

# 12. Data Ownership

Responsibilities are separated across layers.

shared → domain contracts  
database → persistence logic  
api → server logic and validation  
web → UI and interaction

Applications should not duplicate domain contracts owned by the shared package.

---

# 13. Environment Configuration

Sensitive configuration belongs exclusively to the API application.

Examples include:

- database connection string
- authentication secrets

Frontend applications only use safe public environment variables.

---

# 14. Error Handling Strategy

Errors are handled consistently across the API.

Guidelines include:

- validate input early
- return structured error responses
- avoid exposing internal implementation details
- map domain failures to stable error codes
- log internal failures on the server

---

# 15. Testing Strategy

Testing focuses on critical logic rather than broad coverage.

Priority areas include:

1. Shared utilities and validation schemas
2. API endpoint behavior
3. Checkout and order creation flow
4. Critical frontend interactions

The goal is to validate core business behavior.

---

# 16. Git Workflow

This repository follows a lightweight workflow appropriate for a solo project.

### Principles

- small focused commits
- meaningful commit messages
- clean commit history

### Commit Prefixes

feat – new feature  
fix – bug fix  
refactor – internal improvement without behavior change  
docs – documentation updates  
chore – tooling or maintenance tasks  
test – tests added or updated

### Example Commit Messages

```
feat: implement product catalog page

fix: correct cart subtotal calculation

refactor: extract shared product types

docs: add architecture overview

chore: configure pnpm workspace
```

---

# 17. Coding Standards & Conventions

To maintain high code quality and consistency across the monorepo, the following conventions are enforced.

### 17.1 File & Folder Naming

- **Folders**: `kebab-case` (e.g., `user-profile/`)
- **React Components**: `PascalCase` (e.g., `ProductCard.tsx`)
- **Hooks**: `camelCase` with `use` prefix (e.g., `useCart.ts`)
- **Utilities/Logic**: `camelCase` (e.g., `formatCurrency.ts`)

### 17.2 Code Naming

- **Variables & Functions**: `camelCase` (e.g., `isAvailable`, `fetchData()`)
- **Constants**: `UPPER_SNAKE_CASE` (e.g., `API_RETRY_LIMIT`)
- **Types & Interfaces**: `PascalCase` (e.g., `UserType`, `OrderRequest`)

### 17.3 Database Naming (SQL & Drizzle)

- **Tables**: `snake_case` (plural) (e.g., `users`, `order_items`)
- **Columns**: `snake_case` (e.g., `created_at`, `user_id`)
- **Mapping**: In TypeScript/Drizzle, columns are mapped to `camelCase` (e.g., `createdAt: timestamp('created_at')`).

---

# 18. Definition of Done

A feature is considered complete when:

- the implementation works
- types are correct
- validation exists
- lint checks pass
- type checking passes
- critical logic has tests
- documentation remains accurate

---

# 19. Future Expansion

The architecture supports future capabilities such as:

- admin dashboard
- inventory management
- payment provider integration
- discount and coupon systems
- product reviews
- improved search capabilities
- CI/CD automation

These features are intentionally excluded from the MVP but the architecture allows them to be added without major structural changes.
