# SOP-MET-001

**Title:** RAW to mzML Conversion Using MSConvert

**Code:** POP-MET-001

**Area:** Metabolomics

**Software:** MSConvert

**Prepared by:** Tainá Schons

**Creation Date:** 2026-06-01

**Last Updated:** 2026-06-01

**Level:** Basic

## Objective

Describe the procedure for converting raw mass spectrometry data files (.raw) into the open mzML format using MSConvert, ensuring compatibility with downstream metabolomics, lipidomics, and proteomics data processing platforms such as MZmine, MS-DIAL, GNPS, and SIRIUS.

---

## Background

The mzML format is an open standard developed by the Human Proteome Organization Proteomics Standards Initiative (HUPO-PSI) for the storage and exchange of mass spectrometry data. Converting vendor-specific files to mzML improves reproducibility, facilitates data sharing, and enables interoperability across different processing and analysis platforms.

---

## Scope

This procedure applies to data acquired on Waters, Thermo, Agilent, Bruker, and SCIEX instruments supported by ProteoWizard.

---

## Required Software and Tools

| Software               | Version     |
| ---------------------- | ----------- |
| ProteoWizard MSConvert |             |
| Microsoft Windows      | 10 or later |

---

## Input Files

* `.raw` (Waters)
* `.RAW` (Thermo)
* Other ProteoWizard-compatible formats

---

## Procedure

### Step 1 – Open MSConvert

Launch the MSConvert software.

### Step 2 – Select Input Files

Select the RAW files to be converted.

* Click **Browse**
* Navigate to the folder containing the raw data files
* Select the files to be converted

### Step 3 – Define the Output Directory

Select the destination folder where the mzML files will be saved using the **Output Directory** field.

### Step 4 – Configure the Output Format

Under **Output Format**:

* Select **mzML**

### Step 5 – Configure Filters

#### Centroided Data

No additional filters are required.

#### Profile Data

Apply the following filter:

```text
Peak Picking
Algorithm: Vendor
MS Levels: 1-
```

All other settings may remain at their default values.

**Important**

Data acquired in profile mode should be centroided during mzML conversion. The recommended approach is to use the **Peak Picking (Vendor)** filter, which applies the manufacturer's native centroiding algorithm. After conversion, the resulting mzML files will be recognized as centroided by most downstream processing software.

### Step 6 – Start the Conversion

Click **Start** and wait until the conversion process is completed.

---

## Quality Control

After conversion, verify that:

* All files were successfully converted
* No errors were reported by MSConvert
* Output file sizes are consistent with expectations
* The mzML files can be opened successfully in MZmine

---

## Expected Outputs

* mzML files
* Centroided spectra when Peak Picking is applied
* Files compatible with MZmine, MS-DIAL, GNPS, and SIRIUS

---

## Common Issues

| Issue                                | Possible Cause                                         | Solution                                             |
| ------------------------------------ | ------------------------------------------------------ | ---------------------------------------------------- |
| Reader Fail error                    | Special characters in the file path (e.g., á, é, ç, ã) | Move files to a directory without special characters |
| mzML file cannot be opened in MZmine | Incomplete conversion                                  | Repeat the conversion process                        |
| Output file is unexpectedly small    | Incorrect filter configuration                         | Review conversion settings                           |

---

## References

ProteoWizard Documentation
https://proteowizard.sourceforge.io/

HUPO-PSI mzML Specification
https://www.psidev.info/mzml

---

## Notes

* Avoid folder names containing accented or special characters.
* Keep the original RAW files unchanged.
* Store mzML files in a separate directory from the raw data.
* For Thermo, Waters, Agilent, and Bruker profile-mode data, vendor centroiding is generally recommended before downstream metabolomics processing.

---

## Revision History

| Date       | Description     | Author       |
| ---------- | --------------- | ------------ |
| 2026-06-01 | Initial version | Tainá Schons |
