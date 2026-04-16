# Texas Property Tax Protest Skill for Claude Code

This skill helps Claude turn your property address into a complete tax protest package: research the appraisal, find comparable sales, build the evidence, and generate ready-to-file documents.

## What It Does

- Looks up your property on the county appraisal district (CAD) website
- Checks if you're missing exemptions (homestead, disabled veteran, over-65)
- Finds comparable properties that sold for less than your appraised value
- Adjusts comps for size, age, condition, and builder quality differences
- Generates a professional evidence package:
  - **Comp analysis spreadsheet** (`.xlsx`) with adjusted values and market data
  - **Protest letter** (`.docx`) addressed to your county's CAD
  - **Filing checklist** (`.md`) with step-by-step instructions and hearing tips

## Supported Counties

Works for any Texas county. Includes quick-reference info for: Denton, Collin, Tarrant, Dallas, Harris, Travis, and Bexar.

## Disabled Veteran Support

If you have a VA disability rating, the skill flags the disabled veteran property tax exemption (Tax Code §11.22). At 100% disability, Texas provides a **total property tax exemption**.

| VA Rating | Exemption |
|-----------|-----------|
| 10-29% | $5,000 |
| 30-49% | $7,500 |
| 50-69% | $10,000 |
| 70-99% | $12,000 |
| 100% | **$0 property taxes** |

## Installation

Ask Claude to use the skill at [github.com/ChallengerA91/tx-property-tax-protest](https://github.com/ChallengerA91/tx-property-tax-protest).

Or clone it:
```bash
git clone https://github.com/ChallengerA91/tx-property-tax-protest.git
```

Then point Claude at your property and say:

```text
Protest my property taxes at 1234 Main St, Denton TX
```

## What You Get

- Filled comp spreadsheet in `output/`
- Ready-to-file protest letter
- Step-by-step filing checklist with exemption guidance
- Estimated tax savings breakdown

## Key Deadlines

- **Protest deadline**: May 15 each year (or 30 days after your appraisal notice)
- File online through your county's CAD website, or mail Form 50-132

## Contributing

Contributions welcome via PR — especially adding county-specific details or improving comp research strategies.
