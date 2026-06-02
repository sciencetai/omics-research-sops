# POP-MET-002

Title: Untargeted LC-MS Data Processing Using MZmine 4

Code: POP-MET-002

Area: Metabolomics

Software: MZmine 4.8.30

Prepared by: Tainá Schons

Creation Date: 2026-06-02

Last Updated: 2026-06-02

Level: Advanced

## Objective

> Describe the workflow for preprocessing untargeted metabolomics data using MZmine, from mzML file import to the generation of a feature matrix for downstream statistical analysis and metabolite annotation.

## Scope

This SOP applies to the preprocessing of untargeted LC-MS/MS data acquired on high-resolution mass spectrometers, including Orbitrap and QTOF platforms.

## Workflow

1. **Import Data**
2. **Initial Data Inspection**
3. **Mass Detection**
4. **Chromatogram Builder**
5. **Feature Resolver**
6. **13C Isotope Filter**
7. **Join Aligner**
8. **Gap Filling**
9. **Export Feature List**

### Workflow Overview

```text
RAW files
↓
MSConvert
↓
mzML
↓
Import Data
↓
Mass Detection
↓
Chromatogram Builder
↓
Feature Resolver
↓
13C Isotope Filter
↓
Join Aligner
↓
Gap Filling
↓
Export Feature List
↓
R (Quality Control, Filtering, Normalization and Statistical Analysis)
```

## Required Software and Tools

| Item             | Version                                      |
| ---------------- | -------------------------------------------- |
| MZmine           | Version 4 or later                           |
| Java             | Compatible with the installed MZmine version |
| Operating System | Windows 10 or later                          |

## Input Files

* mzML files

  * Biological samples
  * Quality Control (QC) samples
  * Analytical blanks (when available)

---

## Procedure

### Step 1 – Data Import

### Menu

```text id="w7t0aj"
Import → Data Files
```

Import all mzML files from the experiment.

### Verify the Following

* Correct number of biological samples
* Correct number of QC samples
* Presence of analytical blanks
* Correct sample metadata assignment

### Sample Metadata Verification

Open:

```text id="5qff3r"
Project → Sample Metadata
```

or use:

```text id="yz7gby"
Ctrl + M
```

Verify that all files are correctly classified as:

* Samples
* Quality Controls (QCs)
* Blanks

Proper metadata assignment is essential for downstream quality control, blank filtering, and statistical analyses.

### Step 2 – Initial Data Inspection

Before starting data processing, open at least one biological sample, one analytical blank, and one QC sample in MZmine for visual inspection of the raw data.

The following aspects should be evaluated:

* Presence of well-defined chromatographic peaks;
* Overall signal intensity distribution;
* Blank sample profile;
* QC sample consistency;
* Presence of background noise;
* Preliminary estimation of MS1 and MS2 noise levels.

> **Important**
>
> Background noise is typically characterized by low-intensity signals distributed throughout the mass spectrum without a clear chromatographic or isotopic pattern. These signals may originate from electronic noise, chemical noise, contaminants, or instrumental fluctuations.
>
> The noise level should be set above this baseline to minimize the inclusion of non-informative signals during mass detection and subsequent data processing steps.

### Step 3 – Mass Detection

### Menu

```text
Feature Detection → Mass Detection
```

### Objective

Detect spectral signals above the instrumental noise level.

### Parameters

| Parameter       | Typical Orbitrap Values* | Typical TOF Values* |
| --------------- | ------------------------ | ------------------- |
| MS1 Noise Level | 1,000–10,000             | TBD                 |
| MS2 Noise Level | 100–1,000                | TBD                 |

* Values provided only as an initial reference. Parameters should be optimized for each dataset.

### How to Define the Noise Level?

The most reliable way to determine the instrumental noise level is by inspecting the raw data directly in MZmine, as described in **Step 2 – Initial Data Inspection**.

The noise level should not be defined based on a single spectrum or copied directly from the literature. Instead, biological samples, QC samples, and blanks should be inspected across different regions of the chromatographic run to estimate the background noise intensity.

The parameter should be optimized iteratively by evaluating its impact on:

* the number of detected features;
* chromatogram quality;
* overall data quality.

The goal is to remove background noise and instrumental artifacts without compromising the detection of low-abundance metabolites.

As a practical strategy, test multiple noise level values on a representative subset of the dataset (e.g., one biological sample, one QC, and one blank) and compare the resulting chromatograms and feature counts after chromatogram building and feature resolution.

> **Important**
>
> The noise level is only the first filtering step applied during preprocessing. Other parameters, such as *Minimum Absolute Height*, *Minimum Relative Height*, and chromatographic peak resolution criteria, also have a substantial impact on the final number of detected features.

---

### Step 4 – Chromatogram Builder

### Menu

```text
Feature Detection → LC-MS → Chromatogram Builder
```

### Objective

Group signals with similar m/z values detected across consecutive scans into chromatographic traces representing potential compounds present in the sample.

### How Does It Work?

After the Mass Detection step, MZmine contains only a list of detected signals for each individual scan.

The Chromatogram Builder connects signals with similar m/z values observed across consecutive scans throughout the chromatographic run, generating **Extracted Ion Chromatograms (EICs)**. These chromatograms are subsequently evaluated during the feature resolution step to generate individual chromatographic features.

### Parameters

| Parameter                           | Description                                                                                                          | Orbitrap Example    | How to Define                                                                                                                                                                                                          |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MS level                            | Defines which spectra will be used to build chromatograms. Only MS1 spectra are processed at this stage.             | MS1                 | Use MS1 for untargeted LC-MS feature detection workflows.                                                                                                                                                              |
| Min consecutive scans               | Minimum number of consecutive scans required to build a chromatogram.                                                | 5                   | Depends on chromatographic peak width and acquisition rate. Inspect representative peaks and determine the typical number of data points per peak. Values below 4–5 scans are generally not recommended.               |
| Min intensity for consecutive scans | Minimum intensity required for a group of consecutive scans to be retained as a chromatogram.                        | 30,000              | A useful starting point is approximately 3× the selected MS1 noise level.                                                                                                                                              |
| Min absolute height                 | Minimum intensity required for the most intense point of a chromatogram.                                             | 100,000             | Functions as an additional filter to remove chromatograms generated from weak signals. A reasonable starting value is 7–10× the MS1 noise level.                                                                       |
| m/z tolerance                       | Maximum mass difference allowed between signals in consecutive scans to be considered part of the same chromatogram. | 0.002 m/z and 5 ppm | Should reflect the mass accuracy of the instrument. For Orbitrap instruments, values between 0.002–0.005 m/z and 5–10 ppm are commonly used. For TOF instruments, 0.005 m/z and 10–15 ppm are typical starting points. |

> **Important**
>
> The *Min intensity for consecutive scans* and *Min absolute height* parameters are directly related to the MS1 noise level defined in the previous step. Values that are too low may result in chromatograms generated from instrumental noise, whereas excessively high values may lead to the loss of low-abundance metabolites.

### Quality Control

After running the Chromatogram Builder, verify:

* the total number of chromatograms generated;
* the presence of well-defined chromatographic traces;
* the absence of excessive chromatograms originating from noise;
* consistent behavior across biological samples, QCs, and blanks.

> **Note**
>
> This step does not identify compounds or generate final features. Its sole purpose is to construct chromatograms from detected signals. The separation of co-eluting peaks and the generation of individual features will occur in the subsequent feature resolution step.

### Step 5 – Local Minimum Feature Resolver

### Menu

```text id="zkv8vf"
Feature Detection → Resolving → Local Minimum Feature Resolver
```

### Objective

Separate co-eluting or partially overlapping chromatographic peaks present within the chromatograms generated in the previous step, allowing the detection of individual features.

### How Does It Work?

After chromatogram construction, a single Extracted Ion Chromatogram (EIC) may contain signals originating from multiple compounds eluting at similar retention times.

The Local Minimum Feature Resolver identifies local minima between chromatographic peaks and uses these valleys to split a chromatogram into independent features.

> **This is one of the most critical preprocessing steps, as it directly affects both the number and quality of the detected features.**

### Parameters

| Parameter                  | Description                                                             | Orbitrap Example  | How to Define                                                                                                                           |
| -------------------------- | ----------------------------------------------------------------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Feature lists              | Input chromatogram lists                                                | All feature lists | Use the chromatograms generated in the previous step                                                                                    |
| Dimension                  | Dimension used for peak resolution                                      | Retention time    | Retention time is typically used for LC-MS workflows                                                                                    |
| Suffix                     | Output feature list suffix                                              | resolved          | Record this suffix, as it will be used in the next step                                                                                 |
| Chromatographic threshold  | Minimum valley depth required to separate two adjacent peaks            | 90%               | Higher values make peak separation more stringent. Typical values range from 80–95%.                                                    |
| Minimum search range (RT)  | Minimum chromatographic width considered during local minimum detection | 0.05 min          | Should reflect the expected peak width. Values that are too low may split noise; values that are too high may merge distinct compounds. |
| Minimum relative height    | Minimum relative height required for a peak to be retained              | 5%                | Helps avoid detection of shoulders and low-relevance signals.                                                                           |
| Minimum absolute height    | Minimum intensity required for a resolved peak                          | 10,000            | Should be compatible with the noise level selected during Mass Detection.                                                               |
| Min ratio of peak top/edge | Minimum ratio between peak apex intensity and peak edge intensity       | 1.5               | Helps distinguish real chromatographic peaks from baseline fluctuations.                                                                |
| Peak duration range        | Acceptable chromatographic peak width range                             | 0.03–1.50 min     | Should reflect the expected peak widths for the chromatographic method used.                                                            |
| Minimum scans              | Minimum number of scans required to define a peak                       | 5                 | At least 5 scans are generally recommended for robust peak detection.                                                                   |

### Quality Control

After feature resolution, verify:

* total number of detected features;
* presence of well-defined chromatographic peak shapes;
* absence of excessive splitting of individual peaks;
* consistency across biological samples, QCs, and blanks.

> **Important**
>
> Overly permissive parameters may result in excessive peak splitting (*over-splitting*), artificially increasing the number of detected features. Conversely, overly restrictive parameters may merge distinct compounds into a single feature.

---

## Step 6 – Isotope Filter (13C Isotope Filter)

### Menu

```text id="wf64wp"
Feature List Methods → Isotopes → 13C Isotope Filter
```

### Objective

Identify isotope groups and remove redundant features generated by naturally occurring isotopologues, particularly those arising from ^13C abundance.

### How Does It Work?

Most organic molecules contain multiple carbon atoms. Since approximately 1.1% of naturally occurring carbon corresponds to the ^13C isotope, a single compound may generate multiple related signals:

```text id="z0vb38"
M      → monoisotopic peak
M+1    → one 13C atom
M+2    → two 13C atoms
```

For example:

| Feature | m/z      |
| ------- | -------- |
| M       | 300.1234 |
| M+1     | 301.1268 |
| M+2     | 302.1301 |

Although these appear as separate features, they originate from the same compound.

The Isotope Filter identifies isotope groups and retains only the representative feature, reducing redundancy in the feature matrix.

### Parameters

| Parameter              | Description                                                     | Orbitrap Example   | How to Define                                  |
| ---------------------- | --------------------------------------------------------------- | ------------------ | ---------------------------------------------- |
| Feature lists          | Feature lists generated after chromatographic peak resolution   | `*resolved`        | Apply the filter to the resolved feature lists |
| Name suffix            | Output feature list suffix                                      | deisotoped         | Record this suffix for downstream processing   |
| m/z tolerance          | Tolerance used to identify isotope patterns                     | 0.002 m/z or 5 ppm | Should reflect instrument mass accuracy        |
| RT tolerance           | Maximum retention time difference allowed between isotopologues | 0.05 min           | Isotopologues are expected to co-elute         |
| Maximum charge         | Maximum charge state considered                                 | 1                  | Most metabolomics features are singly charged  |
| Representative isotope | Feature retained after isotope grouping                         | Lowest m/z         | Preferably retain the monoisotopic feature     |

### Quality Control

After isotope filtering, verify:

* reduction in the total number of features;
* retention of monoisotopic peaks;
* absence of excessive feature removal;
* consistency of isotope group assignments.

> **Important**
>
> The Isotope Filter only removes isotope-related redundancy. It does not remove adducts, in-source fragments, or other chemically redundant features.

## Step 7 – Join Aligner

### Menu

```text id="17fh6j"
Feature List Methods → Alignment → Join Aligner
```

### Objective

Align corresponding features across all samples and generate a unified feature matrix for downstream statistical analyses.

### How Does It Work?

Following feature detection, chromatographic resolution, and isotope filtering, each sample contains its own feature list. Due to small instrumental variations, the same compound may exhibit slight differences in m/z and retention time (RT) between samples.

The Join Aligner compares features across samples and calculates a match score primarily based on:

* m/z similarity;
* retention time similarity;
* user-defined weighting factors.

Features considered equivalent are merged into a single row in the final feature table. After alignment, these features represent the same compound across all samples.

### Parameters

| Parameter         | Description                                             | Orbitrap Example     | How to Define                                                                              |
| ----------------- | ------------------------------------------------------- | -------------------- | ------------------------------------------------------------------------------------------ |
| Feature lists     | Input feature lists                                     | `*deisotoped`        | Use the feature lists generated after isotope filtering.                                   |
| Feature list name | Output aligned feature list name                        | Aligned feature list | Record this name for subsequent processing steps.                                          |
| m/z tolerance     | Maximum mass difference allowed for alignment           | 0.002 m/z or 5 ppm   | Should reflect instrument mass accuracy. For Orbitrap instruments, 5 ppm is commonly used. |
| Weight for m/z    | Relative contribution of m/z to the alignment score     | 3                    | Higher values increase the importance of mass accuracy during alignment.                   |
| RT tolerance      | Maximum retention time difference allowed for alignment | 0.10 min             | Should reflect the chromatographic stability of the analytical method.                     |
| Weight for RT     | Relative contribution of RT to the alignment score      | 1                    | Higher values increase the influence of retention time during alignment.                   |

> **Important**
>
> For Orbitrap and other high-resolution instruments, exact mass is generally more reproducible than retention time. Therefore, m/z is commonly assigned a greater weight than RT during feature alignment.

> **Warning**
>
> Alignment is a critical preprocessing step. Errors at this stage may result in either incorrect merging of distinct compounds or the creation of multiple rows representing the same metabolite. QC samples should always be evaluated when defining appropriate m/z and RT tolerances.

### Quality Control

After alignment, verify:

* total number of aligned features;
* presence of the same feature across multiple samples;
* absence of incorrect feature merging;
* consistency among biological samples, QCs, and blanks.

> **Important**
>
> Alignment does not generate new features. It only associates equivalent features detected in different samples, enabling the construction of the feature matrix used for downstream statistical analyses.
>
> ## Step 8 – Gap Filling

### Menu

```text id="v1gt0v"
Feature List Methods
→ Gap filling / Recursive feature finding
→ Same RT and m/z range gap filler
```

> **Note:** In older versions of MZmine, this module was referred to as *Gap Filling (Peak Finder)*.

### Objective

Recover missing signals in the feature matrix that may have been lost during feature detection, chromatographic resolution, or alignment.

### How Does It Work?

After feature alignment, some features may be present in certain samples but absent in others.

These missing values may reflect:

* true biological absence of a metabolite;
* feature detection failures;
* chromatographic variability;
* signals close to the instrument detection limit.

Gap Filling revisits the raw data and searches for signals matching previously aligned features using their m/z and retention time information.

When a matching signal is found, its intensity is added to the feature matrix.

> **Important**
>
> Gap Filling does not create new metabolites.
>
> The algorithm only searches for signals corresponding to features that have already been detected and aligned.

### Parameters

| Parameter             | Orbitrap Example     | How to Define                                                                                                                                       |
| --------------------- | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Feature list          | Aligned feature list | Use the feature list generated by the Join Aligner. At this stage, there is a single aligned feature table rather than one feature list per sample. |
| Name suffix           | gap-filled           | Output suffix assigned to the gap-filled feature list.                                                                                              |
| m/z tolerance         | 0.002 m/z or 10 ppm  | Defines the search window used to recover missing signals. Should be compatible with instrument mass accuracy.                                      |
| Original feature list | KEEP                 | Retains the original aligned feature list and creates a new gap-filled feature list.                                                                |

### Quality Control

After Gap Filling, verify:

* reduction in the proportion of missing values;
* absence of excessive noise recovery;
* consistent behavior across biological samples, QCs, and blanks.

> **Important**
>
> Gap Filling should only be performed after feature alignment. Otherwise, there is no common m/z and retention time reference available to guide the search for missing signals.

---

## Step 9 – Export Feature List

### Menu

```text id="b5b0l9"
Feature List Methods
→ Export feature list
→ CSV export
```

### Objective

Export the processed feature matrix for downstream quality control, filtering, normalization, and statistical analyses in R.

### Procedure

1. Select the feature list generated after Gap Filling.
2. Navigate to:

```text id="r91y6f"
Feature List Methods
→ Export feature list
→ CSV export
```

3. Select the output location.
4. Export the feature table as a `.csv` file.

### Exported Matrix Structure

The exported table contains:

* feature identifiers;
* m/z values;
* retention times (RT);
* feature intensities for each sample;
* additional metadata generated during preprocessing.

Each row represents a detected feature, while each intensity column corresponds to an individual sample.

### Quality Control

After export, verify:

* all samples are present in the matrix;
* the number of exported features matches the final MZmine feature list;
* sample names have been exported correctly.

### Recommended Downstream Processing

After export, the following steps are recommended in R:

1. Blank feature filtering;
2. Feature prevalence filtering;
3. QC-based filtering (CV%);
4. Missing value treatment;
5. Data normalization;
6. Log transformation;
7. Exploratory analyses (PCA);
8. Supervised analyses (PLS-DA / OPLS-DA);
9. Univariate statistical testing;
10. Metabolite annotation and biological interpretation.

> **Note**
>
> MZmine is primarily intended for raw data preprocessing. Feature filtering, quality control, normalization, and statistical analyses are generally more reproducible and transparent when performed in R.
## References

1. MZmine Development Team. **Untargeted LC-MS Workflow – MZmine Documentation**. Available at: https://mzmine.github.io/mzmine_documentation/workflows/lcmsworkflow/lcms-workflow.html. Accessed June 2, 2026.

2. Schmid R, Heuckeroth S, Korf A, Smirnov A, Myers O, et al. Integrative analysis of multimodal mass spectrometry data in MZmine 3. *Nature Biotechnology*. 2023;41(4):447–449. doi:10.1038/s41587-023-01690-2.

3. Pluskal T, Korf A, Smirnov A, Schmid R, Fallon TR, Du X, et al. Metabolomics Data Analysis Using MZmine. In: Górecki T, Smirnov A, editors. *Processing Metabolomics and Proteomics Data with Open Software*. Cambridge: Royal Society of Chemistry; 2020. p. 232–254. doi:10.1039/9781788019880-00232.

4. Du X, Smirnov A, Pluskal T, Jia W, Sumner S. Metabolomics Data Preprocessing Using ADAP and MZmine 2. In: Li S, editor. *Computational Methods and Data Analysis for Metabolomics*. New York: Springer; 2020. p. 25–48.

5. Karaman I, Climaco Pinto R, Graça G. Metabolomics Data Preprocessing: From Raw Data to Features for Statistical Analysis. In: Raftery D, editor. *Comprehensive Analytical Chemistry*. Vol. 82. Amsterdam: Elsevier; 2018. p. 197–225.

---

## Suggested Citation

If you use this SOP in your work, please cite the original MZmine publications listed above as well as the official MZmine documentation.

## Version History

| Version | Date | Description |
|----------|----------|----------|
| 1.0 | 2026-06-02 | Initial release |
