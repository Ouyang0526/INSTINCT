<div align="center">

# INSTINCT

**Instance-Level Interaction Architecture for Query-Based Collaborative Perception**

**[ICCV 2025](https://openaccess.thecvf.com/content/ICCV2025/html/Xu_INSTINCT_Instance-Level_Interaction_Architecture_for_Query-Based_Collaborative_Perception_ICCV_2025_paper.html) | [arXiv](https://arxiv.org/abs/2509.23700)**

[Yunjiang Xu](https://github.com/CrazyShout), Lingzhi Li, Jin Wang, Yupeng Ouyang, Benyuan Yang

[![Paper](https://img.shields.io/badge/Paper-ICCV%202025-red)](https://openaccess.thecvf.com/content/ICCV2025/html/Xu_INSTINCT_Instance-Level_Interaction_Architecture_for_Query-Based_Collaborative_Perception_ICCV_2025_paper.html)
[![arXiv](https://img.shields.io/badge/arXiv-2509.23700-b31b1b)](https://arxiv.org/abs/2509.23700)
[![License](https://img.shields.io/badge/License-MIT-blue)](LICENSE)

---

</div>

## Overview

INSTINCT is a collaborative 3D object detection framework for autonomous driving that achieves **state-of-the-art accuracy** while requiring only **1/281 to 1/264** of the communication bandwidth compared to existing methods. It introduces instance-level query-based interaction between cooperative agents (vehicles and infrastructure), overcoming the limitations of single-vehicle perception in long-range detection and occlusion scenarios.

<p align="center">
  <img src="images/INSTINCT.jpg" alt="INSTINCT Architecture" width="100%">
</p>

## News

- **[2025.07]** INSTINCT is accepted by ICCV 2025.
- **[2025.05]** Code and configs are released.

## Key Contributions

INSTINCT features three core components:

1. **Quality-Aware Filtering (QAF)** — A learned filtering mechanism that selects high-quality instance features before transmission, suppressing noisy or redundant queries to reduce bandwidth and improve robustness.

2. **Dual-Branch Detection Routing (DDR)** — Decouples collaboration-irrelevant instances (detectable by a single agent) from collaboration-relevant instances (requiring cross-agent information), routing them through separate detection pathways for more effective collaboration.

3. **Cross-Agent Local Instance Fusion (CALIF)** — Aggregates local hybrid instance features from multiple agents using two sub-modules:
   - **Cross-Domain Adaption (CDA)**: Aligns feature distributions between ego and cooperative agents.
   - **Gaussian Distance Attention (GDA)**: Models spatial relationships with Gaussian-weighted distance attention for geometrically-aware fusion.

Additionally, an enhanced **GT Sampling** technique is introduced to facilitate training with diverse hybrid instance features.

## Main Results

### Comparison with State-of-the-Art

| Method | Fusion Type | DAIR-V2X AP<sub>50</sub> / AP<sub>70</sub> | V2XSet AP<sub>50</sub> / AP<sub>70</sub> | V2V4Real AP<sub>50</sub> / AP<sub>70</sub> | Comm (log<sub>2</sub> B) |
|--------|-------------|---------------------------------------------|------------------------------------------|---------------------------------------------|------------------------|
| No Fusion | — | 63.49 / 49.62 | 65.15 / 52.04 | 39.80 / 22.00 | 0 |
| Late Fusion | — | 60.43 / 37.46 | 78.76 / 68.01 | 55.00 / 26.70 | 8.39 / 9.60 / 9.79 |
| V2VNet | Intermediate | 63.44 / 42.27 | 82.74 / 65.82 | 64.70 / 33.60 | 24.62 |
| V2XViT | Intermediate | 73.49 / 56.75 | 84.31 / 70.18 | 64.90 / 36.90 | 24.62 |
| DiscoNet | Intermediate | 74.69 / 59.20 | 87.78 / 72.07 | 64.12 / 34.51 | 24.62 |
| CoBEVT | Intermediate | 68.25 / 57.87 | 84.54 / 71.19 | 55.56 / 27.08 | 24.62 |
| Where2comm | Intermediate | 79.01 / 66.49 | 92.59 / 84.92 | 70.21 / 38.01 | 21.72 |
| CoAlign | Intermediate | 77.97 / 65.47 | 92.93 / 84.66 | 72.09 / 46.56 | 24.62 |
| **INSTINCT (Ours)** | **Instance** | **81.91 / 75.29** | **92.29 / 87.31** | **80.88 / 61.96** | **13.58** |

> INSTINCT achieves **13.23% relative improvement** on DAIR-V2X (AP<sub>70</sub>) and **33.08% relative improvement** on V2V4Real (AP<sub>50</sub>) over the previous best intermediate fusion method (CoAlign), while requiring **~10× less** communication bandwidth.

<details>
<summary><b>Robustness to Pose Noise (DAIR-V2X)</b></summary>

| Pose Noise (σ<sub>t</sub>, σ<sub>r</sub>) | V2VNet | V2XViT | DiscoNet | Where2comm | CoAlign | **INSTINCT** |
|---------------------------------------------|--------|--------|----------|------------|---------|-------------|
| **AP@0.5** | | | | | | |
| (0.0, 0.0) | 65.72 | 73.49 | 74.69 | 79.01 | 77.97 | **81.91** |
| (0.1, 0.1) | 64.76 | 73.13 | 74.36 | 78.47 | 77.58 | **81.60** |
| (0.2, 0.2) | 63.36 | 71.90 | 73.26 | 76.19 | 75.08 | **77.82** |
| (0.3, 0.3) | 61.44 | 70.21 | 72.05 | 73.01 | 72.20 | **73.27** |
| (0.4, 0.4) | 59.74 | 69.14 | 70.71 | 70.40 | 69.89 | **71.44** |
| **AP@0.7** | | | | | | |
| (0.0, 0.0) | 49.82 | 56.75 | 59.20 | 66.49 | 65.47 | **75.29** |
| (0.1, 0.1) | 48.79 | 56.42 | 58.91 | 64.03 | 63.27 | **72.38** |
| (0.2, 0.2) | 47.05 | 55.50 | 58.12 | 60.21 | 59.33 | **64.51** |
| (0.3, 0.3) | 46.24 | 54.45 | 57.44 | 57.18 | 57.27 | **61.05** |
| (0.4, 0.4) | 45.12 | 53.63 | 56.80 | 55.87 | 56.04 | **59.85** |

> INSTINCT maintains the highest robustness under localization noise, outperforming all methods at every noise level on the DAIR-V2X dataset.

</details>

<details>
<summary><b>Ablation Study (DAIR-V2X)</b></summary>

| QAF | DDR | CDA | GDA | GT Sampling | AP<sub>50</sub> / AP<sub>70</sub> | Comm (log<sub>2</sub> B) |
|-----|-----|-----|-----|-------------|----------------------------------|------------------------|
| | | | | | 73.02 / 59.78 | 17.67 |
| ✓ | | | | | 69.62 / 60.41 | 13.58 |
| ✓ | ✓ | | | | 71.98 / 63.21 | 13.58 |
| ✓ | ✓ | ✓ | | | 78.98 / 71.01 | 13.58 |
| ✓ | ✓ | ✓ | ✓ | | 81.09 / 73.94 | 13.58 |
| ✓ | ✓ | ✓ | ✓ | ✓ | **81.91 / 75.29** | **13.58** |

</details>

## Installation

### Prerequisites

- Python >= 3.7
- PyTorch >= 1.12
- CUDA >= 11.6
- [spconv](https://github.com/traveller59/spconv) >= 2.x

### Step 1: Clone the Repository

```bash
git clone https://github.com/CrazyShout/INSTINCT.git
cd INSTINCT
```

### Step 2: Create Conda Environment

```bash
conda create -n instinct python=3.7.16
conda activate instinct
```

### Step 3: Install PyTorch and spconv

```bash
# PyTorch 1.12 + CUDA 11.6
pip install torch==1.12.0 torchvision==0.13.0 torchaudio==0.12.0 --index-url https://download.pytorch.org/whl/cu116

# spconv
pip install spconv-cu116
```

### Step 4: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 5: Build CUDA Extensions

```bash
# Install the opencood package
python setup.py develop

# Build CUDA kernels (GPU required)
python opencood/utils/setup.py build_ext --inplace
python opencood/pcdet_utils/setup.py build_ext --inplace
```

> **Build Troubleshooting:** If you encounter `THC/THC.h` errors during pcdet_utils compilation (common with PyTorch >= 2.0), comment out the following lines in the `.cpp` files under `opencood/pcdet_utils/pointnet2/`, `roiaware_pool3d/`, and `iou3d_nms/`:
> ```cpp
> // #include <THC/THC.h>
> // extern THCState *state;
> ```

### Step 6: Install OpenPCDet

```bash
cd ..
git clone https://github.com/open-mmlab/OpenPCDet.git
cd OpenPCDet
python setup.py develop
cd ../INSTINCT
```

<details>
<summary><b>Setup for Modern GPUs (RTX 50-series, CUDA 12.x)</b></summary>

For newer GPUs (e.g., RTX 5080 with sm_120), use PyTorch nightly with CUDA 12.8:

```bash
conda create -n instinct python=3.10
conda activate instinct

# PyTorch nightly for sm_120 support
pip install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu128

# spconv
pip install spconv-cu120

# Use compatible numba (0.48.0 is incompatible with Python 3.10)
pip install numba
pip install --no-deps -r requirements.txt
```

Then follow Steps 5–6 as above.

</details>

## Data Preparation

The data preparation follows the same procedure as [CoAlign](https://github.com/yifanlu0227/CoAlign) and [OpenCOOD](https://opencood.readthedocs.io/en/latest/md_files/data_intro.html).

For the **DAIR-V2X** dataset, please use the [supplemented annotations](https://siheng-chen.github.io/dataset/dair-v2x-c-complemented/) provided by CoAlign.

## Getting Started

### Training

```bash
python opencood/tools/train_simple.py -y dairv2x opencood/hypes_yaml/dairv2x/lidar_only_with_noise/second_CQCPInstance_onecycle.yaml
```

The `-y` flag specifies the dataset name. Available options: `dairv2x`, `opv2v`, `v2v4real`, `v2xsim`, `v2xset`.

Configs are organized by dataset under `opencood/hypes_yaml/<dataset>/`.

### Inference

```bash
python opencood/tools/inference_simple.py --model_dir opencood/logs/<experiment_dir>
```

## Checkpoints

Pre-trained model weights will be released soon. Stay tuned!

## Code Structure

```
INSTINCT/
├── opencood/
│   ├── models/
│   │   ├── second_boxattention_cqcp.py      # Main model entry
│   │   ├── comm_modules/
│   │   │   └── CQCP_head_instance.py        # INSTINCT detection head (QAF + DDR + CALIF)
│   │   └── sub_modules/
│   │       ├── CQCP_transformer.py          # Encoder-decoder transformer
│   │       └── matcher.py                   # Hungarian matching
│   ├── data_utils/datasets/
│   │   └── intermediate_v2_fusion_dataset.py # Data pipeline for INSTINCT
│   ├── hypes_yaml/                          # YAML configs per dataset
│   ├── pcdet_utils/                         # Custom CUDA kernels
│   ├── tools/                               # Training & inference scripts
│   └── visualization/                       # Visualization utilities
├── images/                                  # Demo and architecture figures
└── setup.py
```

## Citation

If you find INSTINCT useful for your research, please consider citing:

```bibtex
@inproceedings{xu2025instinct,
  title={INSTINCT: Instance-Level Interaction Architecture for Query-Based Collaborative Perception},
  author={Xu, Yunjiang and Li, Lingzhi and Wang, Jin and Ouyang, Yupeng and Yang, Benyuan},
  booktitle={Proceedings of the IEEE/CVF International Conference on Computer Vision},
  pages={25464--25473},
  year={2025}
}
```

## Acknowledgements

This project is built upon the excellent collaborative perception codebases:
- [OpenCOOD](https://github.com/DerrickXuNu/OpenCOOD)
- [CoAlign](https://github.com/yifanlu0227/CoAlign)
- [ConQueR](https://github.com/V2AI/EFG)
- [OpenPCDet](https://github.com/open-mmlab/OpenPCDet)
