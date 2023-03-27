Semantic segmentation of LandCover.ai dataset
==============================

The LandCover dataset consists of aerial images of urban and rural areas of Poland. The project focuses on the application of various neural networks for semantic segmentation, including the reconstruction of the neural network implemented by the authors of the dataset. 

The dataset used in this project is the [Landcover.ai Dataset](https://landcover.ai.linuxpolska.com/), 
which was originally published with [LandCover.ai: Dataset for Automatic Mapping of Buildings, Woodlands, Water and Roads from Aerial Imagery paper](https://arxiv.org/abs/2005.02264)
also accessible on [PapersWithCode](https://paperswithcode.com/paper/landcover-ai-dataset-for-automatic-mapping-of).

**Please note that I am not the author or owner of this dataset, and I am using it under the terms of the license specified by the original author. 
All credits for the dataset go to the original author and contributors.**

## Sample results

### High mIoU
![meanIoU_100 0_percent__4952](https://user-images.githubusercontent.com/29732555/201164572-ee6b56b6-b87f-4480-a52d-943678f5245b.png)
![meanIoU_73 03_percent__3929](https://user-images.githubusercontent.com/29732555/201164620-34ecbb4c-b6d4-4385-ac3b-0a9540842589.png)
![meanIoU_63 92_percent__5447](https://user-images.githubusercontent.com/29732555/201164668-920369ed-1c7c-45b6-96e0-6142ac71f1ba.png)

### Low mIoU
![meanIoU_6 12_percent__3183](https://user-images.githubusercontent.com/29732555/201164960-118c1efa-fb5b-496e-b8b6-2609c461a92f.png)
![meanIoU_23 39_percent__5703](https://user-images.githubusercontent.com/29732555/201165001-045e3f7a-9dac-4bce-b0f2-e457130f6f3c.png)


# Installation
The project provides two ways of running the project. The first is environment installed via Anaconda 
and the other is running a Docker container (recommended).

To be able to use dockerized version of Tensorflow, follow this official Nvidia's installation guide: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker 

## DockerHub Image for Linux
Ready image is accessible on Docker Hub.

### Pull image
```
docker pull mortentabaka/landcover_semantic_segmentation_with_deeplabv3plus:0.0.2
```
### Run image in interactive mode
```
docker run --gpus all -it -p 8888:8888 mortentabaka/landcover_semantic_segmentation_with_deeplabv3plus:0.0.2

```
### Run the image and create files locally
```
export PROJECT_PATH_LOCALLY="/path/to/local/code/directory" && 
git clone https://github.com/MortenTabaka/Semantic-segmentation-of-LandCover.ai-dataset.git "$PROJECT_PATH_LOCALLY" --branch v.0.0.2 &&
docker run --gpus all -it -p 8888:8888 -v $PROJECT_PATH_LOCALLY:/app/ mortentabaka/landcover_semantic_segmentation_with_deeplabv3plus:0.0.2
```

## Dockerfile - Tensorflow GPU (recommended)
Clone the repository
```
git clone git@github.com:MortenTabaka/Semantic-segmentation-of-LandCover.ai-dataset.git && cd Semantic-segmentation-of-LandCover.ai-dataset/
```

Build docker image with project's [Dockerfile](https://github.com/MortenTabaka/Semantic-segmentation-of-LandCover.ai-dataset/blob/main/Dockerfile):
```
docker build -t landcover_semantic_segmentation .
```
Official image was used as a base: https://hub.docker.com/layers/tensorflow/tensorflow/2.5.1-gpu-jupyter/images/sha256-5cdcd4446fc110817e5f6c5784eba6254258632036b426b9f194087e200f8a96?context=explore

Run Jupyter Notebook with:
```
docker run --gpus all -it --rm -p 8888:8888 -v $(pwd):/app landcover_semantic_segmentation
```
If port `8888` is already in use, then change its value, e.g. `-p 5555:8888`. 
Remember to manually replace port in a link to the chosen value:

Would be: `http://127.0.0.1:8888/?token=...`

Should be: `http://127.0.0.1:5555/?token=...`

## Dockerfile - Tensorflow CPU (not tested)
Clone the repository
```
git clone git@github.com:MortenTabaka/Semantic-segmentation-of-LandCover.ai-dataset.git && cd Semantic-segmentation-of-LandCover.ai-dataset/
```

In [Dockerfile](https://github.com/MortenTabaka/Semantic-segmentation-of-LandCover.ai-dataset/blob/main/Dockerfile) 
change tensorflow image name to `tensorflow/tensorflow:2.5.1-jupyter`.

Build a image.
```
docker build -t landcover_semantic_segmentation .
```

To run the image, do not use flag `--gpus all`:

```
docker run -it --rm -p 8888:8888 -v $(pwd):/app landcover_semantic_segmentation
```

## Anaconda Enviornment (legacy)
[Installation guide - Legacy](https://github.com/MortenTabaka/Semantic-segmentation-of-LandCover.ai-dataset/blob/main/INSTALLATION.md)

# Jupyter Notebooks

## [Development notebooks](https://drive.google.com/drive/folders/105HjfaU6_3NHRozYWXR9IjKiKKJRW5ez?usp=sharing)
Jupyter notebooks used in early-stage development.

## [Templates](https://github.com/MortenTabaka/Semantic-segmentation-of-LandCover.ai-dataset/tree/main/notebooks/templates)
Jupyter notebook templates for machine learning operations in the project.
### Available templates
   * [Training a new model.](https://github.com/MortenTabaka/Semantic-segmentation-of-LandCover.ai-dataset/blob/main/notebooks/templates/Model_training.ipynb)
   * [Predict and plot masks, generated by model with downloaded best weights.](https://github.com/MortenTabaka/Semantic-segmentation-of-LandCover.ai-dataset/blob/main/notebooks/templates/Plot_predictions.ipynb)

# Data exploration

1. Check the class convention in the mask (Notebook v.1.0).

2. Preparing images for training (Notebook v.2.0).

# Model exploration

## DeepLabv3+ architecture

[Developemnt notebooks](https://drive.google.com/drive/folders/105HjfaU6_3NHRozYWXR9IjKiKKJRW5ez?usp=sharing)

| Ver. | Backbone                     | Weights    | Frozen convolution base | Loss function | Data augmentation | Train dataset size | Loss weights | mIoU on test dataset |
| --- |------------------------------|------------| --- | --- | --- | --- | --- | --- |
| 5.1 | Tensorflow Xception          | Imagenet   | Yes | Sparse Categorical Crossentropy | No | 7470 | No | 0.587 | 
| 5.2 | Tensorflow Xception | Imagenet   | Yes | Sparse Categorical Crossentropy | Yes | 14940 | No | 0.423 |
| 5.3 | Tensorflow Xception          | Imagenet   | Yes | Sparse Categorical Crossentropy | No | 7470 | Yes | 0.542 |
| 5.4 | Modified Xception            | Cityscapes | Yes | Sparse Categorical Crossentropy | No | 7470 | No | 0.549 |
| 5.4 | Modified Xception            | Cityscapes | Yes | Sparse Categorical Crossentropy | No | 7470 | Yes | 0.562 |
| 5.5 | Modified Xception            | Cityscapes | Yes | Sparse Categorical Crossentropy | No | 7470 | Yes | 0.567 |
| 5.6 | Modified Xception            | Cityscapes | Yes | Sparse Categorical Crossentropy | No | 7470 | Yes | 0.536 |
| 5.7 | Modified Xception            | Cityscapes | No | Sparse Categorical Crossentropy | No | 7470 | Yes | 0.359 |
| 5.8 | Modified Xception            | Cityscapes | Yes | Soft Dice Loss | No | 7470 | No | 0.559 |
| 5.9 | Modified Xception            | Pascal VOC | Partially | Soft Dice Loss | No | 7470 | No | 0.607 |
| **5.10** | Modified Xception            | Cityscapes | Partially | Soft Dice Loss | No | 7470 | No | **0.718** |
| 5.11 | Modified Xception            | Cityscapes | Partially | Soft Dice Loss | Yes | 14940 | No | 0.659 |
| 5.12 | Modified Xception            | Cityscapes | Partially | Soft Dice Loss | Yes | 7470 | No | 0.652 |

## Currently best mIoU score

![alt text](https://github.com/MortenTabaka/Semantic-segmentation-for-LandCover.ai-dataset/blob/main/reports/figures/Currently_best_classes_iou.png)

**Notebook v.5.10** with **meanIoU = 0.718**.

Notebooks are available [**on Google Drive**](https://drive.google.com/drive/folders/105HjfaU6_3NHRozYWXR9IjKiKKJRW5ez?usp=sharing).

## References
<a id="1">[1]</a> 
Boguszewski, Adrian and Batorski, Dominik and Ziemba-Jankowska, Natalia and Dziedzic, Tomasz and Zambrzycka, Anna (2021). ["LandCover.ai: Dataset for Automatic Mapping of Buildings, Woodlands, Water and Roads from Aerial Imagery"](https://arxiv.org/abs/2005.02264v2)

<a id="2">[2]</a> 
A. Abdollahi, B. Pradhan, G. Sharma, K. N. A. Maulud and A. Alamri, ["Improving Road Semantic Segmentation Using Generative Adversarial Network,"](https://ieeexplore.ieee.org/document/9416669) in IEEE Access, vol. 9, pp. 64381-64392, 2021, doi: 10.1109/ACCESS.2021.3075951.


Project Organization
------------

    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make data` or `make train`
    ├── README.md          <- The top-level README for developers using this project.
    ├── data
    │   ├── processed      <- The final, canonical data sets for modeling.
    │   └── raw            <- The original, immutable data dump.
    │
    ├── docs               <- A default Sphinx project; see sphinx-doc.org for details
    │
    ├── models             <- Trained and serialized models, model predictions, or model summaries
    │
    ├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
    │                         the creator's initials, and a short `-` delimited description, e.g.
    │                         `1.0-jqp-initial-data-exploration`.
    │
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └── figures        <- Generated graphics and figures to be used in reporting
    │
    ├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
    │                         generated with `pip freeze > requirements.txt`
    │
    ├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
    ├── src                <- Source code for use in this project.
    │   ├── __init__.py    <- Makes src a Python module
    │   │
    │   ├── data           <- Scripts to download or generate data
    │   │   └── make_dataset.py
    │   │
    │   ├── features       <- Scripts to turn raw data into features for modeling
    │   │   └── build_features.py
    │   │
    │   ├── models         <- Scripts to train models and then use trained models to make
    │   │   │                 predictions
    |   |   ├── architectures      <- Model architectures available for training
    │   │   ├── predict_model.py   
    │   │   └── model_builder.py
    │   │
    │   └── visualization  <- Scripts to create exploratory and results oriented visualizations
    │       └── visualize.py
    │
    └── tox.ini            <- tox file with settings for running tox; see tox.readthedocs.io


--------

## Citation
If you use this software, please cite it using these metadata.
```
@software{Tabaka_Semantic_segmentation_of_2021,
author = {Tabaka, Marcin Jarosław},
license = {Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)},
month = {11},
title = {{Semantic segmentation of LandCover.ai dataset}},
url = {https://github.com/MortenTabaka/Semantic-segmentation-of-LandCover.ai-dataset},
year = {2021}
}
```

<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>

Shield: [![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg
