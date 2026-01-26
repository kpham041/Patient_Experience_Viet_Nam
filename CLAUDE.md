# CLAUDE.md - AI Assistant Guide for Patient Experience Vietnam Project

## Project Overview

This is a **statistical analysis project** for analyzing patient experience data from Vietnamese hospitals using the **PPE-15 (Patient-Perceived Experience-15) survey instrument**. The project evaluates factors influencing patient satisfaction and hospital loyalty.

**Primary Language:** R (R Markdown)
**Data Format:** Excel (.xlsx)
**Output Format:** HTML reports, Word documents
**Target Region:** Vietnam

---

## Repository Structure

```
Patient_Experience_Viet_Nam/
├── README.md                      # Project description (minimal)
├── CLAUDE.md                      # This file - AI assistant guide
├── prepare_raw_data.Rmd           # Data cleaning and preprocessing
├── Analysis.Rmd                   # Statistical modeling and analysis
├── PPE_Analysis.Rmd               # Main analysis report (comprehensive)
├── PPE_Analysis.html              # Rendered HTML report output
├── 7_hospital_data.xlsx           # Raw hospital survey data
└── data_PPE_15_10_09_2025.xlsx    # Processed/cleaned PPE-15 data
```

---

## Key Files and Their Roles

### Data Files

| File | Description |
|------|-------------|
| `7_hospital_data.xlsx` | Raw survey data with patient demographics and experience responses (~3,500+ records) |
| `data_PPE_15_10_09_2025.xlsx` | Cleaned and processed PPE-15 dataset |

### R Markdown Files

| File | Purpose |
|------|---------|
| `prepare_raw_data.Rmd` | Data import, cleaning, variable encoding, factor creation |
| `Analysis.Rmd` | Statistical modeling (logistic & linear regression) |
| `PPE_Analysis.Rmd` | Main comprehensive analysis with publication-ready tables |

---

## PPE-15 Survey Structure

The project implements the PPE-15 survey with the following sections:

### Section A: Demographics (19 items)
- A1-A3: Age/birth year, gender
- A4-A9: Education, marital status, living situation, occupation, income, payment method
- A10-A12: Hospital, department, admission reason
- A13-A19: Length of stay, chronic disease, insurance, discharge condition, health perception

### Section B: Experience Items (15 items)
- B1-B9: Doctor/nurse communication, respectful treatment, shared concerns
- B10-B12: Pain management, family communication
- B13-B15: Medication instructions and warnings

### Section C: Satisfaction & Loyalty (6 items)
- C1: Overall experience score (0-10 numeric scale)
- C2-C5: Trust, recommendations, loyalty, intention to return
- C6: Reasons to return (multi-choice)

---

## Data Conventions

### Vietnamese Text Encoding

Survey responses use Vietnamese text that maps to numeric values:

**Frequency Scales:**
- "Không" (No) = 0
- "Thỉnh thoảng" (Sometimes) = 1
- "Luôn luôn" (Always) = 2

**Binary Responses:**
- "Không" (No) = 0
- "Có" (Yes) = 1

**Likert Scales (Health Perception):**
- "Rất tệ" (Very bad) = 1
- "Tệ" (Bad) = 2
- "Trung bình" (Average) = 3
- "Tốt" (Good) = 4
- "Rất tốt" (Very good) = 5

### Variable Naming Conventions

- `A1`, `A2`, ... `A19`: Demographic variables
- `B1`, `B2`, ... `B15`: Experience items
- `C1`, `C2`, ... `C6`: Satisfaction/loyalty items
- Suffix `_bin`: Binary encoded version of a variable
- Suffix `_cat`: Categorical grouped version

---

## R Package Dependencies

### Data Management
```r
readxl, writexl, janitor, dplyr, tidyr, stringr, lubridate, forcats
```

### Statistical Analysis
```r
MASS, car, olsrr, pscl, pROC, ResourceSelection, DHARMa, performance
```

### Visualization & Reporting
```r
ggplot2, gtsummary, gt, flextable, officer
```

### Utilities
```r
pacman, here, broom
```

---

## Development Workflow

### Running Analysis

1. **Data Preparation:**
   ```r
   # Open and knit prepare_raw_data.Rmd first
   rmarkdown::render("prepare_raw_data.Rmd")
   ```

2. **Statistical Analysis:**
   ```r
   # Run main analysis
   rmarkdown::render("PPE_Analysis.Rmd")
   ```

3. **Output:** HTML report generated as `PPE_Analysis.html`

### Rendering R Markdown

From command line:
```bash
Rscript -e "rmarkdown::render('PPE_Analysis.Rmd')"
```

---

## Statistical Methods Used

### Regression Models

- **Linear Regression:** For continuous outcome (experience score 0-10)
- **Logistic Regression:** For binary outcome (positive vs. negative experience)
- **Stepwise Selection:** Both directions for model optimization
- **Best Subset Regression:** For variable selection

### Model Diagnostics

- Cook's distance (outlier detection)
- Standardized residuals
- Leverage analysis
- Hosmer-Lemeshow test (logistic models)
- AUC/ROC analysis

### Performance Metrics

- R², Adjusted R², RMSE (linear models)
- Nagelkerke R², McFadden R², AUC (logistic models)
- Model comparison via AIC

---

## Code Style Conventions

### R Markdown Format
- YAML header with title, author, date, and output format
- Code chunks with descriptive names
- Liberal use of `pacman::p_load()` for package loading
- `here::here()` for path management

### Data Processing Style
- Pipe operators (`%>%` or `|>`) for data transformations
- `janitor::clean_names()` for column standardization
- Explicit factor level ordering with `forcats`
- Extensive regex for Vietnamese text cleaning

### Table Output
- `flextable` for publication-ready tables
- JAMA journal style formatting
- Consistent decimal places and formatting

---

## Important Notes for AI Assistants

### Language Considerations
- All survey data and many code comments are in **Vietnamese**
- Department names, occupation categories, and response options require Vietnamese text handling
- Use UTF-8 encoding when working with data files

### Data Sensitivity
- This is healthcare survey data - handle with appropriate care
- Do not expose individual patient information
- Aggregate statistics only in outputs

### Common Tasks

**Adding new analysis:**
1. Create new R Markdown file or add section to `PPE_Analysis.Rmd`
2. Follow existing code style and table formatting
3. Use same package ecosystem

**Modifying data cleaning:**
1. Edit `prepare_raw_data.Rmd`
2. Re-run to regenerate cleaned dataset
3. Verify downstream analyses still work

**Updating statistical models:**
1. Models defined in `Analysis.Rmd` and `PPE_Analysis.Rmd`
2. Keep model naming convention (Model 1, Model 2, Model A, etc.)
3. Document any model changes

---

## Git Workflow

- Main development on feature branches
- Pull requests for merging changes
- Commit messages should describe analysis changes clearly
- Avoid committing sensitive data files

---

## Troubleshooting

### Common Issues

1. **Package not found:** Use `pacman::p_load()` which auto-installs missing packages
2. **File path errors:** Use `here::here()` for reliable path resolution
3. **Encoding issues:** Ensure UTF-8 encoding for Vietnamese text
4. **Large HTML output:** Normal - contains embedded tables and styling

### File Dependencies

```
prepare_raw_data.Rmd
    └── 7_hospital_data.xlsx (input)
    └── data_PPE_15_10_09_2025.xlsx (output)
         │
         ▼
Analysis.Rmd / PPE_Analysis.Rmd
    └── PPE_Analysis.html (output)
```

---

## Contact & Attribution

**Project Author:** Đam Mê Nghiên Cứu (Research Enthusiast)
**Repository:** Patient_Experience_Viet_Nam
**Last Updated:** October 2025
