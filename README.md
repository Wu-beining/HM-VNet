# HM-VNet

This repository provides the official implementation of our submission to the **HECKTOR 2025 Challenge**. We present a unified framework that leverages PET/CT imaging to address two core tasks in head-and-neck cancer analysis: (1) **automatic segmentation** of primary tumors and metastatic lymph nodes, and (2) **recurrence-free survival (RFS) prediction**. 

Our approach employs a **Hierarchical Multi-modal Vision Network (HM-VNet)** to achieve precise segmentation and a **deep multi-modal fusion network** to deliver robust survival analysis. On the HECKTOR challenge dataset, the framework demonstrates **state-of-the-art** performance across both tasks.

## 💡Primary contributions

1) 🕐 **Task 1 – Segmentation (HM-VNet):**
We introduce HM-VNet, a 3D Vision Transformer–based architecture that effectively fuses the complementary information in PET and CT. Compared with representative baselines such as **SegResNet** and **SwinUNETR**, HM-VNet achieves substantially higher segmentation accuracy.

2) 🕑**Task 2 – Survival Prediction:**
We design a **deep multi-modal fusion network** that integrates raw imaging (PET/CT), clinical tabular variables, and lesion features extracted by HM-VNet. Coupled with advanced survival modeling modules, our method yields risk scores that outperform traditional approaches such as **CoxPH** and **ICARE**.



## Proposed method

We integrate the two tasks within a single pipeline. First, the segmentation model extracts lesion-centric representations; these features are then fused with the original imaging modalities and clinical tabular data to produce the final survival risk estimates.
![](https://github.com/Wu-beining/HM-VNet/blob/main/img/HECKTORv3.png)

## Contents

- Requirements  
- Dataset Preparation  
- Training  
- Evaluation  
- Results  
- Citation

## Requirements

We recommend **Python ≥ 3.8** and **PyTorch ≥ 1.10**. Install all dependencies via:

```bash
pip install -r requirements.txt
```

A recommended `requirements.txt` is provided, curated according to the paper and common deep-learning practice.

## Dataset Preparation

1. Download the dataset from the **HECKTOR 2025** official website.  
2. Follow the preprocessing pipeline described in Section 2.2 of the paper, including **resampling**, **region-of-interest (ROI) cropping**, and **intensity normalization**.  
3. Place the processed data under `./data` using the following layout:

```
./data/
└── HECKTOR_2025/
    ├── train/
    │   ├── patient_001/
    │   │   ├── patient_001_ct.nii.gz
    │   │   ├── patient_001_pt.nii.gz
    │   │   └── patient_001_gtvt.nii.gz
    │   └── ...
    └── test/
        ├── patient_701/
        │   ├── patient_701_ct.nii.gz
        │   └── patient_701_pt.nii.gz
        └── ...
```

## Training

Ensure paths are correctly configured in the YAML files before launching training.

**Task 1 – Segmentation (HM-VNet)**

```bash
python train_task1.py --config configs/task1_config.yml
```

**Task 2 – Survival Prediction**

```bash
python train_task2.py --config configs/task2_config.yml
```

## Evaluation

Use the provided scripts to generate predictions on the test set with your trained weights.

```bash
python evaluate.py --task 1 --model_path /path/to/task1_model.pth --data_dir ./data/HECKTOR_2025/test/
python evaluate.py --task 2 --model_path /path/to/task2_model.pth --data_dir ./data/HECKTOR_2025/test/
```

## Results

Our model achieves **strong performance on the HECKTOR 2025 validation set**, surpassing both baseline and advanced methods.

### Task 1 – Segmentation Performance

HM-VNet performs robustly on both **GTVp** (primary gross tumor volume) and **GTVn** (metastatic lymph nodes). Notably, for the more challenging GTVn segmentation, it attains an **aggregated Dice** of **0.7452**, evidencing strong capability in segmenting small, scattered targets.

**Table 1. Segmentation results on HECKTOR 2025 validation set**

| Method         | GTVp Mean Dice | GTVn Aggregated Dice | GTVn Aggregated F1-Score |
|:---------------|:--------------:|:--------------------:|:------------------------:|
| SegResNet      | 0.6621         | 0.7181               | 0.5297                   |
| SwinUNETR      | 0.6673         | 0.7053               | 0.1713                   |
| Restormer      | 0.6589         | 0.7024               | 0.3439                   |
| MICFormer      | 0.6521         | 0.6802               | 0.2157                   |
| H-Denseformer  | 0.6453         | 0.6826               | 0.4050                   |
| MMCA-Net       | 0.6437         | 0.6671               | 0.2507                   |
| H2ASeg         | 0.5824         | 0.6855               | _0.4756_                 |
| AIMERS         | 0.5250         | 0.5110               | 0.3849                   |
| **HM-VNet (Ours)** | **0.6833** | **0.7452**           | 0.4544                   |

### Task 2 – Survival Prediction Performance

Our multi-modal fusion network achieves a leading **C-index** for survival prediction. When incorporating Task-1-derived segmentation features as additional inputs, the C-index improves from _0.6826_ to **0.7045**, highlighting the synergy between the two stages.

**Table 2. Survival prediction results on HECKTOR 2025 validation set**

| Model                 | C-index             |
|:----------------------|:-------------------:|
| CoxPH                 | 0.6073 ± 0.0637     |
| DeepHit               | 0.5457 ± 0.0674     |
| MTLR                  | 0.5877 ± 0.0900     |
| ICARE                 | 0.6705 ± 0.0608     |
| HMT                   | 0.5688 ± 0.0840     |
| Lyn’s                 | 0.6032 ± 0.0869     |
| Ours (without Task 1) | _0.6826 ± 0.0578_   |
| **Ours (with Task 1)**| **0.7045 ± 0.0568** |

