# Bootstrappign — Windows 11 Setup for R + RStudio

This repo includes a one-click PowerShell setup for a clean **Windows 11 Pro** machine:
- Installs/updates **PowerShell 7** (if needed)
- Installs **R** and **RStudio Desktop**
- (Optional) installs **Rtools**, **Git**, **Quarto**
- Creates a user `~\Documents\.Rprofile` with a CRAN mirror
- Generates and runs `bootstrap.R` to install common R packages (or your own list from `packages.txt`)

> You can run everything without cloning (via raw GitHub download), or clone the repo and run locally.

---

## 🚀 Quick Start (no clone)

Open **PowerShell 7 (x64)** as **Administrator**, then run:

```powershell
$raw = "https://raw.githubusercontent.com/estanzel/Bootstrappign/main/Setup-R-RStudio.ps1"
Invoke-WebRequest $raw -OutFile Setup-R-RStudio.ps1
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
.\Setup-R-RStudio.ps1
```

**Optional:** If you want to provide a custom package list instead of the built-in defaults, also fetch `packages.txt` into the same folder before running the script:

```powershell
$pkg = "https://raw.githubusercontent.com/estanzel/Bootstrappign/main/packages.txt"
Invoke-WebRequest $pkg -OutFile packages.txt
```

The setup script will detect `packages.txt` in the current folder and install those exact packages (skipping ones already present).

---

## 🧰 What the script does

- Ensures **PowerShell 7** is available, relaunches itself under PS7 if needed
- Installs: **R**, **RStudio**, (optionally) **Rtools**, **Git**, **Quarto**
- Writes `~\Documents\.Rprofile` with:
  ```r
  options(
    repos = c(CRAN = "https://cloud.r-project.org"),
    download.file.method = "wininet"
  )
  ```
- Creates `bootstrap.R` next to the `.ps1` and runs it once to install packages

Default packages include (edit `packages.txt` to customize):  
`tidyverse, data.table, sf, ggplot2, ggnewscale, viridis, terra, CoordinateCleaner, SDMtune, furrr, blockCV, usdm, geodata, spocc, plotROC, dplyr, stringr, readr, lubridate, devtools, remotes, here, janitor, renv`

---

## 🧪 Verify + Troubleshoot

- After the script, you should see **R** and **RStudio** in Start Menu.
- If `pkgbuild::has_build_tool()` warns about build tools, **Rtools** may need a restart to appear on PATH. Close and reopen RStudio.
- If `Rscript` is not found during the first run, just open RStudio and run:
  ```r
  source("bootstrap.R")
  ```

**Windows tips:**
- **sf / terra**: CRAN provides Windows binaries; these typically work without extra system dependencies. Keeping R up to date helps.
- **Quarto → PDF**: If you later need PDF output, run `quarto install tinytex` or `tinytex::install_tinytex()` in R.

---

## 💻 Clone-and-workflow (optional)

If you want to clone and work inside this repo (e.g., with RStudio Projects / Quarto files):

```powershell
cd $HOME
git clone https://github.com/estanzel/Bootstrappign.git
cd Bootstrappign
Rscript bootstrap.R   # or open RStudio and source("bootstrap.R")
```

Open **RStudio** → **File → Open Project…** and select the `.Rproj` file if present, then open your `.qmd` files and press **Render** (or render via `quarto render` from the terminal).

---

## 📦 Exporting a package list from another machine (optional)

When you regain access to your other environment, you can export its installed packages to re-use here:

```r
writeLines(installed.packages()[,"Package"], "packages.txt")
```

Copy `packages.txt` to the same folder as `Setup-R-RStudio.ps1` and re-run the script, or commit it to this repo.

---

## 📝 License

This project is licensed under the MIT License. See `LICENSE` for details.
