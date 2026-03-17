# Real-time Anomaly Detection for Liquid Argon Time Projection Chambers
[![arXiv](https://img.shields.io/badge/arXiv-2512.06208-b31b1b.svg?style=flat-square)](https://arxiv.org/abs/2509.21817)

1. Input Data and preprocessing: https://github.com/uboone/OpenSamples
  Use the hdf5 files, `Inclusive, WithWire`

  Use `ubopendata` conda environment for this part

  The `wire_table` contains (Wire, Timetick, ADC) values. I used plane2, the collection plane
  The input data is preprocessed to reduce the model size.
  My naming scheme is AXB, where A and B are the division factors for Timetick and Wire respectively.
  The Timetick dimension is downsampled by a factor of 10 before division

  ex: 10X4
  Original input size values for single event is (Wire, Timetick) = (3456, 6400) --> (3456,640) after downsampling --> (864,64) after division
  
  hdf5 files are preprocessed into npy files through `01_data2npy_full.py`. Samples are inside `inputData`

2. Training Teacher
  Use `02_train_teacher.py`
  Use `ubqkeras` conda environment for step 2 to 4

3. Training Student
  Use `03_train_student.py`
  Requires Teacher from step 2.
  
  Trained models are saved in `savedModel`

4. Evaluating Loss (or 'Anomaly Score')
  Use `04_getLoss.py`
  Evaluates loss (or Anomlay Score in our language) and saves into npy file

5. Dependencies
  Use `conda_envs`
  If one has to install the environments from scratch, the `.txt` folders have dependencies.
  
  `file.py`, `microboone_utils.py` are obtained from https://github.com/uboone/OpenSamples
  `QDenseBatchnorm.py` is in this branch of qkeras https://github.com/google/qkeras/pull/74

6. Models: Taken from https://github.com/Princeton-AD/cicada/blob/main/models.py

  Defined in `models.py`
  
  `teacher_reshape` and `teacher_dense`, `teacher_reshape2` values need to be changed according with the prepocessed input image size
