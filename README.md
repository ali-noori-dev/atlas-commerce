# Atlas Commerce

A solo-built ecommerce portfolio project focused on frontend engineering, clean architecture, and domain-driven design.

Atlas Commerce demonstrates how a modern ecommerce application can be structured using a TypeScript monorepo with clear separation between frontend, backend, shared contracts, and database layers.

This project prioritizes architectural clarity and maintainability over feature volume.

---

## 🎯 Purpose

The goal of this project is to:

- Showcase frontend engineering quality
- Demonstrate scalable monorepo structure
- Model realistic ecommerce flows
- Share contracts between applications
- Apply consistent API design and validation
- Maintain engineering discipline in a solo codebase

For full technical architecture details, see: **`ARCHITECTURE.md`**

---

## ✅ MVP Features

The MVP implements a complete purchase journey:

- Browse product catalog
- View product details
- Add/update/remove cart items
- Checkout flow
- Mock payment processing
- Order creation
- Order history (authenticated users)
- Basic authentication

### Intentionally Out of Scope

- Real payment gateway integration
- Admin dashboard
- Reviews & ratings
- Discount system
- Wishlist
- Inventory management UI
- Multi-vendor support
- Shipment tracking

Scope limitation is intentional and part of the architectural discipline.

---

## 🧱 Project Structure

```
apps/
  web/        # Next.js storefront
  api/        # Fastify REST API

packages/
  shared/     # Shared domain types & validation schemas
  database/   # Prisma schema & database client
```

---

## 🛠 Tech Stack

### Frontend

- Next.js
- React
- TypeScript
- Tailwind CSS

### Backend

- Fastify
- TypeScript

### Database

- PostgreSQL
- Prisma

### Tooling

- pnpm workspaces
- ESLint
- Prettier
- TypeScript (strict mode)

---

## 🚀 Getting Started

### Prerequisites

- Node.js 20+
- pnpm 9+
- PostgreSQL

### Installation

```bash
git clone https://github.com/ali-noori-dev/atlas-commerce.git
cd atlas-commerce
pnpm install
```

### Run in Development

```bash
pnpm dev:web
```

Or run apps individually:

```bash
pnpm --filter web dev
pnpm --filter api dev
```

### Build

```bash
pnpm build
```

### Lint & Type Check

```bash
pnpm lint
pnpm typecheck
```

---

## 🔐 Environment Variables

### API

```env
DATABASE_URL=
JWT_SECRET=
PORT=
```

### Web

```env
NEXT_PUBLIC_API_BASE_URL=
```

Sensitive configuration is restricted to the backend.

---

## 🧪 Testing Approach

Testing focuses on high-value logic:

- Shared utilities and schemas
- API endpoint behavior
- Checkout and order flow
- Critical frontend interactions

The goal is validating business logic rather than maximizing coverage metrics.

---

## 💼 Portfolio Highlights

This project demonstrates:

- Frontend architecture decisions
- Clear domain modeling
- Contract sharing across applications
- Structured monorepo organization
- Maintainable project structure
- Professional engineering workflow in a solo environment

Atlas Commerce emphasizes clarity, scalability, and thoughtful system design.
