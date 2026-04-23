# SAVIC Multimodal

This repository accompanies the manuscript:

**A Two-Stage Multimodal Semantic Reconstruction Framework for Receiver-Side Direct MP4 Recovery**

## 1. Overview

This project studies multimodal semantic reconstruction for receiver-side recovery of synchronized audio-video MP4 output using an **in-house audio-visual model**. The current public repository is positioned as a **companion/artifact repository** for the experimental package and manuscript assets.

The validated workflow is organized in two stages:

- **Stage 1**: reconstruction-capable warm start with emphasis on audio stabilization.
- **Stage 2**: quality-oriented semantic reconstruction with three semantic operating points:
  - `semantic_low`
  - `semantic_mid`
  - `semantic_high`

The repository is intended to support:

- semantic evaluation,
- direct MP4 reconstruction,
- baseline generation and comparison,
- controlled retraining-based ablation,
- manuscript tables, plots, and qualitative figures.

## 2. Scientific Positioning

This work is **not** positioned as a claim of superiority over conventional H.264/H.265 codecs. Instead, it is positioned as a validated proof of concept for reconstruction-capable multimodal semantic communication using a **self-developed model**.

Main validated observations from the current experimental package:

- `semantic_high` is the strongest semantic operating point in the validated Stage-2 results.
- The framework can reconstruct synchronized audio-video into a receiver-side MP4 output.
- Conventional H.264/H.265 baselines remain substantially stronger in bitrate efficiency and low-level fidelity.
- Controlled retraining-based ablation indicates that audio-oriented objectives, especially SI-SDR loss and STFT-based spectral loss, are the most sensitive components for final reconstruction quality.

## 3. Current Public Repository Structure

The public repository currently exposes the following top-level structure:

```text
savic_multimodal/
├── README.md
├── LICENSE
├── CITATION.cff
├── checkpoints/
│   └── README.md
├── data/
│   └── README.md
└── results/
    └── README.md
```

This public layout is intentionally lightweight. Additional private or local working directories such as configs, scripts, notebooks, and large experiment outputs may exist in the full experimental workspace, but they are not assumed to be fully released in the current public snapshot.

## 4. Environment

Recommended baseline environment:

- Python 3.10+
- Linux or Google Colab
- CUDA-capable GPU for training and large-scale reconstruction
- FFmpeg available in the system path

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

If the exact environment differs, document the final tested environment before manuscript submission.

## 5. Data Policy

This repository should not upload non-redistributable raw data if ownership or sharing rights are restricted.

Instead:

- place only shareable sample media in `data/`,
- describe the full dataset preparation workflow in `data/README.md`,
- explain which files are private, institution-owned, or otherwise restricted,
- provide reproduction instructions for users who have legal access to the source data.

For the current manuscript package, `lionel.mp4` is used as the main demo video in the qualitative reconstruction pipeline.

## 6. Quick Start

### 6.1 Clone the repository

```bash
git clone https://github.com/nurwanreza79/savic_multimodal.git
cd savic_multimodal
```

### 6.2 Prepare data

- Put accessible input media under `data/`
- Make sure the demo file is available, for example:

```text
data/lionel.mp4
```

## 7. What should be uploaded next

For a stronger public release, the next recommended uploads are:

1. one or more shareable demo inputs under `data/`,
2. result artifacts under `results/` such as:
   - `compare_semantic_vs_baselines.csv`,
   - manuscript tables,
   - plots,
   - qualitative figures,
   - selected demo MP4 outputs,
3. checkpoint lineage notes or one lightweight/shareable checkpoint under `checkpoints/`.

## 8. Scope and Limitations

The current public repository should be read as a manuscript companion and artifact index. It should not be interpreted as a claim that the full private training workspace, all datasets, or all large checkpoints have already been fully released.

## 9. Citation

If you use this repository, please cite the associated manuscript and repository metadata in `CITATION.cff`.
