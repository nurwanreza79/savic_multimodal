# SAVIC Multimodal

This repository accompanies the manuscript:

**A Two-Stage Multimodal Semantic Reconstruction Framework for Receiver-Side Direct MP4 Recovery**

## 1. Overview

This project studies **multimodal semantic reconstruction** for receiver-side recovery of synchronized **audio-video MP4 output** using an **in-house audio-visual model**. The pipeline is organized in two stages:

- **Stage 1**: reconstruction-capable warm start with emphasis on audio stabilization.
- **Stage 2**: quality-oriented semantic reconstruction with three semantic operating points:
  - `semantic_low`
  - `semantic_mid`
  - `semantic_high`

The repository is intended to support:

- model training and resumption,
- semantic evaluation,
- direct MP4 reconstruction,
- baseline generation and evaluation,
- controlled retraining-based ablation,
- generation of manuscript tables, plots, and qualitative figures.

## 2. Scientific Positioning

This work is **not** positioned as a claim of superiority over conventional H.264/H.265 codecs. Instead, it is positioned as a **validated proof of concept** for reconstruction-capable multimodal semantic communication using a self-developed model.

Main validated observations from the current experimental package:

- `semantic_high` is the strongest semantic operating point in the validated Stage-2 results.
- The framework can reconstruct synchronized audio-video into a receiver-side MP4 output.
- Conventional H.264/H.265 baselines still remain substantially stronger in bitrate efficiency and low-level fidelity.
- Controlled retraining-based ablation shows that **audio-oriented objectives**, especially **SI-SDR loss** and **STFT-based spectral loss**, are the most sensitive components for final reconstruction quality.

## 3. Recommended Repository Layout

The repository should be organized as follows:

```text
savic_multimodal/
├── README.md
├── LICENSE
├── .gitignore
├── CITATION.cff
├── requirements.txt
├── dataset/
│   └── README.md
├── configs/
│   ├── train_recon_stage1.yaml
│   ├── train_recon_stage1_audiofix.yaml
│   ├── train_recon_stage2.yaml
│   ├── train_recon_stage2_qualityfix_v3.yaml
│   └── ablation_stage2.yaml
├── scripts/
│   ├── train_recon.py
│   ├── train_recon_stage2.py
│   ├── evaluate_semantic_recon.py
│   ├── reconstruct_video_recon.py
│   ├── evaluate_baselines_recon.py
│   ├── compare_recon_vs_baselines.py
│   ├── run_ablation.py
│   ├── generate_paper_results.py
│   ├── plots.py
│   ├── export_qualitative_frames.py
│   └── export_qualitative_audio.py
├── baselines/
│   └── run_ffmpeg_baselines.py
├── checkpoints_recon_stage1/
│   └── README.md
├── checkpoints_recon_stage1_audiofix/
│   └── README.md
├── checkpoints_recon_stage2/
│   └── README.md
├── checkpoints_recon_stage2_qualityfix_v3/
│   └── README.md
├── experiments_recon_stage1/
├── experiments_recon_stage1_audiofix/
├── experiments_recon_stage2/
├── experiments_recon_stage2_qualityfix_v3/
├── experiments_recon_stage2_ablation_v3/
│   ├── summary_ablation.csv
│   └── ablation_progress.json
├── paper_tables/
├── paper_figures/
├── plots/
└── notebooks/
    └── savic_multimodal.ipynb
```

## 4. Environment

Recommended baseline environment:

- Python 3.10+
- Linux or Google Colab
- CUDA-capable GPU for training and large-scale reconstruction
- FFmpeg available in system path

Typical Python packages observed from the project notebook and artifact-generation workflow include:

- `torch`
- `torchaudio`
- `torchvision`
- `numpy`
- `pandas`
- `scipy`
- `matplotlib`
- `opencv-python`
- `librosa`
- `soundfile`
- `PyYAML`
- `tqdm`
- `scikit-image`
- `imageio`
- `imageio-ffmpeg`

If your exact environment differs, please document the final tested environment in `requirements.txt` or `environment.yml` before submission.

## 5. Data Policy

This repository should **not** upload nonredistributable raw data if ownership or sharing rights are restricted.

Instead:

- place only shareable sample media in `dataset/`,
- describe the full dataset preparation workflow in `dataset/README.md`,
- explain which files are private, institution-owned, or otherwise restricted,
- provide reproduction instructions for users who have legal access to the source data.

For the current manuscript package, `lionel.mp4` is used as the main demo video in the qualitative pipeline.

## 6. Quick Start

### 6.1 Clone the repository

```bash
git clone https://github.com/nurwanreza79/savic_multimodal
cd savic_multimodal
```

### 6.2 Install dependencies

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 6.3 Prepare data

- Put accessible input media under `dataset/`
- Make sure the demo file is available, for example:

```text
dataset/lionel.mp4
```

## 7. Training and Evaluation Workflow

The commands below follow the structure used in the project notebook.

### 7.1 Stage 1 training

Train Stage 1 from scratch:

```bash
python -u -m scripts.train_recon --config configs/train_recon_stage1.yaml
```

Resume Stage 1 from a step checkpoint:

```bash
python -u -m scripts.train_recon \
  --config configs/train_recon_stage1.yaml \
  --resume checkpoints_recon_stage1/latest_step.pt
```

Resume Stage 1 from the latest epoch checkpoint:

```bash
python -u -m scripts.train_recon \
  --config configs/train_recon_stage1.yaml \
  --resume checkpoints_recon_stage1/latest.pt
```

### 7.2 Stage 1 semantic evaluation

```bash
python -u -m scripts.evaluate_semantic_recon \
  --config configs/train_recon_stage1.yaml \
  --checkpoint checkpoints_recon_stage1/best.pt \
  --out_dir experiments_recon_stage1/eval_best_final
```

### 7.3 Stage 1 full MP4 reconstruction

```bash
python -u -m scripts.reconstruct_video_recon \
  --config configs/train_recon_stage1.yaml \
  --checkpoint checkpoints_recon_stage1/best.pt \
  --video dataset/lionel.mp4 \
  --op semantic_high \
  --out_dir experiments_recon_stage1/reconstruction_best_final
```

### 7.4 Stage 1 audio-fix refinement

```bash
python -u -m scripts.train_recon \
  --config configs/train_recon_stage1_audiofix.yaml \
  --resume checkpoints_recon_stage1/best.pt
```

Evaluate the refined Stage 1 model:

```bash
python -u -m scripts.evaluate_semantic_recon \
  --config configs/train_recon_stage1_audiofix.yaml \
  --checkpoint checkpoints_recon_stage1_audiofix/best.pt \
  --out_dir experiments_recon_stage1_audiofix/eval_best_final
```

Reconstruct MP4 with the refined Stage 1 model:

```bash
python -u -m scripts.reconstruct_video_recon \
  --config configs/train_recon_stage1_audiofix.yaml \
  --checkpoint checkpoints_recon_stage1_audiofix/best.pt \
  --video dataset/lionel.mp4 \
  --op semantic_high \
  --out_dir experiments_recon_stage1_audiofix/reconstruction_best_final
```

### 7.5 Stage 2 training

Train Stage 2 from the best Stage 1 audio-fix checkpoint:

```bash
python -u -m scripts.train_recon \
  --config configs/train_recon_stage2.yaml \
  --resume checkpoints_recon_stage1_audiofix/best.pt
```

Resume Stage 2 if runtime disconnects:

```bash
python -u -m scripts.train_recon \
  --config configs/train_recon_stage2.yaml \
  --resume checkpoints_recon_stage2/latest_step.pt
```

### 7.6 Stage 2 semantic evaluation

```bash
python -u -m scripts.evaluate_semantic_recon \
  --config configs/train_recon_stage2.yaml \
  --checkpoint checkpoints_recon_stage2/latest.pt \
  --out_dir experiments_recon_stage2/semantic_eval
```

### 7.7 Stage 2 direct MP4 reconstruction

```bash
python -u -m scripts.reconstruct_video_recon \
  --config configs/train_recon_stage2.yaml \
  --checkpoint checkpoints_recon_stage2/epoch_007.pt \
  --video dataset/lionel.mp4 \
  --op semantic_high \
  --out_dir experiments_recon_stage2/reconstruction_high
```

### 7.8 Quality-fix Stage 2 training (validated main recipe line)

Train or continue the quality-fix Stage-2 recipe:

```bash
python -u -m scripts.train_recon_stage2 \
  --config configs/train_recon_stage2_qualityfix_v3.yaml \
  --resume checkpoints_recon_stage2/latest.pt \
  --resume_mode weights_only
```

Resume from latest full-state checkpoint:

```bash
python -u -m scripts.train_recon_stage2 \
  --config configs/train_recon_stage2_qualityfix_v3.yaml \
  --resume checkpoints_recon_stage2_qualityfix_v3/latest.pt \
  --resume_mode full_state
```

Resume from latest step checkpoint:

```bash
python -u -m scripts.train_recon_stage2 \
  --config configs/train_recon_stage2_qualityfix_v3.yaml \
  --resume checkpoints_recon_stage2_qualityfix_v3/latest_step.pt \
  --resume_mode full_state
```

### 7.9 Validated main Stage-2 evaluation

Semantic evaluation:

```bash
python -u -m scripts.evaluate_semantic_recon \
  --config configs/train_recon_stage2_qualityfix_v3.yaml \
  --checkpoint checkpoints_recon_stage2_qualityfix_v3/best.pt \
  --out_dir experiments_recon_stage2_qualityfix_v3/semantic_eval
```

Direct MP4 reconstruction:

```bash
python -u -m scripts.reconstruct_video_recon \
  --config configs/train_recon_stage2_qualityfix_v3.yaml \
  --checkpoint checkpoints_recon_stage2_qualityfix_v3/best.pt \
  --video dataset/lionel.mp4 \
  --op semantic_high \
  --out_dir experiments_recon_stage2_qualityfix_v3/reconstruction_high \
  --crf 18
```

### 7.10 Baseline generation and evaluation

Generate FFmpeg baselines:

```bash
python -u baselines/run_ffmpeg_baselines.py \
  --config configs/train_recon_stage2_qualityfix_v3.yaml \
  --out_dir experiments_recon_stage2_qualityfix_v3/baselines
```

Evaluate baselines:

```bash
CUDA_VISIBLE_DEVICES="" python -u -m scripts.evaluate_baselines_recon \
  --config configs/train_recon_stage2_qualityfix_v3.yaml \
  --baseline_dir experiments_recon_stage2_qualityfix_v3/baselines \
  --out_dir experiments_recon_stage2_qualityfix_v3/baseline_eval \
  --resume
```

Compare semantic vs baselines:

```bash
python -u -m scripts.compare_recon_vs_baselines \
  --semantic_summary experiments_recon_stage2_qualityfix_v3/semantic_eval/summary_semantic.csv \
  --baseline_summary experiments_recon_stage2_qualityfix_v3/baseline_eval/summary_baselines.csv \
  --out_csv experiments_recon_stage2_qualityfix_v3/compare_semantic_vs_baselines.csv
```

### 7.11 Controlled retraining-based ablation

```bash
python -u -m scripts.run_ablation \
  --config configs/ablation_stage2.yaml \
  --out_dir experiments_recon_stage2_qualityfix_v3/ablation
```

### 7.12 Generate manuscript tables and plots

Generate paper tables:

```bash
python -u -m scripts.generate_paper_results \
  --semantic_summary experiments_recon_stage2_qualityfix_v3/semantic_eval/summary_semantic.csv \
  --baseline_summary experiments_recon_stage2_qualityfix_v3/baseline_eval/summary_baselines.csv \
  --ablation_summary experiments_recon_stage2_ablation_v3/summary_ablation.csv \
  --out_dir experiments_recon_stage2_qualityfix_v3/paper_tables
```

Generate plots:

```bash
python -u -m scripts.plots \
  --compare_csv experiments_recon_stage2_qualityfix_v3/compare_semantic_vs_baselines.csv \
  --ablation_csv experiments_recon_stage2_ablation_v3/summary_ablation.csv \
  --out_dir experiments_recon_stage2_qualityfix_v3/plots
```

### 7.13 Export qualitative figures for the manuscript

Visual qualitative figure:

```bash
python -u -m scripts.export_qualitative_frames \
  --original_video dataset/lionel.mp4 \
  --semantic_video experiments_recon_stage2_qualityfix_v3/reconstruction_high/lionel_semantic_high_full.mp4 \
  --baseline_dir experiments_recon_stage2_qualityfix_v3/baselines \
  --baseline_summary experiments_recon_stage2_qualityfix_v3/baseline_eval/summary_baselines.csv \
  --semantic_summary experiments_recon_stage2_qualityfix_v3/semantic_eval/summary_semantic.csv \
  --setting_name semantic_high \
  --num_samples 6 \
  --out_dir experiments_recon_stage2_qualityfix_v3/paper_figures/qualitative_frames
```

Audio qualitative figure:

```bash
python -u -m scripts.export_qualitative_audio \
  --original_video dataset/lionel.mp4 \
  --semantic_video experiments_recon_stage2_qualityfix_v3/reconstruction_high/lionel_semantic_high_full.mp4 \
  --baseline_dir experiments_recon_stage2_qualityfix_v3/baselines \
  --baseline_summary experiments_recon_stage2_qualityfix_v3/baseline_eval/summary_baselines.csv \
  --semantic_summary experiments_recon_stage2_qualityfix_v3/semantic_eval/summary_semantic.csv \
  --setting_name semantic_high \
  --start_sec 10 \
  --duration_sec 3 \
  --out_dir experiments_recon_stage2_qualityfix_v3/paper_figures/qualitative_audio
```

## 8. Expected Key Artifacts

The following outputs are expected from the validated pipeline:

- `summary_semantic.csv`
- `summary_baselines.csv`
- `compare_semantic_vs_baselines.csv`
- `summary_ablation.csv`
- manuscript-ready tables in `paper_tables/`
- plots in `plots/`
- qualitative frame and audio figures in `paper_figures/`
- reconstructed MP4 output such as `lionel_semantic_high_full.mp4`

## 9. Reproducibility Notes for Submission

Before citing this repository in a journal submission, make sure that:

1. the branch used for submission is stable,
2. the README matches the exact experiment package used in the manuscript,
3. the repository contains the exact config files referenced by the paper,
4. the result CSV files used in the paper are included,
5. the repository distinguishes clearly between shareable artifacts and restricted data,
6. a release tag is created for the submission version, for example `v1.0.0-submission`.

## 10. Limitations

The current artifact package demonstrates a reconstruction-capable semantic framework, but the results should be interpreted carefully:

- the semantic model is not yet bitrate-efficient relative to H.264/H.265,
- qualitative comparisons are representative rather than strictly bitrate-matched,
- audio quality remains the most sensitive part of the framework,
- broader generalization should be studied with more extensive datasets and stronger subjective evaluation.

## 11. Citation

If you use this repository, please cite the associated manuscript.

A ready-to-update citation file is provided in `CITATION.cff`.

## 12. Contact
Nurwan Reza Fachrurrozi
Email: nurwan.reza@ui.ac.id
nurwwanreza79@gmail.com
For academic correspondence related to the manuscript, please use the corresponding author information reported in the paper.
