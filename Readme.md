# R + RStudio Setup for Windows 11

This repo contains a one-click PowerShell script to set up a fresh Windows 11 Pro machine with:

- PowerShell 7 (if not already installed)
- R (latest CRAN build)
- RStudio Desktop
- (Optional) Rtools (for building packages from source)
- (Optional) Git and Quarto
- A `.Rprofile` that pins a reliable CRAN mirror
- A `bootstrap.R` script to install R packages (uses `packages.txt` if present)

---

## 🚀 Quick Start

1. **Open PowerShell 7 (x64) as Administrator**  
   - If you don’t have PowerShell 7, this script will install it and relaunch itself.

2. **Run the installer**  
   Replace `<YOUR-USER>` with your GitHub username:

   ```powershell
   $raw = "https://raw.githubusercontent.com/<YOUR-USER>/r-setup-windows/main/Setup-R-RStudio.ps1"
   Invoke-WebRequest $raw -OutFile Setup-R-RStudio.ps1
   Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
   .\Setup-R-RStudio.ps1
