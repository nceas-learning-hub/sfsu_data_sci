# Week 3 Day 2 Teaching Script - Advanced Quarto Features

## Files Included

1. **week3_day2_teaching_script.qmd** - Complete teaching script with data embedded
2. **week3_day2_teaching_script_with_csv.qmd** - Alternative version that reads from CSV
3. **bee_data.csv** - Sample dataset for import

## Quick Start

### Option 1: Embedded Data (Recommended for teaching)
1. Open `week3_day2_teaching_script.qmd` in RStudio
2. Click "Render" button
3. All data is created automatically in the script

### Option 2: External CSV
1. Place `bee_data.csv` in your project directory
2. Open `week3_day2_teaching_script_with_csv.qmd` in RStudio
3. Click "Render" button

## Required Packages

Install before teaching:

```r
install.packages(c("tidyverse", "knitr", "kableExtra"))
```

For PDF output (optional):
```
quarto install tinytex
```

## Features Demonstrated

### Figures
- ✅ Basic labeled figures with `#| label: fig-name`
- ✅ Figure captions with `#| fig-cap:`
- ✅ Custom figure sizing with `#| fig-width:` and `#| fig-height:`
- ✅ Multiple figures side-by-side with `layout-ncol: 2`
- ✅ Figure subcaptions with `#| fig-subcap:`
- ✅ Cross-references using `@fig-name`

### Tables
- ✅ Basic tables with `kable()`
- ✅ Table labels with `#| label: tbl-name`
- ✅ Table captions with `#| tbl-cap:`
- ✅ Enhanced formatting with `kableExtra`
- ✅ Cross-references using `@tbl-name`
- ✅ Inline R code referencing table values

### Equations
- ✅ Inline equations: `$equation$`
- ✅ Display equations: `$$equation$$`
- ✅ Numbered equations with `{#eq-name}`
- ✅ Cross-references using `@eq-name`

### Callout Blocks
- ✅ Note callouts: `:::{.callout-note}`
- ✅ Warning callouts: `:::{.callout-warning}`
- ✅ Tip callouts: `:::{.callout-tip}`
- ✅ Important callouts: `:::{.callout-important}`

### Layout Options
- ✅ Table of contents: `toc: true` in YAML
- ✅ Code folding: `code-fold: true`
- ✅ Theme customization: `theme: cosmo`
- ✅ Two-column layout with `{.columns}`
- ✅ Self-contained HTML with `embed-resources: true`

### Multiple Output Formats
- ✅ HTML output configured
- ✅ PDF output configured (requires tinytex)
- ✅ Both formats can render from same source

## Teaching Tips

### Live Demo Workflow (12 minutes)
1. **Start simple** (2 min): Show basic document structure
2. **Add first figure** (3 min): Demonstrate label and caption
3. **Add table** (3 min): Show kable() with formatting
4. **Add cross-references** (2 min): Use @ syntax in text
5. **Add callout** (1 min): Insert warning or note block
6. **Render and explore** (1 min): Show automatic numbering

### Common Student Issues to Address
- Forgetting `fig-` or `tbl-` prefix in labels
- Using `Figure 1` instead of `@fig-label` in cross-references
- YAML indentation errors for nested options
- Case sensitivity in labels

### Breakout Activity Support
Visit rooms and check for:
- Labels start with correct prefix
- Cross-references use @ syntax
- YAML has proper indentation
- Code chunks run without errors

## Data Description

**bee_data.csv** contains:
- `site`: Study location (North, Central, South)
- `richness`: Number of bee species observed
- `latitude`: Site latitude
- `temperature`: Average temperature (°C)

**Sample size**: 12 observations (4 per site)

**Analysis ready**: Pre-structured for immediate use in ggplot and statistical tests

## Testing Checklist

Before class, verify:
- [ ] Document renders to HTML successfully
- [ ] All figures display with correct numbering
- [ ] All tables format properly
- [ ] Cross-references are clickable and correct
- [ ] Code folding works (click "Show code" buttons)
- [ ] Table of contents appears and links work
- [ ] All packages load without errors
- [ ] (Optional) PDF output works if you'll demo it

## Customization Options

### Change theme:
Replace `theme: cosmo` with: flatly, journal, minty, pulse, sandstone, simplex, yeti

### Adjust code visibility:
- `code-fold: true` - Code hidden by default, can show
- `code-fold: false` - All code visible
- `code-fold: show` - Code visible by default, can hide

### Modify figure sizes:
Default is 6×4 inches. Adjust as needed:
```
#| fig-width: 8
#| fig-height: 6
```

## During Lecture

### Minute-by-Minute (12-minute demo)
00:00-00:02 - Open file, show structure
00:02-00:05 - Add/modify first figure
00:05-00:08 - Add/modify table
00:08-00:10 - Add cross-references
00:10-00:11 - Add callout block
00:11-00:12 - Render and explore

### Key Teaching Points
1. "Labels MUST start with fig- or tbl- for cross-references"
2. "Use @ syntax for automatic numbering"
3. "Cross-references update automatically if you reorder"
4. "Same source file can create HTML, PDF, and Word"

## Support Resources

If students have issues:
- RStudio Visual Editor can help with syntax
- Quarto documentation: https://quarto.org/docs/authoring/
- Your course discussion board for questions
- Office hours for troubleshooting

---

**Created for**: BIO/CHEM 806 - Graduate Introduction to Data Science with R
**Instructors**: Dr. Gretchen LeBuhn & Dr. Nicole Adelstein
**Week**: 3 Day 2
**Duration**: 75-minute session
