# CMB Power Spectrum: Simulation and Analysis with CAMB and healpy

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![Cosmology](https://img.shields.io/badge/Topic-Cosmology-orange)
![CAMB](https://img.shields.io/badge/Code-CAMB-green)
![healpy](https://img.shields.io/badge/Code-healpy-lightgrey)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/YOUR_REPO/blob/main/YOUR_NOTEBOOK.ipynb)

A cosmology data analysis project that computes the **CMB temperature power
spectrum** from ΛCDM parameters using **CAMB**, simulates a random full-sky map
with **healpy**, and studies how **cosmic variance** and the cosmological
parameters shape the spectrum.

---

## Table of Contents
- [Overview](#overview)
- [Background and Theory](#background-and-theory)
  - [The Angular Power Spectrum](#the-angular-power-spectrum)
  - [Acoustic Peaks](#acoustic-peaks)
  - [Cosmic Variance](#cosmic-variance)
- [Methodology](#methodology)
- [Results](#results)
  - [Theory Spectrum](#1-theory-spectrum)
  - [Simulated Sky and Cosmic Variance](#2-simulated-sky-and-cosmic-variance)
  - [Parameter Experiments](#3-parameter-experiments)
- [Conclusions](#conclusions)
- [Getting Started](#getting-started)
- [Repository Structure](#repository-structure)
- [References](#references)

---

## Overview
The Cosmic Microwave Background is the relic light from recombination, about
380,000 years after the Big Bang. Its tiny temperature fluctuations (about one
part in $10^5$) encode the geometry, composition and initial conditions of the
Universe. This project reproduces the standard CMB analysis pipeline end to
end: from cosmological parameters to a theoretical spectrum, to a simulated sky,
to a recovered spectrum with quantified uncertainty.

## Background and Theory

### The Angular Power Spectrum
The temperature map is expanded in spherical harmonics,

$$\frac{\Delta T}{T}(\theta,\phi)=\sum_{\ell m} a_{\ell m}Y_{\ell m}(\theta,\phi),$$

and the power spectrum is the variance of the coefficients,
$C_\ell=\langle |a_{\ell m}|^2\rangle$, with angular scale $\theta\approx180^\circ/\ell$.
The plots show $D_\ell=\ell(\ell+1)C_\ell/2\pi$.

### Acoustic Peaks
Before recombination, photons and baryons formed a plasma that oscillated under
gravity and radiation pressure. The peaks mark modes caught at maximum
compression or rarefaction when the oscillations froze.

| Feature | Approx. location | Physics | Parameter probed |
|---|---|---|---|
| Plateau | ℓ ≲ 30 | Sachs–Wolfe effect | $A_s$, $n_s$ |
| 1st peak | ℓ ≈ 220 | Sound horizon at recombination | Curvature |
| Peak height ratios | ℓ ≈ 220–810 | Baryon loading, dark matter | $\Omega_b h^2$, $\Omega_c h^2$ |
| Damping tail | ℓ ≳ 1000 | Photon diffusion (Silk damping) | Recombination physics |

### Cosmic Variance
With only one sky, each multipole has $2\ell+1$ independent modes, giving an
irreducible fractional uncertainty

$$\frac{\Delta C_\ell}{C_\ell}=\sqrt{\frac{2}{2\ell+1}}.$$

## Methodology
1. **Theory spectrum:** CAMB with Planck-2018-like parameters
   ($H_0=67.4$, $\Omega_b h^2=0.0224$, $\Omega_c h^2=0.120$, $\tau=0.054$,
   $A_s=2.1\times10^{-9}$, $n_s=0.965$).
2. **Sky simulation:** one Gaussian random realization with `healpy.synfast`
   at `nside=512`.
3. **Spectrum recovery:** `healpy.anafast` on the simulated map.
4. **Cosmic variance:** 500 realizations (`synalm` and `alm2cl`) compared with
   the analytic formula.
5. **Parameter experiments:** vary $\Omega_b h^2$, $\Omega_c h^2$ and $H_0$ one at
   a time and track peak positions and heights.

## Results

### 1. Theory Spectrum
![CMB TT power spectrum](images/cmb_spectrum.png)

The first three acoustic peaks sit at $\ell\approx$ 220, 536 and 813,
followed by Silk damping above $\ell\approx1000$.

### 2. Simulated Sky and Cosmic Variance
![Simulated sky map](images/sky_map.png)
![Cosmic variance](images/cosmic_variance.png)

The recovered spectrum scatters around the theory curve, and the scatter
measured over 500 realizations matches $\sqrt{2/(2\ell+1)}$.

### 3. Parameter Experiments
![Parameter experiments](images/parameter_experiments.png)

| Parameter ↑ | Main effect |
|---|---|
| $\Omega_b h^2$ | Odd peaks higher, 2nd peak lower |
| $\Omega_c h^2$ | Overall peak heights lower, especially the 1st |
| $H_0$ | Peaks shift horizontally in $\ell$ |

## Conclusions
- The simulated spectrum reproduces the known features of the CMB: the
  Sachs–Wolfe plateau, acoustic peaks and damping tail.
- A single sky recovers the input spectrum only up to cosmic variance, which
  is largest at low $\ell$.
- Position, height ratios and overall amplitude of the peaks respond to
  different parameters, which is why the CMB constrains cosmology so tightly.

**Limitations:** temperature only (no polarization or lensing); full-sky and
noiseless (no mask, beam or foregrounds); parameters varied one at a time.

**Future work:** polarization and B-modes, realistic masks and noise, and MCMC
fitting of Planck data.

## Getting Started

**Run in Colab:** click the *Open in Colab* badge above, then
*Runtime → Run all*.

**Run locally:**
```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO
pip install camb healpy numpy scipy matplotlib jupyter
jupyter notebook YOUR_NOTEBOOK.ipynb
```

## Repository Structure
```
├── CMBproject.ipynb      # Main analysis notebook
├── images/                  # Figures used in this README
│   ├── cmb_spectrum.png
│   ├── sky_map.png
│   ├── cosmic_variance.png
│   └── parameter_experiments.png
└── README.md
```

## References
1. Planck Collaboration (2020). *Planck 2018 results. VI. Cosmological
   parameters.* A&A, 641, A6. arXiv:1807.06209
2. Lewis, A., Challinor, A., & Lasenby, A. (2000). *Efficient computation of
   CMB anisotropies in closed FRW models.* ApJ, 538, 473. arXiv:astro-ph/9911177
3. Górski, K. M., et al. (2005). *HEALPix.* ApJ, 622, 759. arXiv:astro-ph/0409513
4. Zonca, A., et al. (2019). *healpy.* JOSS, 4(35), 1298.
5. Hu, W., & Dodelson, S. (2002). *Cosmic Microwave Background Anisotropies.*
   ARA&A, 40, 171. arXiv:astro-ph/0110414

Software: [CAMB](https://camb.readthedocs.io) · [healpy](https://healpy.readthedocs.io) ·
Data: [Planck Legacy Archive](https://pla.esac.esa.int) · [LAMBDA](https://lambda.gsfc.nasa.gov)
