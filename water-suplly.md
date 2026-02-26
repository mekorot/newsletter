# Prompt: Israeli Water Tariff Price Calculator – Static HTML Web App

---

## 🌐 Language Requirement — CRITICAL

> **The entire user interface must be written in Hebrew.** This includes — without exception — all headings, labels, button text, dropdown options, placeholder text, tooltips, error messages, section titles, table headers, footer text, and any instructional copy. The `<html lang="he">` attribute must be set, and `direction: rtl; text-align: right` must be applied globally via CSS. Variable names, JSON keys, and code comments may remain in English.

---

## 🎯 Role & Task

You are a senior full-stack developer and Israeli water-law domain expert. Your task is to build a **single-file, static HTML web application** — with all CSS and JavaScript embedded —  <u> that calculates Israeli water supply prices and related fees for a specific client, based on the three Israeli Water Authority regulations provided below.</u>

The app must be **polished, professional, and production-grade**. It is intended for use by accountants, municipal clerks, local water corporation staff, or end consumers who need to understand their water bill.

---
<u>
## 📚 Regulatory Context (Source Documents Summary)
</u>
<u>
The three source documents are Israeli Water Authority regulations (כללי המים), retrieved from nevo.co.il. They are:
  
</u>
<u>
### Document 1 — `501_628__36_.doc`
**כללי המים (חישוב עלויות ותעריפים להפקה ולהולכה), תשע"ז-2017*</u> 
<u>
*
*Water Rules (Calculation of Costs and Tariffs for Extraction and Transportation), 2017*
- Governs tariffs that **license holders** (water producers) pay for extraction (הפקה) and transportation (הולכה).
- Relevant to producers who drill wells (קידוחים), extract surface water, desalinate water, etc.
- **Current tariffs effective 1.1.2026 (תשפ"ו-2025):**
</u>  


| Type | Uniform Extraction Tariff (base) | Basic Extraction Add-on | Drinking Water Add-on |
|------|----------------------------------|-------------------------|-----------------------|
| Borehole (קידוח) | **₪0.90/m³** | **₪0.54/m³** | **₪0.02/m³** |
| Surface water, small producer (≤250,000 m³/year) | **₪0.33/m³** | — | — |
| Surface water, large producer (>250,000 m³/year) | **₪0.11/m³** | — | — |
| Desalinated water (self-use) | **≈₪1.353/m³** (previous; 2026 update expected) | — | — |

- **Formula for Basic Quantity of Hours (כמות שעות בסיסית):**
  - For boreholes built before 31.12.2022 (up to 50 years old): **1,000 hours**
  - For new boreholes: H (recognition hours) = `√(D × Q)` where D = borehole depth, Q = recognized extraction capacity (m³/hr); urban boreholes get a **×1.15 multiplier**
- **Drill Depth Coefficient (מקדם עומק קידוח):**
  - If depth ≤ 175m → coefficient = 1
  - If depth > 175m → coefficient = `(depth / 175) ^ 0.7`
- **Recognized Extraction Capacity Coefficient (מקדם כושר הפקה מוכר):**
  - If capacity ≤ 100 m³/hr → coefficient = capacity (as-is)
  - If capacity > 100 m³/hr → coefficient = `(capacity - 100) × (100 / capacity) + 100` × depth_coefficient, rounded to nearest 0 or 5

---

### Document 2 — `235_071__20_.doc`
**כללי המים (תעריפי מים שמספקים ספקים מקומיים), תשנ"ד-1994**
*Water Rules (Water Tariffs Supplied by Local Suppliers), 1994*
- Governs tariffs that **local water suppliers** (municipalities, water corporations / תאגידי מים) charge **end consumers**.
- Tariff structure references כללי תאגידים (Water Corporation Rules) for the actual NIS amounts for residential tiers.
- Key structures:

**Residential (צריכה ביתית), per dwelling unit (יחידת דיור):**
- **Tier 1 (כמות מוכרת / Recognized Quantity):** 3.5 m³/person/month
  - Tariff = כללי תאגידים Section 3(1)(א) → **≈ ₪8.90/m³ incl. VAT** (water + sewage combined, current 2026)
- **Tier 2 (מעבר לכמות מוכרת / Above Recognized):**
  - Tariff = כללי תאגידים Section 3(1)(ב) → **≈ ₪16.30/m³ incl. VAT** (water + sewage combined, current 2026)

**Other consumption (צריכה אחרת):**
- Regular consumer: same as Tier 2 residential
- **Large consumer (צרכן גדול):** discount tiers:
  - 15,001–250,000 m³/year: discount of **₪1.10/m³**
  - Above 250,000 m³/year: discount of **₪1.35/m³**
- Water-only consumers (no sewage connection): reduced tariff

**Agriculture (מטרת חקלאות):** see Document 3 tariffs (Section 4(3) references כללי מקורות)

**Special uses:** Hospitals, bathhouses, mikvaot — special tariff

**Exceedance (כמות נחרגת):** Consumption above licensed allocation → penalty tariffs apply

---

### Document 3 — `235_057__46_.doc`
**כללי המים (תעריפי מים המסופקים מאת מקורות), תשמ"ז-1987**
*Water Rules (Water Tariffs Supplied by Mekorot), 1987*
- Governs tariffs that **Mekorot** (מקורות חברת מים בע"מ, Israel's national water company) charges its customers (local suppliers, cooperatives, etc.).
- Uses tariff codes **R1** and **R2**:

**Current tariffs effective 1.1.2026 (תשפ"ו-2025):**

| Tariff Code | Purpose | Rate (₪/m³, excl. VAT) |
|-------------|---------|------------------------|
| **R1** | Domestic use, recognized quantity (כמות מוכרת) | ≈ **₪7.50** |
| **R2** | Domestic use, above recognized / other uses | ≈ **₪13.90** |
| Agriculture – fresh water (שפירים) – Zone A (מעלה) | חקלאות | ≈ **₪3.40** |
| Agriculture – fresh water – Zone B standard | חקלאות | ≈ **₪2.90** |
| Agriculture – fresh water – Zone C (מורד) | חקלאות | ≈ **₪2.40** |
| Agriculture – brackish water (מליחים/נחותים) | חקלאות | ≈ **₪1.20** |
| Agriculture – recycled water (קולחין/שפד"ן) | חקלאות | ≈ **₪0.80** |
| Industry (תעשייה) – standard | תעשייה | ≈ **₪9.20** |
| Injected water (מים מוחדרים) | מוחדרים | special formula |

- **Excess consumption (כמות נחרגת) penalty:**
  - Penalty rate = **3× the applicable base tariff** for the excess quantity

> **Note on VAT (מע"מ):** Most tariffs in Document 3 (Mekorot) are **excluding VAT** (17%). Tariffs in Documents 1 and 2 (local suppliers to end consumers) **include VAT**. The calculator must handle this clearly.
</mark>
---

## 🏗️ What to Build

A **single HTML file** (`water_tariff_calculator.html`) containing a complete, interactive price calculator with the following sections:

### 1. Client Setup Panel
Inputs to identify the specific client being calculated for:
- **Client Name** (text field)
- **Client Type** dropdown:
  - Individual/Household consumer (צרכן ביתי)
  - Local water supplier (ספק מקומי) buying from Mekorot
  - Water corporation (תאגיד מים)
  - License holder / water producer (בעל רישיון הפקה)
  - Cooperative / kibbutz (צרכן שיתופי)
  - Agricultural consumer (צרכן חקלאי)
  - Industrial consumer (צרכן תעשייתי)
- **Supplier Type** (for consumers): Municipal supplier (ספק מקומי רשותי) / Other local supplier / Mekorot direct
- **Location zone** (for agricultural): Upper zone (מעלה) / Standard / Lower zone (מורד) / Arava region
- **Billing period**: Monthly / Quarterly / Annual
- **Number of people in household** (for residential, affects recognized quantity)
- **Number of dwelling units** (for multi-unit buildings)
- VAT included toggle (yes/no)

### 2. Water Consumption Input Panel
- **Consumption type tabs**: Residential | Agricultural | Industrial | Other
- **Actual consumption** (m³, for the billing period)
- **Licensed/allocated quantity** (m³) — to detect exceedance
- For Residential: auto-calculated recognized quantity shown
- For Agricultural: water type selector (fresh/brackish/recycled) + zone
- For Industrial: large consumer checkbox (>15,000 m³/yr)
- For extraction license holders: borehole depth (m), recognized extraction capacity (m³/hr), urban location checkbox

### 3. Live Calculation Results Panel
Must show:
- A **step-by-step calculation breakdown** for this specific client
- Each calculation step must display:
  - The **formula** being used (rendered cleanly, e.g. `Total = Q₁ × R1 + Q₂ × R2`)
  - The **values substituted** into the formula
  - The **result** of that step
- Summary table:
  - Subtotals per tier / category
  - VAT amount (if applicable)
  - **Total amount due (₪)**
- Exceedance warning (highlighted in red) if consumption > allocation
- Comparison: "With VAT" vs "Without VAT" line

### 4. Tariff Reference Tables (stored in JSON, displayed as collapsible tables)
Include three collapsible sections — one per regulation — each rendering the relevant tariff table from embedded JSON data:

```json
// Example structure (you must populate with all actual rates above)
{
  "extraction_tariffs_2026": { ... },
  "local_supplier_tariffs_2026": { ... },
  "mekorot_tariffs_2026": { ... }
}
```
Each table must show: tariff name (Hebrew + English), tariff code, rate per m³, VAT inclusion status, effective date, and a **brief plain-language explanation** of when this tariff applies.

### 5. Term Glossary (tooltips)
Every technical Hebrew term used in the UI must have a **tooltip or info popover** explaining it in plain language (Hebrew preferred, bilingual acceptable). Required terms to define:
- כמות מוכרת (Recognized/Acknowledged Quantity)
- הפקה בסיסית (Basic Extraction)
- הפקה אחידה (Uniform Extraction)
- הפקת חורף (Winter Extraction)
- כמות נחרגת (Exceedance Quantity)
- ספק חד-רשותי / ספק רב-רשותי (Single/Multi-authority supplier)
- צרכן גדול (Large Consumer)
- קידוח (Borehole/Drill)
- כושר הפקה מוכר (Recognized Extraction Capacity)
- תעריף R1 / R2 (Mekorot tariff tiers)
- מע"מ (VAT)
- קולחין / שפד"ן (Recycled water / treated effluent)
- מים מליחים (Brackish water)
- מים נחותים (Inferior-quality water)
- הולכה (Transportation/conveyance of water)
- קרן שיקום (Rehabilitation Fund)

---

## 🎨 Design Requirements

- **Language**: **The entire UI must be in Hebrew** — all labels, buttons, headings, instructions, error messages, tooltips, table headers, and placeholder text must be written in Hebrew. RTL layout (`direction: rtl; text-align: right`) must be applied globally. Variable names and code comments may remain in English.
- **Font**: Use `Heebo` or `Rubik` from Google Fonts (excellent Hebrew support, modern feel)
- **Theme**: Clean, professional, government-adjacent but not boring — think **Israeli public tech portal**: white + deep teal (`#1B6B6B` or similar) + amber accent for warnings, subtle blue-grey for backgrounds. Professional and trustworthy.
- **Layout**: Responsive, card-based layout. Left sidebar or top tabs for navigation between sections.
- **Formula display**: Use a monospace or math-styled font for formulas. Show substituted values in a different color.
- **Tooltips**: Use `title` attributes **and** custom CSS tooltips with `?` icons (ⓘ) next to each term
- **Collapsible sections** for reference tables (use `<details>/<summary>`)
- **Print-friendly** CSS so the calculation report can be printed
- Real-time recalculation on any input change (no submit button needed, use `input` event listeners)
- Show **date of tariffs** prominently: "תעריפים בתוקף מ-1.1.2026"

---

## 🧮 Calculation Logic Requirements

### Residential Consumer Calculation
```
recognized_quantity = 3.5 m³ × num_persons × billing_months
tier1_qty = min(actual_consumption, recognized_quantity)
tier2_qty = max(0, actual_consumption - recognized_quantity)
tier1_cost = tier1_qty × R1_tariff  (≈₪8.90 incl. VAT)
tier2_cost = tier2_qty × R2_tariff  (≈₪16.30 incl. VAT)
total = tier1_cost + tier2_cost
```
Show warning if `actual_consumption > licensed_allocation` → exceedance tariff applies (3× base).

### Agricultural Consumer Calculation (Mekorot tariffs)
```
allocated_qty = licensed_quantity
consumed_qty = actual_consumption
normal_qty = min(consumed_qty, allocated_qty)
excess_qty = max(0, consumed_qty - allocated_qty)
normal_cost = normal_qty × zone_tariff
excess_cost = excess_qty × (zone_tariff × 3)   // penalty
total_excl_vat = normal_cost + excess_cost
total_incl_vat = total_excl_vat × 1.17
```

### Extraction License Holder Calculation
```
// Basic tariff
uniform_extraction = volume × 0.90  // ₪/m³, 2026
basic_extraction_addon = min(volume, basic_qty) × 0.54
drinking_water_addon = volume × 0.02  // if drinking water approved
management_costs = volume × 0.03  // (הוצאות ניהול ושונות) for extraction

// Borehole hours formula (new borehole)
H_recognition = sqrt(D × Q)  // D=depth, Q=capacity
H_urban = H_recognition × 1.15  // if urban

total_extraction = uniform_extraction + basic_extraction_addon + drinking_water_addon + management_costs
```

---

## ✅ Quality Standards & Constraints

1. **Single file only** — all CSS, JS, and HTML in one `.html` file. No external dependencies except Google Fonts CDN.
2. **No framework required** — vanilla JS is preferred for simplicity and portability.
3. **All tariff data must be stored in a JavaScript JSON object** at the top of the `<script>` section, clearly labeled, so it can be easily updated when tariffs change annually.
4. **Every formula displayed in the results must match the actual regulation** — do not invent formulas.
5. **Include a "Data Sources" footer** listing the three regulation names, their official numbers (501/628, 235/071, 235/057), and effective date (1.1.2026).
6. **Accessibility**: Use proper `<label for>` associations, ARIA labels where needed, sufficient color contrast.
7. **The calculation must be clearly labeled as being for a SPECIFIC client** — show the client's name in the results header: "חישוב עבור: [שם הלקוח]"
8. **Add a "Reset / New Client" button** that clears all inputs.
9. **Calculation history**: Keep a simple in-page log of the last 3 calculations (client name + total + timestamp) visible below the results.
10. **Print button** that opens the browser print dialog, showing only the results panel.

---

## 📝 Output

Produce the complete, working `water_tariff_calculator.html` file. It must be immediately usable by opening in any modern browser — no server, no build step, no npm.

Start with a brief internal comment block at the top of the HTML explaining the regulatory sources and last update date. Then write the full application.

---

## 🔍 Example Interaction Flow (to validate your implementation)

**Scenario: Residential consumer, 4 people, monthly bill, consumed 20 m³**
1. User selects: Client Type = "Individual/Household", 4 persons, billing = Monthly, consumption = 20 m³
2. System calculates:
   - Recognized quantity = 3.5 × 4 = 14 m³
   - Tier 1: 14 m³ × ₪8.90 = ₪124.60
   - Tier 2: 6 m³ × ₪16.30 = ₪97.80
   - **Total = ₪222.40 (incl. VAT)**
3. Formula is displayed: `סה"כ = (14 × 8.90) + (6 × 16.30) = 124.60 + 97.80 = ₪222.40`
4. Breakdown table shows each line
5. Client name appears at top of results

---

*End of prompt. Build the complete application now.*
