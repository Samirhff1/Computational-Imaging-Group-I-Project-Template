# Super-Resolution on LSUN Church: A Comparative Analysis

**Computational Imaging - Group I**
- Mohamed Samir Haffoudhi (mohamed.haffoudhi@studio.unibo.it)
- Mattia Furini (mattia.furini@studio.unibo.it)

## Overview
This project tackles the inverse problem of Image Super-Resolution: reconstructing High-Resolution (HR) images from Low-Resolution (LR) degraded observations. The system is formalized as `y = Hx + e`, where `H` represents degradation (Gaussian blur and downsampling) and `e` represents Additive White Gaussian Noise (AWGN).

We compare three distinct approaches using different *priors*:
1. **End-to-End Learning (UNet):** Supervised mapping from low-res to high-res.
2. **Variational Method (Total Variation - TV):** Optimization enforcing gradient sparsity.
3. **Generative Model (DiffPIR):** Diffusion model as a generative prior for hallucinating details.

## Dataset & Pipeline
- **Dataset:** LSUN Church (4000 RGB images).
- **Preprocessing:** Resized to 256x256, normalized in `[0, 1]`.
- **Splits:** 3200 Train | 400 Val | 400 Test.
- **Degradation Scenarios:** 4 scenarios (Super-Resolution factors `{2, 4}`, Noise levels `{0.005, 0.01}`).

## Methods
### 1. End-to-End (UNet)
- **Architecture:** Encoder-Decoder with Skip Connections.
- **Setup:** L1 Loss, Adam optimizer, 50 Epochs.
- **Pros:** Excellent noise removal and metric performance (highest PSNR/SSIM). Highly robust across different degradation levels.
- **Cons:** Tends to over-smooth textures due to L1 loss minimizing conditional mean.

### 2. Variational (Total Variation)
- **Mechanism:** Enforces data consistency with a prior promoting sparse spatial gradients using a FISTA solver.
- **Pros:** Solid mathematical guarantees; preserves macro-edges.
- **Cons:** Suffers from a "watercolor" effect and staircase artifacts, making it unsuitable for natural textures.

### 3. Generative (DiffPIR)
- **Mechanism:** Uses an unconditional Diffusion Model (`ddpm-ema-church-256`) as a generative prior, interleaving DDIM denoising steps with data-fidelity guidance.
- **Pros:** Recreates highly realistic textures and images.
- **Cons:** Prone to hallucinations (invents non-existent structures) and grid artifacts under severe degradation.

## Contents
- `notebook_CI.ipynb`: Jupyter notebook containing the code for the experiments, models, and evaluation.
- `presentazione.md` / `presentazione.pptx`: Presentation slides detailing the problem, methods, and evaluation results.
- `Group I.pdf`: Final project report.
- `immagini presentazione/`: Directory containing images used in the presentation and results visualizations.

## License
This project includes a `LICENSE` file. Please refer to it for more details.
