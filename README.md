# sfsu_data_sci

Course textbook (Quarto book) for Biology/Chemistry 806 at SFSU, published via GitHub Pages.

Live site: https://nceas-learning-hub.github.io/sfsu_data_sci/  
Source repository: https://github.com/nceas-learning-hub/sfsu_data_sci/tree/main

---

## Repository structure (what lives where)

At a high level, this repo is a standard **Quarto book** project:

### Top-level (source of the book)
- `index.qmd`  
  The book landing page / “About the course” page and table of contents entry point.
- `s##_*.qmd` lesson files  
  Each lesson/chapter lives as a Quarto markdown file in the repository root (examples include `s01_r_rstudio_server_setup.qmd`, `s06_github_introduction.qmd`, etc.).
- `_quarto.yml`  
  Quarto configuration (book title, output options, sidebar/TOC settings, etc.).
- `metadata_course.csv`, `metadata_lessons.csv`  
  Course/lesson metadata used to describe and organize content.
- `sfsu_data_sci.Rproj`  
  RStudio project file for local development.

### Assets used by lessons
- `data/`  
  Datasets used in exercises or examples.
- `images/`  
  Figures and images referenced by lessons.
- `slides/`  
  Slide decks (typically rendered presentations used alongside lessons).

### Rendering / publishing outputs (mostly generated)
- `docs/`  
  The rendered HTML website used by **GitHub Pages** (do not edit by hand; this is build output).
- `_freeze/`  
  Quarto “freeze” cache for computed content to keep renders reproducible and fast.
- `rosm.cache/`  
  Cached OpenStreetMap tiles/artifacts created by mapping workflows.

### Tooling / theme / automation
- `.github/workflows/`  
  GitHub Actions workflows (commonly used to render the Quarto book and publish to Pages).
- `_extensions/nceas-learning-hub/theme/`  
  Custom Quarto extension/theme used to style the site.
- `.gitignore`, `.Rhistory`  
  Standard development housekeeping files (note: `.Rhistory` is usually not committed in many projects, but is present here).

---

## Naming conventions

### Lessons / chapters (`.qmd`)
Lessons follow a consistent pattern:

**`sNN_<topic>.qmd`**

- `s` = “section/session”
- `NN` = **two-digit** order number (01, 02, …) to keep files sorting correctly
- `<topic>` = short, descriptive slug using **lowercase** + **underscores**

Examples:
- `s01_r_rstudio_server_setup.qmd`
- `s04_r_quarto_literate_analysis.qmd`
- `s06_github_introduction.qmd`
- `s11_r_data_visualization.qmd`

Some lessons include a **type tag** in the filename to set expectations:
- `lecture` (e.g., `s07_lecture_tidy_data.qmd`)
- `practice` (e.g., `s10_r_practice_tidy_data_joins.qmd`)
- `activity` (e.g., `s05_activity_reproducibility_lego.qmd`)

**Rule of thumb:** if you add a new lesson, follow the next available `sNN_...` number and keep the slug short and readable.

### Folders
Folders are **lowercase** and generally singular concepts:
- `data/`, `images/`, `slides/`, `docs/`

Special Quarto folders begin with an underscore and are usually generated or framework-driven:
- `_freeze/`, `_extensions/`

### Metadata files
Metadata files are **snake_case** and describe their scope:
- `metadata_course.csv` (course-level)
- `metadata_lessons.csv` (lesson-level)

---

## How the book is organized

The published site’s table of contents mirrors the lesson numbering: Weeks 1–9 are composed of the `s01`…`s21` lessons in order. If you rename or re-number lesson files, you’ll usually need to update the book configuration (in `_quarto.yml`) so the sidebar/TOC stays aligned.

---

## Editing guidelines (practical + conservative)

- **Edit content in `.qmd` files**, not in `docs/`.
- Treat `docs/` as **build output** (safe to regenerate).
- Keep filenames stable once a course is underway—renames can break links and bookmarks.
- Put datasets in `data/`, images in `images/`, and slide sources/outputs in `slides/`.
- Prefer short, descriptive asset names (lowercase, underscores) so links stay predictable.

---

## Build / preview (typical Quarto workflow)

From the repository root, you can usually preview locally with:

- `quarto preview`

…and render with:

- `quarto render`

(Exact behavior depends on `_quarto.yml` and any GitHub Actions workflows.)

---
## Adding a new lesson (checklist)

Use this checklist to add a lesson while keeping the book stable and consistent.

### 1. Choose the filename
- Use the next available lesson number:
  - `sNN_<topic>.qmd`
- Keep:
  - `NN` = two digits (01, 02, …)
  - `<topic>` = lowercase, short, descriptive, underscores only
- If relevant, include a lesson type:
  - `lecture`, `practice`, or `activity`

**Examples**
- `s12_r_modeling_intro.qmd`
- `s08_practice_git_collaboration.qmd`

---

### 2. Place the file
- Put the `.qmd` file in the **repository root** (same level as other `sNN_*.qmd` files)
- Do **not** place lessons inside `docs/`

---

### 3. Add the lesson to the book
- Update `_quarto.yml` so the new lesson appears in the table of contents
- Confirm the order matches the `sNN_` numbering

---

### 4. Reference assets correctly
- Data files → `data/`
- Images/figures → `images/`
- Slides → `slides/`

Use **relative paths**, for example:
- `data/my_dataset.csv`
- `images/example_plot.png`

Keep asset names lowercase with underscores.

---

### 5. Final sanity check
- Filenames follow existing conventions
- Lesson number is unique
- No broken links or missing assets
- The site builds cleanly

