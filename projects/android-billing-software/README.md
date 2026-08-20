# Android Billing Software

> A Flutter-based billing and order management system designed for small food stalls and retail setups where speed, reliability, and simplicity matter.

**Status:** Completed
**Platform:** Android
**Built with:** Flutter · Dart · SQLite

---

## Overview

**Android Billing Software** is a mobile billing and order management application built for small food businesses and similar retail environments.

The system covers the complete order lifecycle, from creating an order and managing menu items to serving customers, printing receipts, and analysing business performance.

The application was designed around a simple goal:

> **Make everyday billing fast enough to work in a busy food-stall environment.**

The application operates primarily with local data, making it suitable for environments where a lightweight, offline-first workflow is more useful than a complex cloud-based POS system.

---

## The Problem

Small food stalls often rely on handwritten orders, basic calculators, or generic billing applications that introduce unnecessary steps into the ordering process.

A practical billing system needs to handle several things quickly:

* Creating an order
* Selecting menu items
* Editing pending orders
* Tracking order status
* Printing receipts
* Managing the menu
* Understanding daily sales

This project combines those workflows into a single Android application.

---

## What I Built

The application is divided into four main areas:

### 🧾 Orders

The order system manages the complete lifecycle of an order.

* Create orders with optional customer names
* Select and quantity menu items
* Edit pending orders
* Mark orders as Served or Cancelled
* Search orders by customer name or order number
* View detailed order breakdowns
* Print and reprint receipts
* Track Pending, Served, and Cancelled orders

### 🍫 Menu Management

The menu system allows the business to manage its catalogue without modifying the application.

* Create, edit, and delete categories
* Create, edit, and delete menu items
* Assign items to categories
* Organise items by category
* Automatically move items to Uncategorized when a category is deleted

### 📊 Business Reports

The reporting system turns the stored order data into useful business information.

It provides:

* Revenue
* Order count
* Average order value
* Cancellation count
* Top 5 selling items
* Best-performing item in each category
* Peak and slowest days
* Peak business hours
* Weekday demand patterns
* Worst-performing items

The application also includes a **Danger Zone** that allows all business data to be reset after a double-confirmation flow.

### 🖨️ Bluetooth Receipt Printing

The application supports ESC/POS-compatible 58mm Bluetooth thermal printers.

Receipts contain:

* Shop name
* Date and time
* Order number
* Customer name
* Itemised order
* Total amount

The selected printer is remembered, so staff do not need to select the printer for every order.

If printing fails, the saved printer is cleared and the application allows the printer to be selected again.

---

## Application Flow

```text
                    ┌──────────────┐
                    │   New Order  │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │ Select Items │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │ Pending Order│
                    └──────┬───────┘
                       ┌───┴───┐
                       ▼       ▼
                   Served   Cancelled
                       │
                       ▼
                 Print Receipt
                       │
                       ▼
                 Business Data
                       │
                       ▼
                    Reports
```

---

## Architecture

The application uses a lightweight Flutter architecture built around screens, application data, and a SQLite persistence layer.

```text
                    Flutter Application
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
       Orders            Menu            Reports
       Screen            Screen           Screen
          │                │                │
          └────────────────┼────────────────┘
                           │
                           ▼
                    Application Data
                           │
                           ▼
                    Database Helper
                           │
                           ▼
                        SQLite
```

Bluetooth printing operates alongside the application workflow through the printer integration layer.

---

## Tech Stack

| Technology                    | Purpose                          |
| ----------------------------- | -------------------------------- |
| **Flutter**                   | Android application framework    |
| **Dart**                      | Application programming language |
| **SQLite / sqflite**          | Local persistent data storage    |
| **shared_preferences**        | Persisting the selected printer  |
| **flutter_bluetooth_printer** | ESC/POS Bluetooth printing       |
| **Material 3**                | Application UI system            |

The application uses a custom brown colour palette to match the visual identity of the food business.

---

## Project Structure

```text
lib/
├── main.dart
│
├── data/
│   ├── app_data.dart
│   └── database_helper.dart
│
└── screens/
    ├── orders_screen.dart
    ├── billing_screen.dart
    ├── menu_screen.dart
    └── reports_screen.dart
```

The main screens are separated by responsibility, while the data layer handles application state and persistence.

---

## Engineering Highlights

### Local-first data

The application uses SQLite for persistent business data rather than depending on a remote backend.

This keeps the core billing workflow lightweight and reduces dependence on network connectivity.

### Persistent printer selection

The selected Bluetooth printer is stored locally so staff don't have to repeat the printer-selection process for every receipt.

### Order lifecycle management

Orders aren't treated as static bills. They move through explicit states:

```text
Pending → Served
        ↘ Cancelled
```

This allows the application to distinguish active orders from completed and cancelled business activity.

### Automatic category handling

Deleting a category does not leave its items without a valid reference. Its items are automatically moved to **Uncategorized**.

### Business analytics

The reporting system derives useful patterns from the stored order history rather than simply displaying raw transactions.

---

## Challenges & Design Decisions

One of the main design considerations was balancing **functionality with speed**.

A billing application used during busy periods cannot make the user navigate through unnecessary screens for simple operations.

The application therefore keeps common workflows close together:

**Select item → Adjust quantity → Complete order → Print**

The same principle influenced the printer workflow, where the selected printer is remembered rather than requiring repeated configuration.

---

## What This Project Demonstrates

This project gave me practical experience with:

* Flutter application development
* Dart
* SQLite database design
* CRUD operations
* Local state management
* Android Bluetooth integration
* Thermal receipt printing
* Order lifecycle modelling
* Business analytics
* UI/UX for operational software
* Designing software around a real-world workflow

---

## Project Status

**Completed**

The application implements the core billing, order management, menu management, reporting, and Bluetooth printing workflows.

---

## Source Code

🔒 **Private repository**

The source code is intentionally kept private. This page provides a high-level overview of the application's functionality, architecture, and engineering decisions.

---

## Author

**Arunan Kavirajan**

IT undergraduate at SRM Institute of Science and Technology, Chennai.

Building software, experimenting with AI, and turning ideas into working products.

[GitHub](https://github.com/Arunan-Kavirajan) · [LinkedIn](https://www.linkedin.com/in/arunan-kavirajan)
