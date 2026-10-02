# Web-to-Print Production Automation

A custom web-to-print production system developed by [TankDev](https://tankdev.tech), connecting browser-based product customization, order management, and desktop production workflows in a single system.

The platform transforms customer-created designs into structured production data and connects the web ordering experience directly with Adobe Illustrator-based print production.

## Overview

Traditional online ordering does not necessarily automate print production.

Design information, product dimensions, customer preferences, and production files are often handled across separate systems, creating repeated data entry and manual file preparation.

This system connects the complete workflow:

**Web Design → Order → Admin → Desktop Production → Adobe Illustrator → Print-Ready Output**

## What the System Does

Customers can follow two controlled design workflows:

- Personalize predefined print products directly in the browser
- Create original designs using typography, layout tools, and 200+ premium SVG assets

Product dimensions and production constraints are defined by the printing business rather than by the customer.

When an order is completed, the product configuration, dimensions, customer selections, and design data are stored as a single structured order record.

The approved design can then be transferred to the desktop production application and processed through an Adobe Illustrator workflow to generate production-ready files.

## Production Workflow

<pre>
Customer
   │
   ▼
Next.js Web Application
   │
   ├── Product Catalog
   ├── Live Personalization
   └── Custom Design Editor
   │
   ▼
FastAPI Service Layer
   │
   ▼
PostgreSQL
   │
   ├── Products
   ├── Dimensions
   ├── Designs
   └── Orders
   │
   ▼
Admin Panel
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

## Core Capabilities

| Capability | Description |
| --- | --- |
| Live product personalization | Customers modify permitted content directly in the browser with real-time preview |
| Custom design editor | Original designs can be created using typography, layout tools, and 200+ SVG assets |
| Production-aware dimensions | Design areas are constrained by product dimensions defined by the printing business |
| Centralized order data | Product, dimensions, design data, and customer selections remain connected to the same order |
| Admin workflow | Orders and production data are managed through a centralized administrative interface |
| Desktop production | Approved jobs are transferred from the web system to a dedicated production application |
| Illustrator integration | Production data is transferred into the Adobe Illustrator workflow |
| Print-ready generation | Approved designs can be prepared as AI, PDF, or PNG production files |

## Architecture

The system combines web, API, relational data, administrative, and desktop production components around the same order and design model.

<pre>
┌─────────────────────────────┐
│      Customer Interface     │
│           Next.js           │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│        FastAPI Layer        │
│ Orders · Designs · Products │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│         PostgreSQL          │
│ Structured Production Data  │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│         Admin Panel         │
│ Order & Production Control  │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│ Desktop Production Software │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│      Adobe Illustrator      │
│       AI · PDF · PNG        │
└─────────────────────────────┘
</pre>

## Technology Stack

**Frontend**

- Next.js
- TypeScript
- React
- Tailwind CSS

**Backend**

- Python
- FastAPI
- PostgreSQL

**Production**

- Desktop production application
- Adobe Illustrator integration
- SVG-based design assets
- AI, PDF, and PNG output workflows

## System Scope

The implemented system currently includes:

- **200+** premium SVG design assets
- **2** customer design workflows
- **7** connected workflow stages
- **3** production output formats
- Browser-based product personalization
- Custom design editor
- Centralized administration
- Desktop production workflow
- Adobe Illustrator integration

These figures describe the implemented system scope rather than commercial or performance KPIs.

## Engineering Approach

The system is designed around a shared order identity and structured design data.

The web interface, API, relational data model, administrative interface, and desktop production application operate around the same underlying production workflow.

Validation is performed before production transfer to ensure required product, dimension, and design information is available.

Sensitive production rules, transformation logic, infrastructure configuration, and implementation details are intentionally excluded from this public repository.

## Documentation

Detailed public documentation will be maintained in this repository as the technical showcase evolves.

- Architecture documentation
- Production workflow
- System boundaries
- Public screenshots and diagrams

## Case Study

A detailed engineering case study covering the operational problem, system design, workflow, and implementation scope is available on TankDev:

**[Read the Web-to-Print Production Automation Case Study](https://tankdev.tech/tr/case-studies/print-production-automation)**

## System Overview

A product-oriented overview of the system and its capabilities is also available:

**[Explore the Print Production Automation System](https://tankdev.tech/tr/systems/print-production-automation)**

## Source Code

This repository contains public technical documentation for a proprietary TankDev system.

Production source code, credentials, infrastructure configuration, client data, sensitive production rules, and proprietary transformation logic are not included.

## About TankDev

[TankDev](https://tankdev.tech) is a software engineering organization focused on custom software systems, applied artificial intelligence, process automation, web applications, and system integration.

**Software Engineering · Applied AI · Process Automation**

---

Developed by **[TankDev](https://tankdev.tech)**  
[Website](https://tankdev.tech) · [Case Studies](https://tankdev.tech/tr/case-studies)
