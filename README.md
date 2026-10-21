# ✈️ Task 1: Lounge Eligibility Lookup Model

**British Airways Data Science Virtual Experience (Forage)**

Estimating how many passengers on each flight are likely to be eligible for airport lounge access, so lounge demand can be forecast for any future schedule.

---

## 📌 Project Overview

British Airways needs to plan lounge capacity at London Heathrow (LHR). This project analyses the **British Airways Summer Schedule dataset** and builds a simple, explainable **lookup table** that gives the expected share of passengers eligible for **Tier 1, Tier 2 and Tier 3** lounge access, by flight group.

The deliverable is an Excel workbook with:
1. a **Lounge Eligibility Lookup Table**, and
2. a **Justification** sheet explaining the approach, assumptions and scalability.

---

## 📊 Dataset

| Item | Detail |
|---|---|
| Source | British Airways Summer Schedule Dataset (Forage) |
| Flights | 10,000 |
| Departure airport | LHR (all flights) |
| Destinations | 15 airports across Europe, North America, the Middle East and Asia |
| Key columns | `HAUL`, `ARRIVAL_REGION`, `ARRIVAL_STATION_CD`, `AIRCRAFT_TYPE`, `FIRST_CLASS_SEATS`, `BUSINESS_CLASS_SEATS`, `ECONOMY_SEATS`, `TIER1/2/3_ELIGIBLE_PAX` |
| Missing values | None |

---

## 🧠 Approach

1. **Explored** eligibility rates across haul, region, destination, aircraft type and time of day.
2. **Found the strongest driver:** haul length. Short-haul flights have a higher eligible share than long-haul flights. Destinations within the same region behave almost identically.
3. **Grouped flights** by `HAUL` and `ARRIVAL_REGION`, which gives 4 groups.
4. **Calculated rates** for each group:

   `Tier % = total tier-eligible passengers ÷ total seats (First + Business + Economy)`

---

## 🏆 Results: Lounge Eligibility Lookup Table

| Grouping | Example Destinations | Tier 1 | Tier 2 | Tier 3 |
|---|---|---|---|---|
| Short-haul Europe | FRA, AMS, CDG, MAD, BCN, MUC, VIE, ZRH, IST | 0.34% | 4.40% | 16.85% |
| Long-haul North America | JFK, LAX, ORD, DFW | 0.20% | 2.76% | 10.55% |
| Long-haul Middle East | DXB | 0.21% | 2.69% | 10.29% |
| Long-haul Asia | HND | 0.19% | 2.70% | 10.35% |

### 💡 How to use it

For any flight, find its group, then multiply the seat count by the group percentage.

> **Example:** a 180-seat A320 to Europe → 180 × 16.85% ≈ **30 Tier 3 passengers**

---

## ✅ Why this grouping works

- **Data-driven:** haul length is the biggest differentiator in the data.
- **Simple and explainable:** easy to read, audit and share with non-technical teams.
- **Stable:** every group contains hundreds or thousands of flights, so averages are reliable.
- **Practical:** haul and region are known well in advance for any scheduled flight.

---

## 📝 Key Assumptions

- Eligibility is expressed as a share of **seat capacity**, because booked passengers are not in the data.
- The tier counts in the dataset are treated as correct.
- Passengers in a group behave like the group average. Time of day (Tier 3 stays between about 13.4% and 14.0%) and aircraft type are ignored for simplicity.
- Summer 2025 behaviour is assumed to represent future schedules.
- All flights depart from **LHR**, so the table does not apply to other departure airports.

---

## 🔮 Scalability

- **New or changed schedules:** tag each flight with its haul and region, look up the group rate, multiply by seats.
- **New destinations:** map into an existing group, or add a row if a new region appears.
- **Fresh data:** rates can be recalculated whenever new data arrives.
- **Possible refinement:** long-haul rates vary by aircraft (A380 about 6.4% vs B787 about 14% Tier 3), so aircraft type could be added as a further grouping level.

---

## 📁 Files

| File | Description |
|---|---|
| `Lounge_Eligibility_Lookup__Task1_.xlsx` | Completed lookup table and justification |
| `British_Airways_Summer_Schedule_Dataset.xlsx` | Source dataset (Forage) |

---

## 🛠️ Tools

Microsoft Excel (pivot tables and percentage calculations). The group rates can be reproduced in Python with `pandas`, using a `groupby` on `HAUL` and `ARRIVAL_REGION`.

---

## 👤 Author

*Jahangir Ali*, Data Science Virtual Experience, British Airways (Forage)
