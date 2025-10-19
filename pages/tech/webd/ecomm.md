---
layout : page
title :  My ECommerce App Development
date : 2025-10-03
---

My App is called Xorbytes and it is an app to provide instant ordering of food in a restaurant from their menu, effectively bypassing the waiter altogether. It is of immense benefit in restaurants with a large traffic-to-waiter ratio, or for food courts housing multiple restaurants and having a self-service system.

# Step 1: Repository Setup

Create a repository of a **full-stack web application** with two main parts: a **server** (backend) and a **client** (frontend).

## 1. **Server (Backend)**
Located in the server directory.

**Purpose:**
Provides a REST API for the frontend to fetch data about cities, clusters, restaurants, and menus. It connects to a PostgreSQL database.

**Key Parts:**
- **Express App:**
  The entry point is index.ts, which sets up an Express server, loads environment variables, enables CORS, and mounts API routes.

- **API Routes:**
  Defined in myRoutes.ts, providing endpoints to:
  - List clusters in a city
  - List restaurants in a cluster
  - Get a restaurant’s menu grouped by category

- **Database Connection:**
  Managed by index.ts using the `pg` library to connect to PostgreSQL.

- **Type Definitions:**
  In index.ts, defining TypeScript interfaces for menu items and related data.

---

## 2. **Client (Frontend)**
Located in the client directory.

**Purpose:**
A React + TypeScript single-page application (SPA) that interacts with the backend API to display data and provide a user interface.

**Key Parts:**
- **React App Entry:**
  main.tsx and App.tsx are the entry points for the React application.

- **UI Components:**
  Modular, reusable components in components, including UI primitives like tables, cards, buttons, sheets, and separators.

- **Context:**
  CartContext.tsx (incomplete) is intended to provide a shopping cart context for managing cart state across the app.

- **Styling:**
  Uses Tailwind CSS, configured in App.css, for utility-first styling.

- **Utilities:**
  Helper functions in utils.ts for class name merging and other utilities.

- **Configuration:**
  - vite.config.ts: Vite build tool configuration.
  - `client/tsconfig*.json`: TypeScript configuration files.
  - components.json: UI component library configuration.

---

## 3. **Shared Concepts**
- **Data Model:**
  The backend and frontend share a conceptual data model: cities contain clusters, clusters contain restaurants, and restaurants have menus grouped by category.

- **API Communication:**
  The frontend fetches data from the backend via RESTful endpoints.

---

### **Summary Table**

| Part      | Directory   | Purpose                                                      |
|-----------|-------------|--------------------------------------------------------------|
| Server    | server   | REST API, database access, business logic                  |
| Client    | client   | React SPA, UI components, state management, API consumption|
| Shared    | Data Model  | Cities → Clusters → Restaurants → Menus                      |

---

**In summary:**
This repository is structured as a modern full-stack web app, with a PostgreSQL-backed Express API server and a React + TypeScript frontend, designed to display and interact with hierarchical food/restaurant data.
