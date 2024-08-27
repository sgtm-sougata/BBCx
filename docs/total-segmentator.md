
# TotalSegmentator
[Total Segmentator](https://github.com/wasserth/TotalSegmentator?tab=readme-ov-file#totalsegmentator) is a open source automated
structure segmentation module and it segment organ at risk of CT and MRI modality images.

It was trained on CT and MRI images from different scanners, institutions, and protocols, including approximately 1,228 CT subjects and 298 MRI subjects. [Segmented Structure Name](https://github.com/wasserth/TotalSegmentator?tab=readme-ov-file#class-details)

## Selected Structure
```yaml
study: BBCx
version: v1
structure:
  1: spleen
  2: kidney_right
  3: kidney_left
  4: gallbladder
  5: liver
  6: stomach
  7: pancreas
  18: small_bowel
  19: duodenum
  20: colon
  21: urinary_bladder
  25: sacrum
  26: vertebrae_S1
  27: vertebrae_L5
  28: vertebrae_L4
  29: vertebrae_L3
  30: vertebrae_L2
  31: vertebrae_L1
  77: hip_left
  78: hip_right
```
We are using python [TotalSegmentator](https://pypi.org/project/TotalSegmentator/) module to segment CT images.

## Installation
Activate your python environment and go to terminal and write `pip install TotalSegmentator` if already installed go to the python code 
```python
from totalsegmentator.python_api import totalsegmentator
```
It execute successfully go to the next step segmentation.

## Segmentation
Before jump into the final segmentation code at first go to the github page of the totalsegmentator and knowing the usefull 
parameters or settings.

* `--task` : total
* `--device`: Choose cpu or gpu or gpu:X (e.g., gpu:1 -> cuda:1)
* `--fast`: For faster runtime and less memory requirements use this option. It will run a lower resolution model (3mm instead of 1.5mm).
* `--body_seg`: This will crop the image to the body region before processing it
* `--roi_subset`: Takes a space-separated list of class names (e.g. spleen colon brain) and only predicts those classes. Saves a lot of runtime and memory. Might be less accurate especially for small classes (e.g. prostate).
* `--preview`: This will generate a 3D rendering of all classes, giving you a quick overview if the segmentation worked and where it failed (see preview.png in output directory).
* `--ml`: This will save one nifti file containing all labels instead of one file for each class. Saves runtime during saving of nifti files. (see here for index to class name mapping).
* `--statistics`: This will generate a file statistics.json with volume (in mm³) and mean intensity of each class.
--radiomics: This will generate a file statistics_radiomics.json with the radiomics features of each class. You have to install pyradiomics to use this (pip install pyradiomics).

* `--force_split`: This will split the image into 3 parts and process them one after another. (Do not use this for small images. Splitting these into even smaller images will result in a field of view which is too small.)

* `--nr_thr_saving 1`: Saving big images with several threads will take a lot of memory


But in our case we take high resolution so avoid --fast parameter device as gpu and --roi_subset is the selected structure,
make sure this structure as a list comma seperated format.

```python
organs_and_bones = [
    "kidney_right", "kidney_left", "gallbladder", "liver", "stomach",
    "pancreas", "small_bowel", "duodenum", "colon", "urinary_bladder",
    "sacrum", "vertebrae_S1", "vertebrae_L5", "vertebrae_L4", "vertebrae_L3",
    "vertebrae_L2", "vertebrae_L1", "hip_left", "hip_right"
]

def segmentation():
    nifti_file_dir = os.path.join(os.getcwd(), "segmentation")
    for dir in os.listdir(nifti_file_dir):
        nifti_file_path = os.path.join(
            nifti_file_dir, dir
        )
        nifti_files = os.path.join(nifti_file_path, "nifti")
        for nifti in os.listdir(nifti_files):
            if nifti.endswith(".nii.gz"):
                nifti_file = os.path.join(nifti_files, nifti)
                input_img = nib.load(nifti_file)
                output_dir = os.path.join(nifti_file_path, "mask")
                if os.path.exists(output_dir):
                    shutil.rmtree(output_dir)
                os.makedirs(output_dir)
                totalsegmentator(
                    input_img,
                    output_dir,
                    device="gpu",
                    task="total",
                    roi_subset=organs_and_bones
                )

```

