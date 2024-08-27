
# Dicom Preparation

In radiotherapy treatment, CT images are frequently used, and the number of CT slices depends on the treatment's anatomic site as well as the patient's physical condition. Each CT slice represents 2D information, while the series of CT slices together provide 3D information. Generally, CT files are saved in DICOM format, which is widely used for storing medical images.

In radiotherapy, there are three different DICOM modalities associated with treatment: RTSTRUCT, PLAN, and DOSE. For our BBCx study, we are exporting all of these modalities images.

## Directory Structure
In our root dir it have multiple dicom images may or may not be organize structure our aim is to modify the structure and it looks like

```bash
root_dir
├── patient_1
│   ├── CT
│   │   ├── ct_file_1.dcm
│   │   ├── ct_file_2.dcm
│   │   ├── ct_file_3.dcm
│   │   └── ct_file_n.dcm
│   ├── RTSTRUCT
│   │   └── rtstruct_file.dcm
│   ├── DOSE
│   │   └── dose_file.dcm
│   └── PLAN
│       └── plan_file.dcm
└── patient_2
    ├── CT
    │   ├── ct_file_1.dcm
    │   ├── ct_file_2.dcm
    │   └── ct_file_n.dcm
    ├── RTSTRUCT
    │   └── rtstruct_file.dcm
    ├── DOSE
    │   └── dose_file.dcm
    └── PLAN
        └── plan_file.dcm
```
Patient folder seperated by SOP Instance UID and inside CT, RTSTRUCT, PLAN and DOSE are seperated by dicom modality.

```python
"python code"
```
Now dicom data looks organize and easy to connect and readable each modality all the patients.

## Dicom to NIFTI
NIFTI Neuroimaging Informatics Technology Initiative and its a simpler structure which facilitates easier manipulation 
and analysis of imaging data commonly uses in research.

One of the most common python packages is [dicom2nifti](https://dicom2nifti.readthedocs.io/en/latest/), which we use as a base.
```python

```


