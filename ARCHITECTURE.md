# Atlas Commerce Architecture

## 1. Overview

Atlas Commerce is a solo-built ecommerce portfolio project designed to demonstrate frontend engineering, scalable monorepo architecture, and clear domain modeling.

The project intentionally focuses on code quality, architectural clarity, and shared domain contracts rather than production-level infrastructure complexity. The goal is to showcase realistic ecommerce flows while maintaining a clean, maintainable codebase that is easy to discuss in technical interviews.

This repository implements a simplified ecommerce MVP with a dedicated frontend application, backend API, shared domain packages, and a database package within a TypeScript monorepo.

---

# 2. Goals

## Primary Goals

- Demonstrate strong frontend engineering skills
- Showcase scalable monorepo architecture
- Share domain types and schemas across applications
- Model realistic ecommerce entities and flows
- Maintain clear package boundaries and conventions
- Keep the codebase understandable and extensible

## Non‑Goals

The following are intentionally excluded to keep the project focused:

- Production payment gateway integration
- Full admin dashboard
- Marketplace or multi‑vendor support
- Distributed infrastructure or complex caching
- Fulfillment logistics systems

---

# 3. Product Scope (MVP)

## Included Features

- Browse product catalog
- View product details
- Add products to cart
- Update cart quantities
- Remove cart items
- Checkout flow
- Create order with mock payment
- View customer order history
- Basic authentication
- API‑driven product, cart, and order operations

## Excluded Features

- Real payment gateways
- Product reviews and ratings
- Discount or coupon systems
- Wishlist functionality
- Inventory management UI
- Shipment tracking
- Recommendation engines
- Email notifications

These features are intentionally excluded to keep the MVP focused on architecture and frontend quality.

---

# 4. User Roles

## Guest

Guests can:

- Browse products
- View product details
- Add items to cart
- Modify cart contents

Guests cannot:

- Create orders
- View order history
- Access protected endpoints

## Customer

Authenticated customers can:

- Perform all guest actions
- Checkout and place orders
- View order history
- View order details

## Admin (Future Scope)

Admin capabilities are recognized for future expansion but are not implemented in the MVP.

Possible future capabilities include product management, order management, and inventory control.

---

# 5. Core User Journeys

## Product Discovery

1. User visits storefront
2. Browses product catalog
3. Opens product details
4. Adds item to cart

## Cart Management

1. User opens cart
2. Reviews selected items
3. Updates quantities or removes items
4. Proceeds to checkout

## Checkout

1. User enters checkout
2. Provides shipping information
3. Reviews order summary
4. Mock payment is processed
5. Order is created
6. User sees confirmation

## Order History

1. Customer opens account page
2. Views list of previous orders
3. Opens order details

---

# 6. Order Lifecycle

Order states:

- pending
- paid
- processing
- shipped
- delivered
- cancelled

MVP flow:

pending → paid

Additional states exist for realism and future extension.

---

# 7. Payment Lifecycle

Payments are simulated rather than processed through a real provider.

Payment states:

- pending
- succeeded
- failed

Orders are only created after successful payment.

---

# 8. Architecture Principles

The architecture follows several guiding principles:

- Prefer clarity over clever abstractions
- Keep domain boundaries explicit
- Share contracts through packages
- Avoid premature complexity
- Keep the MVP intentionally small
- Design for incremental expansion

---

# 9. Tech Stack

## Frontend

- Next.js
- React
- TypeScript
- Tailwind CSS

## Backend

- Fastify
- TypeScript

## Database

- PostgreSQL
- Drizzle ORM

## Tooling

- pnpm workspaces
- Prettier
- Zod validation
- ESLint
- TypeScript strict mode

---

# 10. Monorepo Structure

```
apps/
  web/
  api/

packages/
  shared/
  database/
```

## apps/web

Next.js frontend responsible for:

- UI rendering
- routing
- client interactions
- calling backend APIs
- consuming shared types

## apps/api

Fastify backend responsible for:

- REST endpoints
- request validation
- domain orchestration
- authentication
- database access

## packages/shared

Shared domain contracts:

- TypeScript types
- Zod schemas
- constants
- framework‑agnostic utilities

## packages/database

Database package containing:

- Drizzle schema
- database client
- migrations configuration

---

# 11. Domain Boundaries

## Product Domain

Handles product catalog and product details.

## Cart Domain

Handles cart items, quantity updates, and subtotal calculations.

## Order Domain

Responsible for order creation, order items snapshot, and order history.

## Auth Domain

Responsible for user identity and protected access.

## Database Domain

Handles persistence concerns and schema definitions.

---

# 12. API Design

The API follows pragmatic REST conventions.

Example endpoints:

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

Guidelines:

- Use nouns for resources
- Use HTTP verbs for actions
- Validate requests at the API boundary
- Return predictable JSON structures

---

# 13. API Response Format

Success response:

```
{
  "data": {},
  "meta": {}
}
```

Error response:

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

- VALIDATION_ERROR
- UNAUTHORIZED
- FORBIDDEN
- NOT_FOUND
- CONFLICT
- INTERNAL_SERVER_ERROR

---

# 14. Shared Types and Schemas

Shared contracts live in `packages/shared`.

Examples:

- Product
- Cart
- CartItem
- Order
- User

Zod schemas are used for request validation and domain rules.

The shared package must remain framework‑agnostic.

---

# 15. Data Ownership

Responsibilities are clearly separated:

- shared → domain contracts
- database → persistence layer
- api → server logic and validation
- web → UI and user interaction

No application should duplicate domain contracts owned by shared.

---

# 16. Environment Variables

Sensitive configuration belongs to the API application.

Examples include:

- database connection string
- authentication secrets

Frontend variables should only contain safe public values.

---

# 17. Error Handling

Errors follow a consistent structure.

Guidelines:

- validate input early
- return structured error responses
- avoid exposing internal details
- map domain failures to stable error codes

---

# 18. Testing Strategy

Testing focuses on critical logic rather than exhaustive coverage.

Priority areas:

1. shared domain utilities
2. API endpoint behavior
3. core frontend flows

The goal is to validate business logic and important user journeys.

---

## 19. Git Workflow

This is a solo project with a lightweight workflow.

### Principles

- small focused commits
- meaningful commit messages
- maintain a clean and readable history

### Commit Convention

Commit messages follow a simplified conventional format.

Prefixes used in this repository:

- `feat` – new feature
- `fix` – bug fix
- `refactor` – internal improvement without behavior change
- `docs` – documentation updates
- `chore` – tooling or maintenance tasks
- `test` – tests added or updated

### Example Commit Messages

```
feat: implement product catalog page

fix: correct cart subtotal calculation

refactor: extract shared product types

docs: add architecture overview

chore: configure pnpm workspace
```

---

# 20. Future Expansion

The architecture supports future features such as:

- admin dashboard
- inventory management
- payment provider integration
- discount system
- reviews
- search improvements

---

# 21. Definition of Done

A feature is complete when:

- implementation works
- types and validation are correct
- lint and type checks pass
- important logic has tests
- documentation remains accurate

---

# 22. Architecture Baseline

Atlas Commerce is a TypeScript monorepo containing a Next.js frontend, Fastify API, shared domain packages, and a database layer.

The architecture baseline is established once product scope, domain boundaries, API conventions, and engineering standards are documented.
