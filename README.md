# eSim 2.5 Installation Compatibility Fixes on Ubuntu 25.04

**FOSSEE Internship Screening Task – Task 4**

![Ubuntu](https://img.shields.io/badge/OS-Ubuntu%2025.04-orange)
![Bash](https://img.shields.io/badge/Script-Bash-green)
![Python](https://img.shields.io/badge/Language-Python-blue)
![eSim](https://img.shields.io/badge/Software-eSim%202.5-blue)
![Status](https://img.shields.io/badge/Status-Installation%20Tested-success)

## 1. About the Project

This project focuses on finding and fixing installation problems in **eSim 2.5 on Ubuntu 25.04**.

eSim is an open-source Electronic Design Automation (EDA) tool used for designing and simulating electronic circuits.

During installation on newer Ubuntu versions, some dependencies, package names, resource paths and installation scripts may not work as expected.

The main goal of this project is to:

- Install eSim 2.5 on Ubuntu 25.04.
- Identify errors that occur during installation.
- Find the reasons behind those errors.
- Modify the Bash installer scripts to fix the problems.
- Test the changes and check whether eSim works correctly.
- Document the complete process so that others can understand and reproduce the fixes.

**The work includes changes to the main eSim installer and the bundled NGHDL installer.**

---

## 2. Project Objectives

The main objectives are:

1. Study the existing eSim installation scripts.
2. Understand the dependency requirements of eSim and its supporting tools.
3. Identify compatibility problems on Ubuntu 25.04.
4. Fix problems related to LLVM, Clang, GHDL, NGHDL and other dependencies.
5. Correct incorrect file and directory paths.
6. Improve the installation process to avoid unnecessary failures and interactive prompts.
7. Test the installation and verify the working application.
8. Maintain the changes using Git and GitHub.

---

## 3. Technologies and Tools Used

| Technology / Tool | Purpose |
|---|---|
| Ubuntu 25.04 | Operating system used for testing |
| VirtualBox | Running Ubuntu in a virtual machine |
| Bash | Modifying and running installation scripts |
| Python 3.13.3 | Python environment used by eSim |
| eSim 2.5 | Main electronic design and simulation software |
| Git | Tracking code changes |
| GitHub | Hosting the modified project |
| LLVM 18 | Compiler infrastructure used by GHDL |
| Clang 18 | C/C++ compiler used for the GHDL build |
| GHDL | VHDL analysis and simulation |
| NGHDL | Supporting HDL tool installation |
| Ngspice | Circuit simulation engine |
| KiCad | Schematic design and editor integration |
| Verilator | Hardware description language tool |

---

## 4. Testing Environment

The installation and testing were performed using the following environment:

| Detail | Version / Information |
|---|---|
| Operating System | Ubuntu 25.04 |
| Environment | Oracle VirtualBox |
| Kernel | Linux 6.14.0-37-generic |
| Python | 3.13.3 |
| GCC | 14.2.0 |
| LLVM | 18.1.8 |
| GHDL | 4.1.0 |
| Ngspice | 35 |
| Verilator | 4.210 |

---

## 5. Installation Methodology

The following approach was used during the project.

### Step 1: Set up Ubuntu

- Installed Ubuntu 25.04 inside VirtualBox.
- Opened the Ubuntu terminal.
- Checked the operating system and installed tool versions.

### Step 2: Get the eSim source code

- Forked the official FOSSEE eSim repository on GitHub.
- Cloned the forked repository.
- Checked out the `installers` branch.
- Studied the existing Ubuntu installation scripts.

### Step 3: Try the installation

Ran the eSim installer and observed the errors that occurred during dependency installation and application setup.

The installation was repeated after making changes so that different problems could be investigated.

### Step 4: Investigate the errors

For each problem:

1. Checked the terminal error message.
2. Located the related section in the installer script.
3. Checked the file paths, package names and dependency versions.
4. Identified the likely reason for the failure.
5. Modified the relevant script.

### Step 5: Test the fixes

- Checked Bash script syntax.
- Checked the updated NGHDL archive.
- Re-ran the installation.
- Tested the installed tools.
- Opened the eSim application and KiCad editor.

---

## 6. Problems Identified and Fixes

The following problems were identified during the installation and debugging process.

### Issue 1: Incorrect resource file paths

**Problem:**

Some installer commands searched for required files in the `library/` and `images/` directories at the repository root.

However, the required resources in the repository were located inside the `MacOS/` directory.

**Cause:**

The installer used incorrect relative paths.

**Fix:**

Updated the paths to use:

- `MacOS/library/`
- `MacOS/images/`

The changes covered the KiCad library archive, Modelica mapping file, SKY130 PDK, model parameter XML files and application logo.

**Verification:**

The installation reached the resource-copying steps and completed successfully.

### Issue 2: Incorrect NGHDL ZIP file location

**Problem:**

The installer tried to extract `nghdl.zip` from the current directory.

**Cause:**

The archive was actually located at `MacOS/nghdl.zip`.

**Fix:**

Changed the extraction command to:

`unzip -o MacOS/nghdl.zip`

**Verification:**

The updated ZIP archive passed the integrity test using `unzip -t`.

### Issue 3: Incorrect eSim launcher path

**Problem:**

The generated eSim launcher tried to open the GUI from:

`src/frontEnd`

But the actual GUI entry point was located at:

`MacOS/src/frontEnd/Application.py`

**Cause:**

The launcher used an incorrect application directory.

**Fix:**

Updated the launcher to use:

`$eSim_Home/MacOS/src/frontEnd`

**Verification:**

Running the `esim` command opened the splash screen, workspace selection window and main eSim application.

### Issue 4: Incorrect KiCad installation control flow

**Problem:**

The KiCad installation check used `exit 0` inside a function.

**Cause:**

The `exit` command could terminate the complete child installer instead of returning from the function.

**Fix:**

Replaced:

`exit 0`

with:

`return 0`

**Verification:**

The installer continued through the remaining installation steps and completed successfully.

### Issue 5: Ubuntu version detection in NGHDL installer

**Problem:**

The NGHDL installer expected an Ubuntu version containing three numeric parts.

Ubuntu 25.04 reports its version using two numeric parts.

**Cause:**

The version-detection pattern did not match the Ubuntu 25.04 version format.

**Fix:**

Updated the version-matching expression to accept two or three numeric components.

Also configured Ubuntu 24.04 and Ubuntu 25.04 to use the compatible `install-nghdl-24.04.sh` script.

**Verification:**

Checked the updated dispatcher inside the NGHDL archive and confirmed the correct script selection.

### Issue 6: LLVM version compatibility problem

**Problem:**

The GHDL build required a compatible LLVM version. The default LLVM version available in the Ubuntu 25.04 package environment was newer than the version supported by the bundled GHDL build.

**Cause:**

The installer did not select the required LLVM version explicitly.

**Fix:**

Updated the NGHDL installation script to install:

- `llvm-18`
- `llvm-18-dev`

Configured GHDL to use LLVM 18 through:

`--with-llvm-config=/usr/bin/llvm-config-18`

**Verification:**

- `llvm-config-18` reported version 18.1.8.
- GHDL 4.1.0 reported LLVM 18.1.8 as its code generator.
- A basic VHDL simulation completed successfully.

### Issue 7: Clang++ command not found

**Problem:**

The GHDL build failed because the generic `clang++` command was not available.

**Cause:**

The installer did not explicitly select the installed Clang version.

**Fix:**

Changed the compiler selection to Clang 18:

`CXX=clang++-18`

Also changed the package installation to use `clang-18`.

**Verification:**

Confirmed that `clang++-18` was available and that GHDL built with the LLVM 18 toolchain.

### Issue 8: GTK Canberra package unavailable

**Problem:**

The package `libcanberra-gtk-module` was unavailable in the Ubuntu 25.04 package environment used for testing.

**Cause:**

The installer requested a package name that was not available in that environment.

**Fix:**

Changed the package to:

`libcanberra-gtk3-module`

**Verification:**

The replacement package was installed and the installation continued.

### Issue 9: Interactive file overwrite during GHDL build

**Problem:**

The build process could stop and wait for confirmation when moving files that already existed.

**Cause:**

The file move operation could request interactive overwrite confirmation.

**Fix:**

Updated the make command to:

`make -j$(nproc) MV="mv -f"`

**Verification:**

Confirmed the updated command in the NGHDL installer archive. The final installation completed.

### Issue 10: Existing NGHDL source directory conflict

**Problem:**

An existing NGHDL source directory could conflict with the extraction and move operations during another installation attempt.

**Cause:**

The installer did not safely handle an existing source directory.

**Fix:**

Added logic to:

1. Create the destination directory.
2. Check whether the previous source directory exists.
3. Move the old directory to a timestamped backup.
4. Move the newly extracted source into the destination.
5. Return an error if a move operation fails.

**Verification:**

Inspected the updated script inside the archive and checked the ZIP integrity. A complete second installation was not separately tested.

### Issue 11: Parent installer conditional structure

**Problem:**

Some configuration commands were placed after the child installer execution, and the parent conditional block was missing its closing `fi`.

**Cause:**

The parent installer had an incorrect conditional structure and contained configuration commands already handled by the child installer.

**Fix:**

Removed the misplaced configuration commands and added the missing closing `fi`.

**Verification:**

Both modified shell scripts passed the Bash syntax check using `bash -n`.

---

## 7. Main Files Modified

The following files contain the main changes:

| File | Changes |
|---|---|
| `Ubuntu/install-eSim.sh` | Corrected parent installer conditional structure |
| `Ubuntu/install-eSim-scripts/install-eSim-25.04.sh` | Fixed resource paths, launcher path, KiCad control flow and NGHDL archive path |
| `MacOS/nghdl.zip` | Updated bundled NGHDL installer scripts for Ubuntu 25.04 compatibility |

---

## 8. Testing and Verification

After applying the changes, the following tests were performed.

### 8.1 Installer completion

The main installer completed and displayed:

`eSim Installed Successfully`

### 8.2 Installed tools

The following tools were checked:

| Tool | Result |
|---|---|
| Ngspice | Version 35 available |
| GHDL | Version 4.1.0 available |
| Verilator | Version 4.210 available |
| NGHDL | Command available at `/usr/local/bin/nghdl` |

### 8.3 VHDL simulation test

A simple VHDL program was analyzed, elaborated and simulated using GHDL with the VHDL-2008 standard.

The test printed:

`GHDL compilation and simulation successful`

### 8.4 eSim GUI test

Ran:

`esim`

The eSim splash screen, workspace selection window and main application opened successfully.

### 8.5 KiCad integration test

- Opened the KiCad Schematic Editor through the eSim workflow.
- Created a test project named `Installation_Test`.
- Created and saved the schematic file `Installation_Test.kicad_sch`.

### 8.6 Script and archive checks

The following checks passed:

- `bash -n Ubuntu/install-eSim.sh`
- `bash -n Ubuntu/install-eSim-scripts/install-eSim-25.04.sh`
- `git diff --check`
- `unzip -t MacOS/nghdl.zip`

---

## 9. Known Issues and Limitations

Some observations were not fully investigated.

- A `pixman_region32_init_rect: Invalid rectangle passed` warning appeared during one schematic-opening attempt. The schematic was later created and saved successfully, but the warning itself was not diagnosed.
- The repeat-install source backup logic was checked in the updated script but was not tested through a complete second installation.
- A complete analog circuit simulation workflow has not yet been verified.

These points are recorded to clearly separate verified fixes from remaining work.

---

## 10. GitHub Repository and Commits

The installer changes and README documentation have been committed and pushed to the forked repository.

**Repository:**
https://github.com/ROOPAM2025/eSim

**Branch:**
`installers`

**Commit 1 — Installer compatibility fixes:**
`1f1eaaaf`

**Commit message:**
`Fix eSim installer compatibility with Ubuntu 25.04`

**Commit link:**
https://github.com/ROOPAM2025/eSim/commit/1f1eaaaf

**Commit 2 — README documentation:**
`687dec11`

**Commit message:**
`Improve README with Ubuntu 25.04 installation fixes`

**Commit link:**
https://github.com/ROOPAM2025/eSim/commit/687dec11

---

## 11. Conclusion

This project investigated the installation compatibility problems of eSim 2.5 on Ubuntu 25.04.

Several problems were found in the main installer and the bundled NGHDL installer. These included incorrect resource paths, Ubuntu version detection, LLVM and Clang compatibility, missing package names, launcher configuration and shell-script control flow.

The relevant installer scripts were modified, and the updated code was committed and pushed to GitHub.

The final installation completed successfully. The eSim GUI opened, the supporting tools were available, a basic VHDL simulation passed, and KiCad schematic creation and saving were verified.

This work provides a set of installer compatibility improvements and documents the investigation and testing process for future users.

---

**Project:** eSim 2.5 Installation Compatibility Fixes
**Task:** FOSSEE Internship Screening – Task 4
**Operating System:** Ubuntu 25.04
