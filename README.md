# Dynamic Functional Connectivity of Escitalopram and Psilocybin Response in Major Depressive Disorder

This repository contains the Python notebooks used for the analyses presented in the accompanying MSc research report. The workflow describes clinical outcomes using the QIDS-SR-16, assesses data quality, extracts regional resting-state fMRI time series and analyses instantaneous phase dynamics using synchrony, metastability, Leading Eigenvector Dynamics Analysis (LEiDA) and Eigenvector Dynamics Analysis (EiDA).

## Repository contents

```text
.
├── README.md
├── requirements.txt
├── notebooks/
│   ├── qids.ipynb
│   ├── motion_qc.ipynb
│   ├── parcellation.ipynb
│   ├── synchrony_metastability.ipynb
│   └── leida_eida.ipynb
└── atlases/
    ├── README.md
    └── Schaefer2018_100Parcels_7Networks_order_Tian_Subcortex_S1_label.txt
```

## Analysis workflow

The notebooks should be read and, where data access permits, run in the following order.

| Notebook | Description |
| --- | --- |
| `qids.ipynb` | Describes pre-treatment and post-treatment QIDS-SR-16 scores, plots their distributions, and compares changes between treatment groups. |
| `motion_qc.ipynb` | Calculates the percentage of frames with framewise displacement greater than or equal to 0.5 mm and displays the worst scan for each participant. |
| `parcellation.ipynb` | Applies the combined Schaefer-100 cortical and Tian-16 subcortical atlas, displays the cortical networks and subcortical regions, and extracts the 116 regional time series. |
| `synchrony_metastability.ipynb` | Calculates whole-brain and network-level synchrony and metastability and performs the associated statistical analyses. |
| `leida_eida.ipynb` | Performs LEiDA and EiDA clustering, compares the information retained by the two representations, and analyses fractional occupancy and dwell time. |

The parcellation notebook produces `parcellated_timeseries.npz`, which is the input to both `04_synchrony_metastability.ipynb` and `05_leida_eida.ipynb`. The saving cell is commented out by default it must be uncommented if this processed file is to be written.

## Data availability

The individual-level imaging and clinical data from the trial are not included in this repository. This data is protected by the Centre for Psychedelic Research at Imperial College London and they should be contacted for access.

The analysis expects a clinical table with one row per participant and the following columns:

| Column | Description |
| --- | --- |
| `subject` | Participant number |
| `treatment` | Treatment allocation (`E` or `P`, or the corresponding full treatment name) |
| `qids_pre` | Pre-treatment QIDS-SR-16 score |
| `qids_post` | Post-treatment QIDS-SR-16 score |

The imaging notebooks expect cleaned resting-state fMRI images and fMRIPrep confound files arranged by participant and session. Paths at the beginning of each notebook should be changed to the local data location. The expected functional-image naming convention is:

```text
sub-<subject>/ses-<session>/func/
    cleaned_sub-<subject>_ses-<session>_task-rest_space-MNI152NLin2009cAsym.nii.gz
```

The motion_qc.ipynb notebook expects fMRIPrep confound files matching:

```text
sub-<subject>/ses-<session>/func/
    *_task-rest_run-*_desc-confounds_timeseries.tsv
```
These can be found in the RDS of the trial upon access being granted.

## Atlases

Parcellation uses a combined atlas containing 100 cortical parcels from the Schaefer atlas and 16 subcortical regions from the Melbourne/Tian atlas. Cortical parcels are assigned to the seven Yeo functional networks.

The combined parcellation atlas is downloaded by `03_parcellation.ipynb` if it is not already present. The parcel-label text file should be placed in `atlases/`.

The atlas visualisation additionally requires the following Melbourne subcortical atlas files:

```text
melbourne_subcortex_scale1.nii
melbourne_subcortex_scale1.json
```

These files must be downloaded separately from the Melbourne Subcortex Atlas [website](https://www.nitrc.org/projects/msa/) and placed in the location specified at the beginning of `03_parcellation.ipynb`.

The atlases and network definitions are described in:

- Schaefer, A. et al. (2018). Local-global parcellation of the human cerebral cortex from intrinsic functional connectivity MRI. *Cerebral Cortex*, 28(9), 3095–3114. https://doi.org/10.1093/cercor/bhx179
- Yeo, B. T. T. et al. (2011). The organization of the human cerebral cortex estimated by intrinsic functional connectivity. *Journal of Neurophysiology*, 106(3), 1125–1165. https://doi.org/10.1152/jn.00338.2011
- Tian, Y. et al. (2020). Topographic organization of the human subcortex unveiled with functional connectivity gradients. *Nature Neuroscience*, 23, 1421–1432. https://doi.org/10.1038/s41593-020-00711-6

## Software

The notebooks use the following third-party software.

| Package | Use in this repository |
| --- | --- |
| [NumPy](https://numpy.org/) | Numerical arrays, linear algebra, eigenvalue calculations, simulation and permutation procedures |
| [pandas](https://pandas.pydata.org/) | Clinical data, scan metadata and result tables |
| [SciPy](https://scipy.org/) | Hilbert transforms, statistical tests, optimisation and special functions |
| [Matplotlib](https://matplotlib.org/) | Figures and plot formatting |
| [seaborn](https://seaborn.pydata.org/) | Annotated heatmaps |
| [statsmodels](https://www.statsmodels.org/) | Regression models, diagnostics and multiple-testing correction |
| [scikit-learn](https://scikit-learn.org/) | Clustering initialisation, confusion matrices and cosine similarity |
| [NiBabel](https://nipy.org/nibabel/) | Reading and constructing NIfTI images |
| [Nilearn](https://nilearn.github.io/) | Atlas retrieval, image resampling, parcellation and brain visualisation |
| [NiChord](https://github.com/paulcbogdan/NiChord) | Chord diagrams of state connectivity |
| [Requests](https://requests.readthedocs.io/) | Downloading the combined parcellation atlas |
| [IPython](https://ipython.org/) | Rich table and Markdown display within notebooks |
| [JupyterLab](https://jupyterlab.readthedocs.io/) | Interactive execution of the notebooks |
