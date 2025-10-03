# Fitness Analytics Dashboard (Power BI)

A polished Power BI dashboard for a gym/fitness business that tracks members, trainers, finance (revenue, expenses, profit), and engagement, plus a built‑in calorie/BMR/TDEE calculator. See **docs/GYM.pdf** for page mockups and example outputs. fileciteturn1file0

## ✨ Highlights
- **Executive KPIs:** Clients, Trainers, Revenue, Expenses, Profit with monthly trend visuals. fileciteturn1file0
- **Membership Insights:** Active vs. Expired counts and membership plan mix (Platinum/Gold/Silver). fileciteturn1file0
- **Members Hub:** Filterable member table (UserName, Age, Gender, Goal, JoinDate, Status, BMI, Progress %). fileciteturn1file0
- **Calorie Calculator:** BMR/TDEE, BMI category, and calorie targets (maintain/deficit tiers). fileciteturn1file0

## 🧭 Report Pages
### 1) Home / Overview
- KPI cards: **Clients**, **Trainers**, **Revenue**, **Expenses**, **Profit**.
- Finance trend: monthly **Revenue/Expenses/Profit** chart.
- **Active vs. Expired** membership summary.
- **Membership plan** distribution: Platinum / Gold / Silver.
- Footer elements include “**FitnessDashboard – Last update**” time stamp. fileciteturn1file0

### 2) Members
- Member table with: **UserName, Age, Gender, Goal, JoinDate, Status, Progress % (bar), BMI**. fileciteturn1file0
- **Members by age and gender** visuals for buckets: **18–25, 25–40, 40–60** split by **Female/Male**. fileciteturn1file0
- Slicers for **Status** (Active/Expired) and **Gender**. fileciteturn1file0

### 3) Calculator
- Inputs: **Age, Weight, Height, Activity level, Gender**.
- Outputs: **BMI** (with category), **BMR**, **TDEE**, and calorie targets (Maintain, Mild/Standard/Extreme loss). fileciteturn1file0

## 🗃️ Data Model
**Fact tables**
- `factRevenue` (Date, Amount, Type)
- `factExpenses` (Date, Amount, Category)

**Dimension tables**
- `dimDate` (Date, Year, Month, MonthNo, Quarter, etc.)
- `dimMember` (**MemberID**, **UserName**, **Age**, **Gender**, **Goal**, **JoinDate**, **Status**, **Plan**, **Height**, **Weight**, **BMI**, **ProgressPct**)
- `dimTrainer` (TrainerID, Name)

> The Members page indicates columns such as *UserName, Age, Gender, Goal, JoinDate, Status, BMI,* and a progress bar; align your schema accordingly. fileciteturn1file0

## 🔢 Key Measures (DAX samples)
```DAX
Revenue := SUM ( factRevenue[Amount] )
Expenses := SUM ( factExpenses[Amount] )
Profit := [Revenue] - [Expenses]
Profit Margin % := DIVIDE ( [Profit], [Revenue] )

Clients := DISTINCTCOUNT ( dimMember[MemberID] )
Trainers := DISTINCTCOUNT ( dimTrainer[TrainerID] )

Active Members :=
CALCULATE (
    DISTINCTCOUNT ( dimMember[MemberID] ),
    KEEPFILTERS ( dimMember[Status] = "Active" )
)

Expired Members :=
CALCULATE (
    DISTINCTCOUNT ( dimMember[MemberID] ),
    KEEPFILTERS ( dimMember[Status] = "Expired" )
)

Active % :=
DIVIDE ( [Active Members], [Active Members] + [Expired Members] )

Members by Plan (e.g., Platinum) :=
CALCULATE (
    DISTINCTCOUNT ( dimMember[MemberID] ),
    KEEPFILTERS ( dimMember[Plan] = "Platinum" )
)

BMI (per member) :=
VAR h = SELECTEDVALUE ( dimMember[Height] ) / 100.0
VAR w = SELECTEDVALUE ( dimMember[Weight] )
RETURN DIVIDE ( w, h * h )
```

> Rename columns in the DAX above to match your dataset exactly. The KPI/card and members table structures mirror the visuals shown in the PDF. fileciteturn1file0

## 🧮 Calculator Logic (reference)
- **BMI** = *weight(kg) / height(m)^2* → categorize (Underweight, Normal, Overweight, Obese).
- **BMR (Mifflin–St Jeor)**:  
  - Male: `10×kg + 6.25×cm – 5×age + 5`  
  - Female: `10×kg + 6.25×cm – 5×age – 161`
- **TDEE** = `BMR × ActivityMultiplier` (e.g., 1.2 sedentary → 1.9 very active)
- Targets: Maintain = `TDEE`; Mild/Standard/Extreme loss = `TDEE – {~250, ~500, ~750+}` kcal/day (adjust to your guidance).

> The Calculator page displays the inputs and outputs (BMI/BMR/TDEE and calorie targets) with activity selection and gender toggle. fileciteturn1file0

## 📂 Project Structure
```
/PowerBI
  ├─ FitnessDashboard.pbix            # Main Power BI report
  ├─ data/                            # Source data (CSV/Excel/DB extracts)
  ├─ docs/
  │   └─ GYM.pdf                      # Exported mockups / report preview
  └─ images/                          # Optional screenshots used in docs/README
```
> The provided PDF includes **Home**, **Members**, and **Calculator** pages and sample KPIs/visuals. fileciteturn1file0

## ⚙️ Setup & Usage
1. Open `FitnessDashboard.pbix` in **Power BI Desktop**.
2. Update **Data source settings** and credentials to your environment.
3. Create/validate relationships between `fact*` and `dim*` tables.
4. Refresh the model (Home → **Refresh**).
5. Use slicers (Status, Gender, Age buckets, Plans) and navigate via the page tabs (Home, Members, Calculator). fileciteturn1file0

## 📈 Visuals Included
- KPI cards (Clients, Trainers, Revenue, Expenses, Profit) and monthly trend charts.
- Active vs. Expired membership chart and plan distribution (Platinum/Gold/Silver).
- Members by Age & Gender column charts (18–25, 25–40, 40–60).
- Member detail table (UserName, Age, Gender, Goal, JoinDate, Status, Progress %, BMI).
- Calorie/BMR/TDEE calculator panel. fileciteturn1file0

## 🔐 Data & Privacy
- Replace all sample/mock data with governed sources.
- Review and implement **RLS** if required before publishing.
- Avoid embedding personal data in screenshots.

## 🚀 Publish
- Publish to Power BI Service (Home → **Publish**).
- Configure **Scheduled refresh** for cloud-accessible sources.
- Share via **App** or report **link**, with appropriate permissions.

## 🧪 Validation Checklist
- [ ] KPI totals match the source of truth
- [ ] Month sorting and date hierarchy verified
- [ ] Active vs. Expired reconciles with membership system
- [ ] Calculator outputs validated against known formulas
- [ ] Tooltips/drill-through (if any) behave as expected


