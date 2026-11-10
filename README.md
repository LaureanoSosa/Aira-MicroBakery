# Aira-MicroBakery
Management System for a Microbakery

A spreadsheet-based production planning (ERP) system optimized for an artisanal sourdough and pastry bakery. This project eliminates manual data entry, redundancy, and calculation errors through a "Single Source of Truth" architecture integrated into the Google Workspace ecosystem.

## 🎯 The Problem

The initial management in a traditional Excel file presented critical bottlenecks that limited scalability:

* **Transactional Duplication:** Sales had to be recorded in both the orders sheet and the income sheet.


* **Static Calculation:** Recipe calculators required manual entry of dough quantities every time daily demand changed.


* **Fragile Costing:** Due to inflation, updating flour or butter prices misaligned the costing and batch profitability calculations.



## 🏗️ Solution Architecture (3 Levels)

The model abandons flat tables and adopts a clean three-layer relational data flow:

```text
 ┌────────────────────────────────────────────────────────┐
 │ 1. DATA ENTRY (Transactional)                          │
 │   - Customer Orders (Google Forms)                     │
 │   - Raw Material Purchases Log                         │
 │   - Baking and Batch Log                               │
 └──────────────────────────┬─────────────────────────────┘
                            ▼
 ┌────────────────────────────────────────────────────────┐
 │ 2. MASTER TABLES (Configuration)                       │
 │   - DB_Products_Recipes (Baker's percentages)          │
 │   - Config_Fixed_Costs_Rates (Electricity, Packaging)  │
 └──────────────────────────┬─────────────────────────────┘
                            ▼
 ┌────────────────────────────────────────────────────────┐
 │ 3. AUTOMATED OPERATIONAL VIEWS                         │
 │   - Daily Baking Plan (Dynamic dough calculation)      │
 │   - Income and Logistics Control                       │
 │   - Real-Time Costing and Profitability                │
 └────────────────────────────────────────────────────────┘

```

## ✨ Key Features

* **Automatic Order Consolidation:** Automatically sums the required units (Rustic Bread, Focaccia, Croissants, etc.) upon selecting the production date.


* **Dynamic Dough Explosion:** Instantly transforms bread demand into exact grams of 000 Flour, Whole Wheat Flour, Water, Sourdough Starter, Salt, and Seeds using baker's percentages.


* **Inflation Protection:** The costing view includes a function that automatically fetches the latest price paid per gram for each ingredient from the purchases database, recalculating total cost and gross margin (%) of the batch in real-time.


* **Indirect Costs Integration:** Calculates unit electrical cost by multiplying oven minutes, equipment power (e.g., $6.8\text{ kW}$ Convection Oven), and the current electricity rate.


* **Retail vs. Wholesale Pricing:** Automatically calculates weighted average real prices based on the sales channel (Retail vs. Wholesale) to ensure accurate profitability margins.



## 🚀 Implementation Guide

1. **Import:** Upload the `Aira_Sistema_Gestion_Unificado.xlsx` file to Google Drive and open it as a native Google Sheet to enable query automations (`FILTER`, `SUMIFS`, `XLOOKUP`).


2. **Order Linking:** Create a public Google Form for order taking (name, products, delivery date) and link the responses to directly feed the `DB_Pedidos` (Orders DB) tab.


3. **Weekly Operational Flow:**
* **Wednesday:** Close form reception.


* **Thursday:** Select the date in the `Prod_Horneada_Dia` tab to get the exact weights for sourdough feeding and mixing.


* **Saturday:** Log oven times in `DB_Lotes` and track pickups/payments using interactive checkboxes in the `Ventas_Cobranzas` view.
