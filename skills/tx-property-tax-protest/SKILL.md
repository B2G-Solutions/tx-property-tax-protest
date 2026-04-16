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
District (CAD). Produce a complete evidence package: comp analysis spreadsheet, formal
protest letter, filing checklist, and exemption guidance.

## Why This Matters

Texas has no state income tax, so property taxes are high — often 2-3% of home value.
The county appraisal district sets your home's "market value" each January 1 and your
tax bill is based on that number. Appraisals are often inflated. Texas law gives every
property owner the right to protest, and most protests result in some reduction. Even a
$20,000 reduction saves ~$400-600/year. The process is free and low-risk.

## Folder Structure

```
working_dir/
  work/                  ← intermediate files
    property_details.txt ← subject property info from CAD
    comps_data.txt       ← comparable sales data
  output/                ← final deliverables
    comp_analysis.xlsx   ← spreadsheet with comp table and summary
    protest_letter.docx  ← ready-to-file formal letter
    filing_checklist.md  ← step-by-step guide with exemption info
```

Create `work/` and `output/` at the start. Keep the working directory clean.

## Dependencies

Install before generating the evidence package:
```bash
pip install openpyxl python-docx
```

## Key Deadlines

- **Appraisal notices** mailed: mid-April
- **Protest deadline**: May 15 (or 30 days after the notice date, whichever is later)
- **Informal hearings**: May-June
- **Formal ARB hearings**: June-August
- If past the deadline, check whether the user qualifies for a late protest (errors,
  late notice delivery, etc.)

## Workflow

### Step 1: Gather Info from the User

Ask the user for ALL of the following before proceeding:
1. **Property address** or **CAD account number**
2. **County** (if not obvious from the address)
3. The **current appraised value** from their notice (or "I don't have it")
4. **What they paid** for the property and when (purchase price is the strongest
   evidence of market value — especially for recent purchases within 1-2 years)
5. Any known issues (deferred maintenance, foundation, flood zone, road noise,
   needed repairs, nearby commercial/industrial, HOA issues)
6. **Veteran status** — are they a disabled veteran? What VA disability rating %?
7. **Age** — are they 65 or older?

### Step 2: Look Up the Property on the County CAD Website

Navigate to the county's CAD website using the browser. Search by address.

**Record all of these** (save to `work/property_details.txt`):
- CAD account/property ID
- Legal description (subdivision, block, lot)
- Current year appraised value (land + improvement breakdown)
- Square footage (living area AND gross building area)
- Year built
- Bedrooms and bathrooms (from improvement features section)
- Lot size (acres and square feet)
- Foundation type, roof, exterior
- Garage type and size
- **Value history** (prior year values — shows appraisal trend)
- **Exemptions on file** — CHECK THIS CAREFULLY
- **Protest status** — has one already been filed?
- Taxing units and tax rates

**Calculate price per square foot**: appraised value / living area sqft.

### Step 3: Check Exemptions (Often Worth MORE Than the Protest)

Compare the CAD exemption field against what the user qualifies for. Missing
exemptions should be flagged prominently — they can save thousands per year.

**Homestead Exemption** (Tax Code §11.13):
- $100,000 exemption from school district taxes (as of 2023 amendment)
- 10% annual cap on appraisal increases (huge for long-term savings)
- Additional local exemptions vary by taxing unit
- Requires: TX driver's license + vehicle registration at the property address
- Can be filed any time; applies to Jan 1 of the year filed

**Disabled Veteran Exemption** (Tax Code §11.22):

| VA Rating | Exemption |
|-----------|-----------|
| 10-29% | $5,000 off assessed value |
| 30-49% | $7,500 off assessed value |
| 50-69% | $10,000 off assessed value |
| 70-99% | $12,000 off assessed value |
| 100% | **TOTAL exemption — $0 property taxes** |

- Stacks with homestead exemption
- Requires: VA disability rating letter or benefit summary, DD-214, TX driver's license
- Surviving spouse of deceased disabled veteran may also qualify (§11.22(h))
- If rating increases later, the user can refile for the higher exemption

**Over-65 / Disabled Exemption** (Tax Code §11.13(c)-(d)):
- Additional $10,000 exemption from school taxes
- School tax ceiling (freeze) — school taxes can never increase above the
  amount in the year you turned 65 or became disabled
- Portable: transfers to a new homestead (adjusted proportionally)

### Step 4: Research Comparable Sales (Comps)

This is the core of the protest. Find similar homes that sold for less than the
appraised value. Use MULTIPLE sources — no single source is complete.

**Source 1: CAD Website (Neighbor Appraisals)**
- Search for other properties on the same street / in the same subdivision
- Record their appraised values, sqft, year built
- This supports the **unequal appraisal** argument (your $/sqft vs theirs)

**Source 2: Redfin / Zillow (Market Estimates + Sold Data)**
- Search for the subject property on Redfin — record the Redfin Estimate
- Check the "Sale & Tax History" tab for the user's purchase price
- Search for "Recently Sold" homes nearby with similar characteristics
- Record Zillow Zestimate and Zestimate trend (% change over 1-3 years)
- A declining trend strengthens the argument that the CAD overvalued

**Source 3: Web Search (Recent Sales)**
- Search: `"[subdivision name]" [city] TX sold [year] price`
- Search: `[zip code] Denton TX homes sold [year] 3 bedroom 2000 sqft`
- Look for actual closing prices, not just listing prices

**Source 4: MLS (if the user has access)**
- Pull comps from NTREIS Matrix or their local MLS
- MLS data is the most authoritative source for sale prices

**What makes a good comp:**
- Same subdivision (best) or within 1 mile
- Within 20% of square footage
- Within 10 years of year built
- Similar bed/bath count and condition
- Sold within 12 months of January 1 of the tax year
- Sale was arm's-length (not foreclosure, family transfer, or auction)

**Record for each comp** (aim for 5-8):
- Address
- Sale price and sale date (or Zestimate if sale price unavailable)
- Square footage, year built, beds/baths, lot size
- Distance from subject property
- Price per square foot
- Condition notes (pool, renovation, builder quality)
- Source (CAD, Redfin, Zillow, MLS)

**Also record market-wide data points:**
- Zip code median sold price (from Redfin or Zillow market reports)
- Redfin Estimate for the subject property
- Zillow Zestimate trend (% change)
- Any similar listings sitting unsold for months (shows market resistance)

Save everything to `work/comps_data.txt`.

### Step 5: Analyze and Adjust Comps

For each comp, calculate price per square foot. Then adjust for differences:
- **Size**: Smaller homes tend to have higher $/sqft; adjust ~$50-80/sqft for
  significant size gaps
- **Age**: Newer homes command a premium
- **Builder quality**: Premium builders (Toll Brothers, Highland) vs value builders
  (Impression, DR Horton) — adjust ~$20-40K
- **Pool**: +$15-25K if comp has pool and subject doesn't (or vice versa)
- **Garage**: +$10-20K per extra bay
- **Lot size**: Adjust if materially different
- **Condition**: Renovated vs original

Calculate the **median** and **average** of adjusted comp values. This is the
"indicated market value" — the number you argue the property is actually worth.

### Step 6: Build the Evidence Package

Generate three files in `output/` using a Python script:

#### 1. `comp_analysis.xlsx` (openpyxl)

Professional spreadsheet with:
- **Header section**: subject property address, CAD account, subdivision, sqft,
  beds/baths, year built, current appraised value, appraised $/sqft
- **Comp table** with columns: Address, Sale/List Price, Sq Ft, $/Sq Ft,
  Beds/Baths, Year Built, Distance, Adjustments, Adjusted Value
- **Summary section** (highlighted): median adjusted value, average adjusted value,
  CAD appraised value, argued market value, potential reduction
- **Notes section**: zip median, Redfin/Zillow estimates, market trends, unsold
  listings that show market resistance
- Professional formatting: header fills, borders, number formats, column widths

#### 2. `protest_letter.docx` (python-docx)

Formal 1-2 page letter:
- Addressed to the county's Central Appraisal District (use correct address)
- References property ID and address
- States the protest of the current appraised value
- Cites protest basis: Market Value (§41.43(b)(1))
- Includes a comp summary table (address, price, sqft, $/sqft)
- Lists key arguments as bullet points (market estimates, zip median, trends,
  unsold comps, condition issues)
- States the owner's opinion of value (the median adjusted comp value)
- Requests reduction to the argued value
- Professional, factual tone — data over emotion
- Signature block with owner name and address

#### 3. `filing_checklist.md`

Step-by-step guide covering:
- Filing deadline and how to file (online, mail, in person) for the specific county
- What documents to bring to the informal hearing
- Hearing tips and what to expect
- Formal ARB hearing process if informal doesn't work
- **Exemption filing instructions** — homestead, veteran, over-65 (with what documents
  are needed for each)
- **Estimated savings breakdown** — table showing savings from protest + each exemption

### Step 7: Present Results

Show a clean summary:

```
PROPERTY TAX PROTEST SUMMARY
=============================
Subject:               [address]
CAD Account:           [number]
Current Appraised:     $XXX,XXX
Argued Market Value:   $XXX,XXX
Potential Reduction:   $XX,XXX

Estimated Annual Savings:
  Protest:             $XXX - $XXX
  Homestead Exemption: $X,XXX - $X,XXX  [if missing]
  Veteran Exemption:   $XXX - $XXX      [if applicable]
  TOTAL:               $X,XXX - $X,XXX

Evidence Package:
  output/comp_analysis.xlsx
  output/protest_letter.docx
  output/filing_checklist.md

DEADLINE: May 15, [year]
```

Then walk the user through the immediate next steps (file protest, file exemptions).

## Protest Strategies

1. **Market value** (§41.43(b)(1)) — comparable properties sold for less than your
   appraisal. The primary strategy. Use actual sale prices, not listing prices.

2. **Unequal appraisal** (§41.43(b)(3)) — your property is appraised at a higher
   $/sqft than similar properties in the same area. Compare CAD appraisals of
   neighbors, not sale prices. Often easier to prove because you're using the
   district's own data against them.

3. **Errors in property description** — wrong sqft, extra bathrooms counted,
   incorrect year built, wrong lot size. Check CAD records line by line.

4. **Condition issues** — foundation problems, flood zone, needed repairs, road
   noise, power lines, adjacent commercial. Bring dated photos.

5. **Recent purchase price** — if the user bought within the last 1-2 years, the
   purchase price is the strongest single data point. The CAD's own guidelines
   treat arm's-length transactions as the best evidence of market value.

## Hearing Tips

- **Informal hearing**: One-on-one with an appraiser. Be friendly and professional.
  Most reductions happen here (~85% of protests settle informally). Bring your comp
  spreadsheet printed. The appraiser will counter-offer. Know your bottom line
  before you walk in. You can accept on the spot or decline and go to formal.

- **Formal ARB hearing**: 3-person citizen panel. More structured. You get ~15
  minutes to present. Lead with your strongest 3 comps, not all 8. Bring 4
  printed copies of everything (3 for panel + 1 for you). The panel votes.

- **Key phrase**: "Based on comparable market data, I believe the market value of
  my property as of January 1 is $[your number], not $[their number]."

- **Don't**: get emotional, argue about tax rates (ARB only controls appraised
  value), compare to neighbors without data, or badmouth the appraiser.

- **Do**: bring your purchase contract (if recent), photos of condition issues,
  printouts of Redfin/Zillow estimates, and the comp spreadsheet.

## Major Texas CAD Websites

| County | Website | Phone |
|--------|---------|-------|
| Denton | dentoncad.com | 940-349-3800 |
| Collin | collincad.org | 469-742-9200 |
| Tarrant | tad.org | 817-284-0024 |
| Dallas | dallascad.org | 214-631-0910 |
| Harris (Houston) | hcad.org | 713-957-7800 |
| Travis (Austin) | traviscad.org | 512-834-9317 |
| Bexar (San Antonio) | bcad.org | 210-242-2432 |
| Williamson | wcad.org | 512-930-3787 |
| Fort Bend | fbcad.org | 281-344-8623 |
| Montgomery | mcad-tx.org | 936-756-3354 |

**Protest form**: Form 50-132 (Notice of Protest) —
comptroller.texas.gov/taxes/property-tax/forms/
