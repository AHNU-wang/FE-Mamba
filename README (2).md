# FE-Mamba

**FE-Mamba: Integrated Station-aware Frequency Enhancement and Patch-Mamba Paradigm for Metro Ridership Flow Prediction**

This repository provides the implementation of **FE-Mamba**, a deep learning framework designed for short-term metro ridership flow prediction. The model aims to improve the prediction of passenger flow by jointly modeling sequential temporal dependencies, station-level spatial correlations, and intrinsic periodic patterns in metro ridership data.

## Overview

Short-term metro ridership forecasting is important for intelligent urban rail transit operation, passenger flow management, and service scheduling. Existing graph-based or attention-based methods often depend on predefined graph structures or may struggle to preserve temporal order and periodic dynamics. FE-Mamba addresses these limitations by introducing a graph-free spatio-temporal modeling framework that combines frequency-domain enhancement with multi-scale Patch-Mamba sequence modeling.

The core idea of FE-Mamba is to capture both:

- **Sequential evolution** of metro passenger flow over time;
- **Periodic patterns** such as daily or short-term ridership cycles;
- **Global and local dependencies** among stations and temporal segments;
- **Multi-scale temporal dynamics** at different time resolutions.

## Key Features

### 1. Station-aware Frequency Enhancement

FE-Mamba integrates frequency-domain enhancement based on the Fast Fourier Transform (FFT). This module is designed to capture periodic ridership patterns and strengthen station-specific temporal representations without relying on manually predefined graph structures.

### 2. Patch-Mamba Temporal Modeling

The model adopts a multi-scale Patch-Mamba paradigm to divide historical ridership sequences into temporal patches. This enables the model to capture passenger flow evolution at different temporal granularities, such as 15-minute, 30-minute, or 60-minute intervals.

### 3. Global-local Dual-path Fusion

FE-Mamba uses a dual-path fusion mechanism to combine global spatial dependencies and local sequence dynamics. Through gated recurrent mechanisms, the model learns complementary spatio-temporal representations from different dependency paths.

### 4. Parallel Non-autoregressive Decoding

Instead of generating future time steps one by one, FE-Mamba employs a parallel decoder to predict all future steps simultaneously. This non-autoregressive strategy reduces error accumulation and improves inference efficiency.

### 5. Composite Training Objective

The training objective combines Huber loss and MAPE-based optimization. Huber loss improves robustness to abnormal ridership fluctuations, while MAPE helps balance prediction performance across stations with different passenger flow scales.

## Model Architecture

The overall FE-Mamba framework can be summarized as follows:

```text
Historical Metro Ridership Sequence
        |
        v
Station-aware Frequency Enhancement
        |
        v
Multi-scale Patch Construction
        |
        v
Patch-Mamba Spatio-temporal Modeling
        |
        v
Global-local Dual-path Fusion
        |
        v
Parallel Decoder
        |
        v
Future Metro Ridership Prediction
```

## Recommended Repository Structure

If you are preparing a new public repository, the following structure is recommended:

```text
FE-Mamba/
├── README.md
├── requirements.txt
├── configs/
│   └── fe_mamba.yaml
├── data/
│   └── README.md
├── datasets/
│   └── metro_dataset.py
├── models/
│   ├── fe_mamba.py
│   ├── frequency_enhancement.py
│   ├── patch_mamba.py
│   └── decoder.py
├── scripts/
│   ├── train.sh
│   └── test.sh
├── train.py
├── test.py
└── utils/
    ├── metrics.py
    └── losses.py
```

You can modify this structure according to the actual implementation.

## Installation

Clone the repository:

```bash
git clone https://github.com/your-username/FE-Mamba.git
cd FE-Mamba
```

Create a Python environment:

```bash
conda create -n fe-mamba python=3.8
conda activate fe-mamba
```

Install dependencies:

```bash
pip install -r requirements.txt
```

If your implementation uses Mamba-related packages, install the required Mamba dependencies according to your CUDA and PyTorch versions.

## Data Preparation

Place the processed metro ridership datasets under the `data/` directory. A typical data format is:

```text
data/
├── Beijing/
├── Shanghai/
└── Hangzhou/
```

The expected input tensor format is generally:

```text
X: [B, T_in, N, C]
Y: [B, T_out, N, C]
```

where:

- `B` denotes batch size;
- `T_in` denotes the historical observation length;
- `T_out` denotes the prediction horizon;
- `N` denotes the number of metro stations;
- `C` denotes the number of input features.

Please update the dataset loader according to the specific data preprocessing format used in your project.

## Training

A standard training command can be organized as follows:

```bash
python train.py --config configs/fe_mamba.yaml
```

You can also provide a shell script:

```bash
bash scripts/train.sh
```

Important configurable options may include:

```yaml
dataset: Beijing
input_len: 12
pred_len: 12
batch_size: 64
learning_rate: 0.001
hidden_dim: 64
num_layers: 2
patch_sizes: [3, 6, 12]
loss: huber_mape
```

Please adjust these settings according to your dataset and experimental protocol.

## Evaluation

After training, evaluate the model with:

```bash
python test.py --config configs/fe_mamba.yaml --checkpoint checkpoints/best_model.pth
```

Common evaluation metrics for metro ridership forecasting include:

- MAE: Mean Absolute Error;
- RMSE: Root Mean Squared Error;
- MAPE: Mean Absolute Percentage Error.

## Acknowledgements

This project is inspired by the SDT-GRU framework proposed in the following work:

```bibtex
@article{YANG2024124431,
  title = {Are Graphs and GCNs necessary for short-term metro ridership forecasting?},
  journal = {Expert Systems with Applications},
  volume = {254},
  pages = {124431},
  year = {2024},
  issn = {0957-4174},
  doi = {https://doi.org/10.1016/j.eswa.2024.124431},
  url = {https://www.sciencedirect.com/science/article/pii/S0957417424012971},
  author = {Qiong Yang and Xianghua Xu and Zihang Wang and Juan Yu and Xiaodong Hu},
  keywords = {Metro ridership prediction, Transformer encoder, GRU, Encoder-decoder architecture, Graph convolutional networks}
}
```

We sincerely thank the authors for their valuable contribution to short-term metro ridership forecasting.

## Citation

If you find this repository useful for your research, please consider citing the FE-Mamba paper once the official citation information is available:

```bibtex
@article{FEMamba,
  title = {FE-Mamba: Integrated Station-aware Frequency Enhancement and Patch-Mamba Paradigm for Metro Ridership Flow Prediction},
  author = {Author Names},
  journal = {Journal Name},
  year = {Year}
}
```

## License

Please add an appropriate open-source license before releasing this repository publicly.
