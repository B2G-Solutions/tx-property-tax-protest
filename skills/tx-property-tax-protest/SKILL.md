---
name: tx-property-tax-protest
description: >
  Protest property tax appraisals in Texas. Use this skill whenever the user mentions
  property taxes, tax protest, appraisal protest, CAD, property appraisal, or wants to
  lower their property tax bill. Also triggers on "my property taxes are too high",
  "protest my appraisal", "fight my property taxes", "reduce property taxes",
  "comparable properties", "homestead exemption", or "disabled veteran exemption". This
  skill researches the property, finds comps, builds an evidence package, and generates
  a protest-ready letter and spreadsheet. Works for any Texas county.
user_invocable: true
triggers:
  - protest property taxes
  - property tax protest
  - fight property taxes
  - lower property taxes
  - appraisal protest
  - property appraisal too high
  - comparable properties for protest
  - homestead exemption
  - disabled veteran exemption
---

# Texas Property Tax Protest Skill

Help the user protest their property tax appraisal with their county's Central Appraisal
District (CAD). The goal is a complete evidence package: comp analysis spreadsheet, formal
protest letter, filing checklist, and exemption guidance.

## Why This Matters

Texas has no state income tax, so property taxes are high — often 2-3% of home value
annually. The county appraisal district sets your home's "market value" each January 1,
and your tax bill is based on that number. But appraisals are often inflated. Texas law
gives every property owner the right to protest, and most protests result in some
reduction. Even a $20,000 reduction saves roughly $400-600/year. The process is free.

## Key Deadlines

- **Appraisal notices** mailed: mid-April
- **Protest deadline**: May 15 (or 30 days after the notice date, whichever is later)
- **Informal hearings**: typically May-June
- **Formal ARB hearings**: June-August

## Workflow

### Step 1: Identify the County and Gather Property Details

Ask the user for:
1. **Property address** or **CAD account number**
2. **County** (if not obvious from the address)
3. The **current appraised value** from their notice (or "I don't know")
4. Any known issues (deferred maintenance, foundation, flood zone, noise, etc.)
5. **Veteran status** — ask if they are a disabled veteran (significant TX exemptions)
6. **Age** — ask if they are 65 or older (additional exemptions available)

Look up the property on the county's CAD website. Record: account number, legal
description, appraised value, land value, improvement value, square footage, year built,
bedrooms/bathrooms, lot size. **Check the exemptions field** — flag if homestead or
veteran exemptions are missing.

Save all property details to `work/property_details.txt`.

### Step 2: Check Exemption Status (Often Worth More Than the Protest)

**Homestead Exemption** (Tax Code §11.13):
- $100,000 exemption from school district taxes
- 10% annual cap on appraisal increases
- Requires: TX driver's license + vehicle registration at the property address

**Disabled Veteran Exemption** (Tax Code §11.22):

| VA Disability Rating | Property Tax Exemption |
|---|---|
| 10-29% | $5,000 off assessed value |
| 30-49% | $7,500 off assessed value |
| 50-69% | $10,000 off assessed value |
| 70-99% | $12,000 off assessed value |
| 100% | **TOTAL exemption — $0 property taxes** |

- Requires: VA disability rating letter, DD-214, TX driver's license
- Stacks with homestead exemption
- Surviving spouse may also qualify

**Over-65 Exemption** (Tax Code §11.13(c)):
- Additional $10,000 exemption from school taxes + tax ceiling freeze

If any exemptions are missing, flag this prominently.

### Step 3: Research Comparable Sales (Comps)

Find similar homes nearby that sold for less than the appraisal. "Similar" means:
- Within 1 mile (same subdivision ideal), within 20% sqft, within 10 years age

**Where to find comps:**
1. County CAD website — nearby properties, sale prices and appraised values
2. Web search for recent sales in the subdivision and zip code
3. Redfin / Zillow — filter by "Recently Sold"
4. MLS access if the user has it

Record for each comp (aim for 5-8): address, sale date, sale price, sqft, year built,
beds/baths, lot size, distance, price per sqft, condition notes.

Prioritize comps that sold BELOW the appraised value. Include 1-2 near or above for
credibility. Save to `work/comps_data.txt`.

### Step 4: Analyze and Adjust Comps

Calculate $/sqft for each. Make adjustments for size (~$50-80/sqft), age, pool
(+$15-25K), garage (+$10-20K), builder quality, and condition. Calculate median and
average adjusted values — this is the "indicated market value."

### Step 5: Build the Evidence Package

Generate in `output/`:

1. **`comp_analysis.xlsx`** — Professional spreadsheet with comp table, summary section
   (median, average, CAD value, argued value, potential reduction), and market notes.

2. **`protest_letter.docx`** — Formal letter addressed to the CAD with property info,
   comp summary table, opinion of value, and reduction request. Professional, factual,
   concise (1-2 pages).

3. **`filing_checklist.md`** — Step-by-step: how to file, what to bring, hearing tips,
   exemption filing instructions, estimated savings breakdown.

### Step 6: Present Results

Show summary: current value, argued value, potential reduction, estimated savings
(including exemption savings). Walk through next steps.

## Protest Strategies

1. **Market value** (§41.43(b)(1)) — comps sold for less than your appraisal
2. **Unequal appraisal** (§41.43(b)(3)) — higher $/sqft appraisal than neighbors
3. **Description errors** — wrong sqft, bathrooms, year built
4. **Condition issues** — foundation, flood zone, repairs needed (bring photos)

## Hearing Tips

- **Informal**: One-on-one, friendly. Most reductions happen here. Bring printed comps.
- **Formal ARB**: 3-person panel, 15 minutes. Lead with strongest comps. Bring 4 copies.
- **Say**: "Based on comparable market data, I believe the market value is $X, not $Y."
- **Don't**: get emotional, argue tax rates, or compare without data.

## Major Texas CAD Websites

| County | Website | Phone |
|--------|---------|-------|
| Denton | dentoncad.com | 940-349-3800 |
| Collin | collincad.org | 469-742-9200 |
| Tarrant | tad.org | 817-284-0024 |
| Dallas | dallascad.org | 214-631-0910 |
| Harris | hcad.org | 713-957-7800 |
| Travis | traviscad.org | 512-834-9317 |
| Bexar | bcad.org | 210-242-2432 |

**Protest form**: Form 50-132 — comptroller.texas.gov/taxes/property-tax/forms/
