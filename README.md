# Real-time Anomaly Detection for Liquid Argon Time Projection Chambers

[![arXiv](https://img.shields.io/badge/arXiv-2512.06208-b31b1b.svg?style=flat-square)](https://arxiv.org/abs/2509.21817)

## 1. Input Data & Preprocessing

**Source:** https://github.com/uboone/OpenSamples\
Use the HDF5 files: `Inclusive, WithWire`\
Use the `ubopendata` conda environment for this step.

### Data Description

The `wire_table` contains:

    (Wire, Timetick, ADC)

-   Plane used: **plane2 (collection plane)**
-   Input data is preprocessed to reduce model size
-   Naming scheme: **AXB**
    -   A = Timetick division factor
    -   B = Wire division factor
-   Timetick dimension is first downsampled by a factor of **10**, then
    divided

### Example: `10X4`

Original single-event input:

    (Wire, Timetick) = (3456, 6400)

After downsampling:

    (3456, 640)

After division:

    (864, 64)

### Conversion to NumPy

HDF5 files are converted to `.npy` using:

    01_data2npy_full.py

Output samples are stored in:

    inputData/

------------------------------------------------------------------------

## 2. Train Teacher Model

Script:

    02_train_teacher.py

Use the `ubqkeras` conda environment (Steps 2--4).

------------------------------------------------------------------------

## 3. Train Student Model

Script:

    03_train_student.py

**Requirements:**

-   A trained Teacher model from Step 2

Output models are saved in:

    savedModel/

------------------------------------------------------------------------

## 4. Evaluate Loss (Anomaly Score)

Script:

    04_getLoss.py

**Function:**

-   Computes loss (referred to as **Anomaly Score**)
-   Saves results as `.npy` files

------------------------------------------------------------------------

## 5. Dependencies

Conda environments are provided in:

    conda_envs/

If installing from scratch:

-   Use the `.txt` files for dependency lists

### External Files

From OpenSamples:

-   `file.py`
-   `microboone_utils.py`\
    https://github.com/uboone/OpenSamples

From QKeras (custom branch):

-   `QDenseBatchnorm.py`\
    https://github.com/google/qkeras/pull/74

------------------------------------------------------------------------

## 6. Models

Base implementation:

https://github.com/Princeton-AD/cicada/blob/main/models.py

Local definitions:

    models.py

### Important Configuration

The following parameters must match the preprocessed input image size:

-   `teacher_reshape`
-   `teacher_dense`
-   `teacher_reshape2`
