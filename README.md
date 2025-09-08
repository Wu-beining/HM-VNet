>📋 For code accompanying Hierarchical Multi-modal Vision Network of the paper

# HM-VNet: A Hierarchical Multi-modal Vision Network for Head-and-Neck Tumor Segmentation and Survival Prediction


This repository provides the official implementation of our submission to the **HECKTOR 2025 Challenge**. We present a unified framework that leverages PET/CT imaging to address two core tasks in head-and-neck cancer analysis: (1) **automatic segmentation** of primary tumors and metastatic lymph nodes, and (2) **recurrence-free survival (RFS) prediction**. 

Our approach employs a **Hierarchical Multi-modal Vision Network (HM-VNet)** to achieve precise segmentation and a **deep multi-modal fusion network** to deliver robust survival analysis. On the HECKTOR challenge dataset, the framework demonstrates **state-of-the-art** performance across both tasks.

## 💡Primary contributions

1) 🕐 **Task 1 – Segmentation (HM-VNet):**
We introduce HM-VNet, a 3D Vision Transformer–based architecture that effectively fuses the complementary information in PET and CT. Compared with representative baselines such as **SegResNet** and **SwinUNETR**, HM-VNet achieves substantially higher segmentation accuracy.

2) 🕑**Task 2 – Survival Prediction:**
We design a **deep multi-modal fusion network** that integrates raw imaging (PET/CT), clinical tabular variables, and lesion features extracted by HM-VNet. Coupled with advanced survival modeling modules, our method yields risk scores that outperform traditional approaches such as **CoxPH** and **ICARE**.



## Proposed method

We integrate the two tasks within a single pipeline. First, the segmentation model extracts lesion-centric representations; these features are then fused with the original imaging modalities and clinical tabular data to produce the final survival risk estimates.

<br><br>
![](https://github.com/Wu-beining/HM-VNet/blob/main/img/HECKTORv3.png)
<br><br>

## Table of Contents
- [Requirements](#-Requirements)
- [Training](#-Training)
- [Evaluation](#-Evaluation)
- [Results](#-Results)
- [Contributing](#-Contributing)

## 📝 Requirements

To install requirements:

```setup
pip install -r requirements.txt
```

## 🔥 Training

To train our model in the paper, run this command:

```train
python train.py
```

>📋 Before training, specify the data set and training configuration using the config.xml file

## 📃 Evaluation

To evaluate our model in the paper, run this command:

```eval
python eval.py
```

<
