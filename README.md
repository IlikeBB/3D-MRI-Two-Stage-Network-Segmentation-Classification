# Toward Explainable Semantic Segmentation Guided Detector for Segmentation and Classification of Diffusion-Weighted MRI of Acute Ischemic Stroke
## Abstract
#### Background and Purpose
> Neuroimaging is valuable for clinical decision support in acute ischemic stroke (AIS). Diffusion-weighted imaging (DWI) is the representative MRI sequence to timely, sensitively, and accurately reflect ischemia injuries to brain tissue. However, the complexity of MRI elevates the threshold for instantaneously precise interpretation of the images. Therefore, this work leverage machine learning to segment and classify the AIS lesions on DWI images. We focused on model development, performance comparisons, and utilization for decision making.
#### Methods
> We enrolled brain MRI images of AIS during 2017-2020 in a tertiary teaching hospital. DWI were analyzed by the network for semantic segmentation guided detector (SGD-Net), composed of a U-shaped stage 1 (S1) model for segmentation and a stage 2 (S2) model for classification. An image review board labeled images to provide ground truth for model training and testing. The binary classes in S2 were AIS lesion size (lacune vs. non-lacune) and circulatory territory of lesion location (anterior vs. posterior circulation). We compared different backbones of SGD-Net and contrasted them with one-stage classifiers.
#### Conclusions
> The SGD-Net precisely segmented AIS lesions on DWI and accurately classified lesion size and location. Based on the two-stage design, combinations of different backbones for S1 U-Net and S2 classifier performed superiorly to the one-stage 3D-ResNet and 3D-CNNs. The U-shaped architecture of S1 was considered crucial for satisfactory model performance. In the future, the deployment of SGD-Net to the emergency health care system would improve AIS patient care. 

## Experiment Environment

```
OS: Ubuntu 16.04
CUDA: 10.1
GPU: Nvidia GTX 1080Ti x 1
```
## Build Virtual Environment (Using Conda)

```
!conda env create -f SGD.yml

!conda activate SGD
```
## Model Architecture
<img src='https://github.com/IlikeBB/F3DD/blob/main/plot_results/%E6%9E%B6%E6%A7%8B%E5%9C%96.jpg'>

## Data Preparing
```
!pip install nibabel
!pip install scikit-image
```
> * Loading nii or nii.gz data and transformation to numpy data array.
> * Please refer to `00.nii data loading.ipynb` build your dataset array or build random data array.
```
 example:
 ./data_set/...
     1| 00001.nii -> 00001.npy
     2| 00002.nii -> 00002.npy
     3| 00003.nii -> 00003.npy
                  .
                  .
     n|     n.nii ->     n.npy
```
```
concate data array-> [Sample Number, depth, width, height] ([140,32,384,384])
```

## Training
```
!pip install git+https://github.com/mjkvaak/ImageDataAugmentor
```
### Training S1 Semantic Segmentation Network
```
!python 01.S1_Training.py
```
> * Edit env parameter value in `/utils/model_config_S1.yam`.

### Training S2 3D Classification Network

```
!python 02.S2_AP_Training.py
!python 02.S2_NL_Training.py
```
> * Edit env parameter value in `/utils/model_config_NL or AP.yaml`.

## Testing
```
!pip install -U scikit-learn
```
> * Please refer to `03.evaluate.ipynb`
> * Edit env parameter value in `/utils/model_config.yaml`.

## Perfromace Plot
<img src='https://github.com/IlikeBB/F3DD/blob/main/plot_results/fig4-5%20revise%20table.001.png'>
<img src='https://github.com/IlikeBB/F3DD/blob/main/plot_results/fig4-5%20revise%20table.002.png'>

## Visual Results
<img src='https://github.com/IlikeBB/F3DD/blob/main/plot_results/is0302.gif'></p>
<img src='https://github.com/IlikeBB/F3DD/blob/main/plot_results/is0323.gif'></p>
<img src='https://github.com/IlikeBB/F3DD/blob/main/plot_results/is0319.gif'></p>
<img src='https://github.com/IlikeBB/F3DD/blob/main/plot_results/is0337.gif'></p>

## Reference
#### Data Augmentation reference from<a href='https://github.com/albumentations-team/albumentations'> Albumentations</a>, <a href='https://github.com/mjkvaak/ImageDataAugmentor'> ImageDataAugmentor</a>.
#### The code is heavily adapted from<a href='https://github.com/JihongJu/keras-resnet3d'> 3D ResNet</a>, <a href='https://github.com/qubvel/segmentation_models'> Segmentation Models</a>.
