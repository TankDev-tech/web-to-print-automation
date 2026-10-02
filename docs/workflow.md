# Production Workflow

This document describes the public operational workflow of the **Web-to-Print Production Automation** system developed by [TankDev](https://tankdev.tech).

The workflow connects customer-facing product design with administrative review and desktop print production while preserving the relationship between product configuration, design data, order information, and production output.

> This document describes the workflow at a public system level. Proprietary transformation logic, production source code, credentials, client data, and security-sensitive implementation details are intentionally excluded.

## Workflow Overview

<pre>
Customer
   │
   ▼
Product Selection
   │
   ▼
Design Path
   │
   ├── Product Personalization
   │
   └── Custom Design
   │
   ▼
Validation
   │
   ▼
Order Creation
   │
   ▼
Central Data Storage
   │
   ▼
Administrative Review
   │
   ▼
Production Transfer
   │
   ▼
Desktop Production Application
   │
   ▼
Adobe Illustrator
   │
   ▼
AI / PDF / PNG
</pre>

## Stage 1 — Product Selection

The workflow begins with a product configured by the printing business.

Each product can carry production-related information such as dimensions and design constraints.

The customer selects a product without directly controlling the underlying production configuration.

**Input:** Product catalog  
**Process:** Product selection  
**Output:** Selected product with defined production parameters  
**Responsibility:** Customer / Web Application

## Stage 2 — Design Path Selection

After selecting a product, the customer proceeds through one of two supported design workflows.

### Product Personalization

The customer modifies permitted content within an existing design.

The system keeps the interaction within the boundaries defined for that product.

### Custom Design

The customer creates an original composition using available design functionality, including typography, layout controls, and a library of more than 200 SVG assets.

Both paths ultimately produce design data associated with the selected product.

**Input:** Selected product  
**Process:** Personalization or custom design  
**Output:** Product-associated design data  
**Responsibility:** Customer / Web Application

## Stage 3 — Validation

Before the workflow proceeds to order creation, required information is validated.

Validation ensures that the system has the information required to maintain the relationship between the selected product, its dimensions, the customer design, and the resulting order.

Production-sensitive validation rules are not documented publicly.

**Input:** Product configuration and design data  
**Process:** Required-data validation  
**Output:** Validated order input  
**Responsibility:** Application Layer

## Stage 4 — Order Creation

Once the required information is available, the system creates a structured order.

Rather than treating the customer design as an isolated production file, the order maintains relationships between relevant system entities.

<pre>
Product
   │
   ▼
Dimensions
   │
   ▼
Design
   │
   ▼
Order
</pre>

This shared identity allows later administrative and production stages to work with the same logical order.

**Input:** Validated product and design information  
**Process:** Structured order creation  
**Output:** Persistent order record  
**Responsibility:** FastAPI / PostgreSQL

## Stage 5 — Central Data Storage

Order information is persisted in PostgreSQL.

The central data model provides a common source for customer-facing, administrative, and production components.

At a high level, the stored relationships include:

- Product information
- Product dimensions
- Design data
- Order information
- Production state

This reduces the need to reconstruct production context from disconnected files or separate manual records.

**Input:** Structured order  
**Process:** Persistent storage  
**Output:** Centrally accessible production data  
**Responsibility:** PostgreSQL

## Stage 6 — Administrative Review

The administrative interface provides operational visibility over incoming orders and production-related information.

The printing business can manage the workflow from a separate interface without exposing administrative controls to the customer-facing application.

The administrative stage acts as the operational boundary between order intake and production.

**Input:** Stored order and design information  
**Process:** Administrative review and workflow control  
**Output:** Order ready for production processing  
**Responsibility:** Administrative Interface

## Stage 7 — Production Transfer

When an order reaches the appropriate production state, its required information can be retrieved by the desktop production application.

This stage connects the web-based environment with the production workstation.

Before downstream processing, required product, dimension, order, and design information can be checked.

<pre>
Web System
    │
    │ Structured Order Data
    ▼
Desktop Production Application
</pre>

The desktop application acts as an explicit integration boundary rather than placing production responsibilities inside the customer-facing web application.

**Input:** Approved structured order data  
**Process:** Retrieval and production preparation  
**Output:** Production-ready job data  
**Responsibility:** Desktop Production Application

## Stage 8 — Adobe Illustrator Workflow

Prepared production information is transferred into the Adobe Illustrator workflow.

Illustrator serves as the production environment for preparing the final files required by the printing process.

The implementation of the Illustrator automation and proprietary transformation logic is intentionally not included in this public repository.

**Input:** Production-ready job data  
**Process:** Illustrator production workflow  
**Output:** Prepared production document  
**Responsibility:** Production Environment

## Stage 9 — Print-Ready Output

The documented workflow supports multiple production output formats:

- AI
- PDF
- PNG

The required format can then be used within the downstream print-production process.

**Input:** Prepared production document  
**Process:** Production output generation  
**Output:** AI, PDF, or PNG  
**Responsibility:** Production Environment

## Workflow Responsibilities

| Stage | Primary Component | Responsibility |
| --- | --- | --- |
| Product Selection | Web Application | Present business-configured products |
| Design | Web Application | Capture personalization or custom design data |
| Validation | Application Layer | Validate required workflow information |
| Order Creation | FastAPI | Create structured order data |
| Persistence | PostgreSQL | Maintain order and production relationships |
| Administrative Review | Admin Interface | Control operational workflow |
| Production Transfer | Desktop Application | Retrieve and prepare approved jobs |
| Production | Adobe Illustrator | Process production document |
| Output | Production Environment | Generate AI, PDF, or PNG |

## Workflow Principle

The core workflow principle is:

**Preserve structured production context from customer interaction to final production output.**

The system therefore avoids treating each stage as an unrelated manual handoff.

<pre>
Customer Intent
      │
      ▼
Structured Design
      │
      ▼
Structured Order
      │
      ▼
Controlled Production Transfer
      │
      ▼
Print-Ready Output
</pre>

Product, dimension, design, order, and production information remain logically connected throughout the workflow.

## Operational Boundaries

The workflow separates responsibilities across three primary environments:

| Environment | Responsibility |
| --- | --- |
| Customer Environment | Product selection and design interaction |
| Administrative Environment | Order and workflow management |
| Production Environment | Desktop processing and print-ready output |

This separation keeps customer interaction, business operations, and production tooling within their appropriate system boundaries.

## Public Documentation Boundary

This document intentionally focuses on the observable system workflow.

### Included

- Workflow stages
- Component responsibilities
- High-level data movement
- Operational boundaries
- Production integration points
- Output formats

### Excluded

- Production source code
- Credentials and secrets
- Client or customer data
- Internal infrastructure configuration
- Proprietary transformation algorithms
- Illustrator automation implementation
- Security-sensitive production logic

## Related Documentation

- [Repository Overview](../README.md)
- [System Architecture](./architecture.md)
- [TankDev Case Study](https://tankdev.tech/tr/case-studies/print-production-automation)
- [System Overview](https://tankdev.tech/tr/systems/print-production-automation)

## About TankDev

[TankDev](https://tankdev.tech) develops custom software systems around real operational requirements, business rules, data flows, and system boundaries.

**Software Engineering · Applied AI · Process Automation**
