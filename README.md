# <center> The Double Dilemma in Multi-Task Radiology Report Generation: A Gradient Dynamics Analysis and Solution

<div align="center">


</div>



<div align="center">
  <img src="Image.png" alt="CAME-Grad Framework" width="1000">
</div>

While multi-task learning based automatic radiology report generation (RRG) is widely adopted to ensure clinical consistency, most focus on architectural designs yet remain limited to coarse linear scalarization strategies. These strategies can not effectively balance the hard constraints of discriminative clinical supervision with the smoothness requirements of report generation. To address these problems, we analyze the failure mechanism of linear scalarization from the perspective of gradient dynamics for the first time, utilizing the stochastic differential equation (SDE) framework to formalize it as a "Double Dilemma" of drift term deviation and diffusion term decay. Based on this, we propose a backbone-agnostic optimizer named **C**onflict-**A**verse **M**agnitude-**E**nhanced **Grad**ient Descent (CAME-Grad). Through conflict-averse direction rectification and magnitude-enhanced energy injection, the algorithm not only ensures geometric validity, but also avoids the local optimal solution. Then, the adaptive gradient fusion technology is used to establish a dynamic balance between the theoretical optimal direction and the task-specific inductive bias. Experiments show that as a universal plug-and-play optimizer, CAME-Grad brings substantial and consistent improvements across eight diverse RRG methods, elevating overall clinical efficacy performance by an average of 2.3\% on MIMIC-CXR and 1.9\% on IU X-Ray.
## TODO
Our complete codebase is released for anonymous review.

- [x] [2026.1.29]: The training and inference code has been released.
- [x] [2026.1.29]: The CAME-Grad optimizer implementation is available.
- [x] The configuration and evaluation scripts are released.
- [ ] Release pre-trained checkpoints (Upon acceptance).

## Getting Started

### 1. Install Environment

```bash
conda create -n came_grad python=3.8
conda activate came_grad
pip install -r requirements.txt
```

### 2. Data Preparation

We use two standard datasets: **MIMIC-CXR** and **IU X-Ray**.

- **MIMIC-CXR**: Please request access from [PhysioNet](https://physionet.org/content/mimic-cxr/2.0.0/).
- **IU X-Ray**: Available from [OpenI](https://openi.nlm.nih.gov/).

### 3. Directory Structure

Please organize your data as follows:

```
data
├── mimic_cxr
│   ├── images               # Raw images
│   ├── annotation.json      # Original annotations
│   └── mimic_cxr.json       # Preprocessed for this codebase
├── iu_xray
│   ├── images
│   └── annotation.json
└── ...
```

## Training

You can train the model using `main_train.py`. The **CAME-Grad** optimizer is enabled via the `--use_came_grad` flag.

### Training with CAME-Grad (Ours)

```bash
python main_train.py \
    --dataset_name mimic_cxr \
    --image_dir data/mimic_cxr/images/ \
    --ann_path data/mimic_cxr/mimic_cxr_ddatr.json \
    --batch_size 16 \
    --save_dir results/mimic_cxr_came_grad \
    --cls_weight 4.0 \
    --use_came_grad \
    --came_rho 0.5 \
    --came_kappa 1.5 \
    --came_nu 0.2 
```

### Training Baseline (Linear Scalarization)

```bash
python main_train.py \
    --dataset_name mimic_cxr \
    --image_dir data/mimic_cxr/images/ \
    --ann_path data/mimic_cxr/mimic_cxr_ddatr.json \
    --batch_size 16 \
    --save_dir results/mimic_cxr_baseline \
    --cls_weight 4.0
```

## Evaluation

To evaluate the trained model on the test set, run `main_test.py`:

```bash
python main_test.py \
    --dataset_name mimic_cxr \
    --image_dir data/mimic_cxr/images/ \
    --ann_path data/mimic_cxr/mimic_cxr_ddatr.json \
    --load_pretrained results/mimic_cxr_came_grad/model_best.pth \
    --batch_size 32
```

## License

This project is licensed under the Apache-2.0 License.
