# Non-Profit Community Outreach & Resource Allocation Tracker

An interactive Business Intelligence dashboard designed to unify fragmented operational spreadsheets, monitor branch expenditures against approved budgets and track community welfare interventions in real time.

---

### Project Overview & Inspiration

Earlier this year, while volunteering at a girls’ rescue centre, I observed firsthand the operational challenges grassroots non-profits face when managing community outreach and social work data. Multiple departments—including social casework, education support, field reintegration, and logistics—were each tracking interventions, family welfare distributions, and field expenditures independently across disconnected Excel spreadsheets.

Answering basic management questions—such as remaining budget balances for rescue trips, branch resource depletion rates or total quarterly beneficiaries—required hours of manual cross-referencing and data reconciliation. 

This project tries to address that gap by building a centralized, visual Business Intelligence solution that consolidates siloed operational logs, automates financial variance tracking and delivers clear, actionable visibility to leadership without requiring custom code.

---

### The Operational Challenge

Grassroots organizations and rescue centres operate under tight budgets and strict donor reporting timelines. Key operational friction points identified include:

* **Siloed Departmental Records:** Activity tracking remained disconnected from overall institutional budget allocations.
* **Delayed Decision-Making:** Budget overruns and supply shortages were often discovered weeks after field visits took place.
* **Administrative Burden:** Staff devoted significant hours to manual report assembly rather than direct community intervention.

---

### Data Architecture & Schema

The solution utilizes a two-table relational data model designed to mirror standard non-profit operations:

+---------------------------+             +---------------------------+
|       Center_Budgets      |             |       Outreach_Logs       |
+---------------------------+             +---------------------------+
| Center_Name (PK)          |<---[1:M]--->| Log_ID                    |
| Annual_Budget_KES         |             | Date                      |
| Target_Beneficiaries      |             | Center_Name (FK)          |
+---------------------------+             | Beneficiary_Category      |
| Assistance_Type           |
| Cost_Incurred_KES         |
+---------------------------+


#### Data Dictionary

##### 1. `Outreach_Logs` (Activity Table)
| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `Log_ID` | Text | Unique identifier for each outreach transaction |
| `Date` | Date | Date when assistance was delivered |
| `Center_Name` | Text | Name of the center/branch managing the activity |
| `Beneficiary_Category` | Text | Target demographic (e.g., Girls - Primary, Youth, Families) |
| `Assistance_Type` | Text | Type of aid provided (e.g., Food Hamper, Tuition Support) |
| `Cost_Incurred_KES` | Decimal/Currency | Direct operational cost incurred in Kenya Shillings (KES) |

##### 2. `Center_Budgets` (Dimension / Target Table)
| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `Center_Name` | Text | Primary key identifying the operating branch |
| `Annual_Budget_KES` | Decimal/Currency | Approved annual spending limit |
| `Target_Beneficiaries` | Integer | Planned outreach reach per branch |

---

### Key Calculations & Measures (DAX)

The dashboard relies on three dynamic calculations that adapt automatically based on active filters and date ranges:

#### 1. Total Outreach Spend
Calculates cumulative actual spending across all recorded field interventions:
```
Total Spend = SUM(Outreach_Logs[Cost_Incurred_KES])
```
#### 2. Total Approved Budget
Calculates the authorized budget allocated for the selected centers:
```
Total Budget = SUM(Center_Budgets[Annual_Budget_KES])
```
#### 3. Budget Variance (Surplus/Deficit)
Measures the remaining financial balance:

Positive - Under Budget

Negative - Over Budget
```
Budget Variance = [Total Budget] - [Total Spend]

