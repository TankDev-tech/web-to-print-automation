# System Architecture

This document describes the public high-level architecture of the **Web-to-Print Production Automation** system developed by [TankDev](https://tankdev.tech).

The system connects customer-facing product customization with order management and desktop print-production workflows while maintaining structured data across the complete process.

> This document intentionally describes system boundaries and component responsibilities without exposing proprietary source code, credentials, infrastructure configuration, customer data, or sensitive production logic.

## Architecture Goals

The architecture was designed around several operational requirements:

- Maintain a single structured representation of each order
- Keep product dimensions and production constraints under administrative control
- Preserve customer design data throughout the production workflow
- Separate customer-facing, administrative, and production responsibilities
- Transfer approved jobs from the web environment into the desktop production workflow
- Reduce repeated manual data entry between ordering and production
- Keep production-specific logic isolated from the public web interface

## High-Level Architecture

<pre>
┌─────────────────────────────────────┐
│              Customer               │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│        Web Application Layer        │
│                                     │
│  Next.js · React · TypeScript       │
│                                     │
│  • Product Catalog                  │
│  • Live Personalization             │
│  • Custom Design Editor             │
│  • Order Interface                  │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│          Application Layer          │
│                                     │
│          Python · FastAPI           │
│                                     │
│  • Product Operations               │
│  • Design Operations                │
│  • Order Operations                 │
│  • Validation                       │
│  • Production Data Access           │
└──────────────────┬──────────────────┘
                   │
                   ▼
┌─────────────────────────────────────┐
│             Data Layer              │
│                                     │
│             PostgreSQL              │
│                                     │
│  • Products                         │
│  • Product Dimensions               │
│  • Design Data                      │
│  • Orders                           │
│  • Production State                 │
└──────────────────┬──────────────────┘
                   │
          ┌────────┴────────┐
          │                 │
          ▼                 ▼
┌───────────────────┐  ┌──────────────────────┐
│    Admin Layer    │  │ Production Interface │
│                   │  │                      │
│ Order Management  │  │ Desktop Application  │
│ Product Control   │  │ Production Transfer  │
│ Workflow Control  │  │ Validation           │
└───────────────────┘  └──────────┬───────────┘
                                  │
                                  ▼
                       ┌──────────────────────┐
                       │  Adobe Illustrator   │
                       │                      │
                       │ Production Workflow  │
                       │ AI · PDF · PNG       │
                       └──────────────────────┘
</pre>

## Component Responsibilities

### 1. Web Application

The customer-facing application is responsible for product discovery, product configuration, design interaction, and order creation.

The interface supports two primary design paths:

**Product personalization**

Customers modify permitted fields of predefined print products while maintaining production constraints defined by the business.

**Custom design**

Customers create original designs using controlled typography, layout functionality, and a library containing more than 200 SVG design assets.

The browser interface is not responsible for defining production dimensions. Those constraints originate from product configuration controlled by the printing business.

### 2. FastAPI Service Layer

The API layer provides the boundary between user interfaces and persistent production data.

Its public architectural responsibilities include:

- Product data operations
- Design data operations
- Order creation and retrieval
- Input validation
- Production state management
- Communication between application components

Production-specific transformation rules and internal implementation details are intentionally outside the scope of this public documentation.

### 3. PostgreSQL Data Layer

PostgreSQL provides structured persistence for the system.

Instead of treating a customer design as an isolated file, the system maintains relationships between:

**Product → Dimensions → Design → Order → Production State**

This allows downstream components to operate on the same underlying order identity.

The objective is to prevent production information from becoming disconnected as an order moves between customer, administrative, and production environments.

### 4. Administrative Interface

The administrative layer provides operational control over the workflow.

Its responsibilities include management of:

- Products
- Product dimensions
- Orders
- Design information
- Production state

This separates business-controlled production configuration from customer-controlled design interaction.

### 5. Desktop Production Application

The desktop application acts as the bridge between the web-based system and the print-production environment.

Approved order data can be retrieved from the central system and prepared for the production workflow.

Before production transfer, required product, dimension, and design information can be validated.

This boundary is intentional: customer-facing application logic and production workstation responsibilities remain separate while operating on shared structured data.

### 6. Adobe Illustrator Workflow

Adobe Illustrator represents the final production environment in the documented workflow.

Structured information originating from the web system is transferred into the production process, where approved designs can be prepared for output.

Supported production outputs include:

- AI
- PDF
- PNG

Implementation details of proprietary transformation and Illustrator automation logic are not published in this repository.

## Data Flow

At a high level, an order moves through the system as follows:

<pre>
1. Product Selection
        │
        ▼
2. Product Personalization / Custom Design
        │
        ▼
3. Design & Product Validation
        │
        ▼
4. Structured Order Creation
        │
        ▼
5. Central Data Storage
        │
        ▼
6. Administrative Review
        │
        ▼
7. Desktop Production Transfer
        │
        ▼
8. Illustrator Production Workflow
        │
        ▼
9. Print-Ready Output
</pre>

The important architectural principle is that these stages are not modeled as unrelated files or manual hand-offs.

They operate around a shared order identity and structured production data.

## System Boundaries

The system intentionally separates three operational environments:

| Environment | Primary Responsibility |
| --- | --- |
| Customer | Product selection, personalization, and design |
| Administration | Product configuration, order management, and workflow control |
| Production | Production validation, desktop processing, and print-ready output |

This separation allows each interface to expose only the functionality required for its role.

## Design Principles

### Structured Data Over File Handoffs

Designs are associated with structured product and order information rather than being treated only as standalone production files.

### Production Constraints at the Source

Product dimensions and production rules originate from business-controlled configuration.

The customer interface operates within those boundaries.

### Shared Order Identity

Web, administrative, and production components operate around the same logical order.

### Explicit System Boundaries

Customer interaction, administrative operations, API responsibilities, persistent data, and production tooling are separated into defined components.

### Validation Before Production

Required information is validated before an order enters the production workflow.

## Technology Boundaries

| Layer | Technology |
| --- | --- |
| Web Application | Next.js, React, TypeScript, Tailwind CSS |
| API | Python, FastAPI |
| Data | PostgreSQL |
| Production Bridge | Desktop production application |
| Production Environment | Adobe Illustrator |
| Design Assets | SVG |
| Production Outputs | AI, PDF, PNG |

## Public vs. Proprietary Architecture

This repository documents the architecture at a level suitable for technical evaluation without exposing implementation assets that belong to the production system.

**Publicly documented**

- System boundaries
- Technology choices
- Component responsibilities
- High-level data flow
- Production workflow
- Integration boundaries

**Not publicly distributed**

- Production source code
- Credentials and secrets
- Infrastructure configuration
- Client or customer data
- Proprietary transformation logic
- Internal Illustrator automation implementation
- Security-sensitive implementation details

## Related Documentation

- [Repository Overview](../README.md)
- [Production Workflow](./workflow.md)
- [TankDev Case Study](https://tankdev.tech/tr/case-studies/print-production-automation)
- [System Overview](https://tankdev.tech/tr/systems/print-production-automation)

## About TankDev

[TankDev](https://tankdev.tech) develops custom software systems around real operational requirements, business rules, data flows, and system boundaries.

**Software Engineering · Applied AI · Process Automation**
