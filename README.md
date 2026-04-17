# HCL-Hospitality-ETL

# 🏨 Hospitality ETL Pipeline – Daily Occupancy & Revenue Analytics

## 📌 Project Overview

This project builds an **end-to-end ETL pipeline using Informatica (IICS/PowerCenter)** to process hotel data and generate key business insights such as **occupancy, revenue, room performance, and customer value**.

The system reads raw CSV files, performs data transformation, and loads structured data into a database for analytics.

---

## 🎯 Business Objective

Hotels want to track:

* Number of guests checked in/out
* Room occupancy
* Daily revenue
* Customer behavior

This project automates these calculations using ETL pipelines.

---

## 📂 Data Sources

### 1. Checkin_Checkout_Hospitality.csv (Fact Data)

* StayID
* GuestID
* RoomID
* CheckinDateTime
* CheckoutDateTime
* RoomRate
* ExtraCharges
* DiscountAmount
* TotalAmount
* Status
* LastUpdated

---

### 2. Guest_Master_Hospitality.csv

* GuestID
* FirstName
* LastName
* Gender
* Contact Details

---

### 3. Room_Master_Hospitality.csv

* RoomID
* RoomType
* FloorNumber
* BaseRate
* RoomStatus

---

## 🏗️ Architecture

```
CSV Files
   ↓
Mapping 1 (Data Processing)
   ↓
GUEST_STAY (Fact Table)
   ↓
Analytical Mappings (4 Use Cases)
   ↓
Summary Tables
```

---

## 🔄 ETL Process

### 🔹 Mapping 1: M_LOAD_GUEST_STAY

#### Flow:

```
Source → Source Qualifier → Expression → Lookup → Target
```

#### Transformations Used:

1. **Source Qualifier (filter)**

   * Filters incremental data
   * Condition: `LastUpdated > $$Last_Run_Time`
   * Removes invalid records

---

2. **Expression ()**

   * Converts date format (MM/DD/YY → Date)
   * Calculates Stay Duration
   * Calculates Total Amount

   ```sql
   StayDuration =
   IIF(ISNULL(CheckoutDateTime),1,
       DATEDIFF(v_Checkout,v_Checkin))

   TotalAmount =
   (RoomRate * StayDuration)
   + NVL(ExtraCharges,0)
   - NVL(DiscountAmount,0)
   ```

---

3. **Lookup (LKP_GUEST, LKP_ROOM)**

   * Fetch Guest and Room details
   * Improves data enrichment

---

4. **Target: GUEST_STAY**

   * Central fact table used for analytics

---

## 📊 Analytical Use Cases

---

###

---

### 🟡 1. Room-Type Performance

**KPIs:**

* Total Bookings
* Revenue per Room Type
* Average Daily Rate (ADR)

---

### 🔵 2. Stay Duration Analysis

**KPIs:**

* Avg Stay Duration
* Min/Max Stay
* Total Guest Nights

---

### 🔴 3. High-Value Guest Analysis

**KPIs:**

* Total Revenue per Guest
* Visit Count
* Avg Spend

---

## 🧠 Key Concepts Used

* Incremental Load using `LastUpdated`
* Expression Transformation for business logic
* Lookup Transformation for dimension enrichment
* Aggregator Transformation for KPIs
* Parameterization (`$$Last_Run_Time`)
* Data validation using flags

---

## ⚙️ Taskflow Design

```
Start
  ↓
M_LOAD_GUEST_STAY
  ↓
Decision Task
  ↓
Success → Run all 4 mappings
Failure → Send Email Notification
  ↓
End
```

---

## 🚀 Technologies Used

* Informatica (IICS / PowerCenter)
* SQL
* CSV Data Processing

---

## 📈 KPIs Implemented

* Occupancy Rate
* Total Revenue
* Average Daily Rate (ADR)
* Customer Lifetime Value
* Stay Duration Metrics

---

## ❗ Challenges & Solutions

| Challenge              | Solution                      |
| ---------------------- | ----------------------------- |
| Date format (MM/DD/YY) | Used `TO_DATE()` conversion   |
| Incremental loading    | Used `LastUpdated` parameter  |
| Duplicate handling     | Used proper filtering         |
| Performance            | Used Lookup instead of Joiner |

---

## 🏆 Key Learnings

* Real-world ETL pipeline design
* Data transformation logic
* Performance optimization
* Business KPI generation

---

## 📌 Future Enhancements

* Dashboard integration (Power BI/Tableau)
* Real-time data processing
* Email alert system
* Error logging framework

---

## 👨‍💻 Author

**Data Knights**
**Ansh Tiwari**
B.Tech CSE | MERN Stack Developer | Data Analyst

---

